# Power Automate — SharePoint file upload (Custom GPT action)

Use the connected OpenAPI action (e.g. **Upload file to SharePoint via Power Automate** / `uploadFileToSharePointViaFlow`) when the user asks to **save**, **upload**, or **put** a generated report (especially the **Excel workbook**) on **SharePoint**.

## How the flow expects the body

The flow’s **Create file** step maps JSON from the HTTP trigger to SharePoint:

| SharePoint field | Source in request body |
|------------------|-------------------------|
| Site Address | `siteUrl` |
| Folder Path | `folderPath` |
| File Name | `fileName` |
| File Content | `base64ToBinary(triggerBody()?['fileContentBase64'])` |

So the trigger receives **one JSON object**. The file bytes must be sent as a **Base64 string** in the property named exactly **`fileContentBase64`**. The flow decodes that string to binary before writing the file.

### Query string vs body (common failure)

**`api-version`, `sp`, `sv`, and `sig` are not body fields.** They belong **only** in the **request URL** as query parameters (the Custom GPT Action adds them when it calls the operation; `requests.post` uses them on `FLOW_URL`).

**Wrong (do not do this):** one JSON object that mixes URL parameters with file fields, for example:

```json
{
  "api-version": "1",
  "sp": "/triggers/manual/run",
  "sv": "1.0",
  "sig": "...",
  "siteUrl": "...",
  "folderPath": "...",
  "fileName": "...",
  "fileContentBase64": "..."
}
```

That can cause **400 / schema validation errors** on the Power Automate **When an HTTP request is received** trigger if the trigger’s JSON schema allows only the file/site properties, or it can confuse debugging because it is not how the platform expects the HTTP request to be shaped.

**Right:** **Body** = only the SharePoint/file properties below (plus optional `fileType`). **URL** = `.../invoke?api-version=1&sp=...&sv=...&sig=...` (handled by the Action or your full invoke URL string).

If a ChatGPT debug or log view **shows** query parameters next to body fields in one blob, verify the **actual** HTTP request: query on the URL, JSON body without `api-version` / `sp` / `sv` / `sig`.

## Required JSON properties (all mandatory)

Send **`application/json`** with these keys (names and casing must match):

1. **`siteUrl`** (string) — Full SharePoint site URL, e.g. `https://contoso.sharepoint.com/sites/Finance`.
2. **`folderPath`** (string) — Path **inside the site** as the flow expects (usually library + folders), e.g. `Shared Documents/Invoices/2026` or the path your org uses for the target library. Do **not** invent paths; use a path the user provided or confirm with them.
3. **`fileName`** (string) — Name **including extension**, e.g. `Acme_Evaluation_Report_20260407.xlsx`. For Excel exports, the extension must be **`.xlsx`**.
4. **`fileContentBase64`** (string) — **Standard Base64 encoding of the raw file bytes** (see below).

Optional (if present in the schema): **`fileType`** — MIME-style hint (e.g. `pdf`, `xlsx`). It does **not** replace a correct **`fileName`** extension; the SharePoint action still relies on the file name and decoded binary content.

## Base64 rules (critical)

- Encode the **entire file** as downloaded from Code Interpreter (or equivalent): **binary → Base64**, not UTF-8 text of a path or placeholder string.
- Use **standard Base64** (RFC 4648). **Do not** include a **`data:`** URI prefix. If you have `data:application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;base64,<payload>`, strip everything **before and including** `base64,` and send **only** `<payload>`.
- **No line breaks** inside the Base64 string (Power Automate `base64ToBinary` expects a continuous string).
- The payload must be **valid Base64**; invalid characters or truncation causes the flow or SharePoint step to fail.

### Practical sequence for an `.xlsx` from Code Interpreter

1. Build the workbook and write it to a path in the analysis environment.
2. Build the JSON body using the Python pattern below (`encode_file_base64` + `create_payload`).
3. Send it via the **connected OpenAPI action** (preferred in Custom GPT) or `requests.post` only when a flow URL is supplied through configuration—**never hardcode** invoke URLs or `sig` query parameters in code or knowledge files.

---

## Python reference: payload and request (Code Interpreter)

Use this in **Code Interpreter** to turn a saved file on disk into the exact JSON the flow expects. **`fileName`** is taken from the file path (`p.name`); **`fileContentBase64`** is raw bytes → Base64 (UTF-8 ASCII string, no `data:` prefix).

