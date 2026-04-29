# AutoSpec Evaluator — Custom GPT package

## Layout

| Path | Purpose |
|------|---------|
| **`prompt.md`** | Paste into the GPT **Instructions** field |
| **`knowledge/`** | Upload **every** `.md` file as **Knowledge** |
| **`agent.yaml`** | Metadata (name, model, capabilities) for sync scripts / docs |
| **`apps/`** | Optional integrations (`connected-apps.yaml`) only if you add external APIs later |

## Setup (ChatGPT)

1. Create a custom GPT.
2. Paste **`prompt.md`** into **Instructions**.
3. Add **Knowledge**: upload the schema/ops `.md` files in **`knowledge/`** (`workflow_operations.md`, `evaluation_report_schema.md`, `structure_analysis_schema.md`, `scope_aligned_comparison_schema.md`, `system_context_schema.md`, `xlsx_report_export.md`). If you connect **SharePoint via Power Automate** (below), also upload **`power_automate_report.md`**. `knowledge/README.md` is optional. **Step 2** merges full- vs reduced-scope team comparison. **Step 3** is **Research context (two ERDs)**—System Context compared **only inside the two ERDs** (no PRD in that step); legacy chat phrase **Step 4** still accepted.
4. Enable **Code interpreter & data analysis** and **Canvas** (optional).
5. **Optional — SharePoint (Power Automate HTTP trigger):** In **Actions**, import **`apps/power-automate-report.openapi.json`** (the working **Power Automate** definition—keep it aligned with your flow in **ChatGPT** or in a branch; do not fork the JSON casually in-repo). In the **Actions** UI, set the **base URL** and **query** defaults so they match your real **HTTP request** trigger (`api-version`, `sp`, `sv`, `sig` as the platform provides). Set authentication to **None** if you use **SAS** in `sig` per the spec. Upload **`power_automate_report.md`** as Knowledge so the model uses **`WorkbookSheetPayload` only** in the **body** (never invented query values in JSON) and **retries with a valid `api-version`** when the error response provides one.

## Repo mirror

The same agent also lives under `agents/autospec-evaluator/` in the workspace for version control and automation.