```python
import base64
from pathlib import Path

import requests


def encode_file_base64(file_path: str) -> str:
    file_bytes = Path(file_path).read_bytes()
    return base64.b64encode(file_bytes).decode("utf-8")


def create_payload(site_url: str, folder_path: str, file_path: str) -> dict:
    p = Path(file_path)

    if not p.exists():
        raise FileNotFoundError(f"File not found: {file_path}")

    if p.suffix.lower() not in {".xlsx", ".csv", ".md"}:
        raise ValueError("Only .xlsx, .csv, and .md are supported")

    return {
        "siteUrl": site_url,
        "folderPath": folder_path,
        "fileName": p.name,
        "fileContentBase64": encode_file_base64(file_path),
    }


def post_flow(flow_invoke_url: str, payload: dict) -> requests.Response:
    """POST the same JSON the Power Automate trigger receives. Use only with a URL from env/config, never committed secrets."""
    return requests.post(
        flow_invoke_url,
        json=payload,
        headers={"Content-Type": "application/json"},
        timeout=120,
    )


# Example wiring (replace values with user-provided or environment-backed config):
# payload = create_payload(site_url, folder_path, path_to_saved_xlsx)
# resp = post_flow(flow_invoke_url, payload)
# resp.raise_for_status()
```

### Custom GPT: call the **Action** vs `requests`

- **Preferred:** After `create_payload(...)`, invoke the GPT’s **Upload file to SharePoint via Power Automate** (OpenAPI) action and pass **the same dictionary** as the JSON body. ChatGPT supplies the server base URL and query parameters (`api-version`, `sp`, `sv`, `sig`); you only provide **`siteUrl`**, **`folderPath`**, **`fileName`**, **`fileContentBase64`**.
- **Code Interpreter + `requests`:** Use `post_flow` only when a **full invoke URL** (including query string) is available from the user or from a **secret** such as an environment variable—do **not** embed real `sig` values in prompts, knowledge files, or printed scripts.

### `folderPath` format

Your SharePoint **Create file** action may expect a leading slash or library name exactly as in the site (e.g. `Shared Documents/Folder` vs `/Shared Documents/Folder`). Use the path the **user** confirms for their library; match what works in Power Automate for that site.

### Extending file types

**`.md`** is included for Markdown exports or PRD/ERD uploads to SharePoint. If the flow accepts other extensions (for example `.pdf`), widen the suffix check in `create_payload` accordingly.

## User confirmation

- If **`siteUrl`** or **`folderPath`** is missing or ambiguous, **ask the user** before calling the action. Do not guess production SharePoint locations.
- Align **`fileName`** with the export naming in `xlsx_report_export.md` when the upload is the evaluation report (`{ProjectSlug}_Evaluation_Report_<YYYYMMDD>.xlsx`).

## After a successful upload

Confirm **file name**, **folder path** (as sent), and **site** in plain language. If the API returns a JSON body with `status` / `fileName` / `folderPath`, you may mirror those fields for the user.

## Troubleshooting (user-visible)

- **400 on the HTTP trigger or “schema” errors:** Often the body includes **`api-version`, `sp`, `sv`, or `sig`**. Remove them from JSON; keep them on the URL only (see **Query string vs body** above). Align the Power Automate trigger’s **Request body JSON Schema** with the properties you really expect (typically the four required keys ± `fileType`).
- **Empty or corrupt file on SharePoint:** Often invalid Base64, or a **`data:`** prefix left on the string. Re-encode from raw bytes and omit prefixes. Valid `.xlsx` Base64 often decodes to bytes starting with `PK` (ZIP signature); the sample prefix `UEsDB` is consistent with that when decoded.
- **Wrong location / “folder not found”:** **`folderPath`** must match **exactly** what the SharePoint **Create file** action expects (library display name vs path, leading `/`, subfolders). Compare with a path that works when you run the flow manually. `Shared Documents` alone is the library root; subfolders need `Shared Documents/YourFolder` (or the format your tenant uses).
- **Permission errors:** The flow runs as the connected account (e.g. the account shown on the SharePoint action in Power Automate); the user may need a different path or site where that identity can write.
- **401 / 403 from Power Platform:** Callback **`sig`** or environment URL mismatch; regenerate the HTTP trigger URL in Power Automate if the signature may have been exposed.
