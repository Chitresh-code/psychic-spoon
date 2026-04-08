# Structured Excel export (.xlsx)

When the user asks for **Excel**, **Excel export**, **export report**, **download xlsx**, or **structured spreadsheet**, produce **one** downloadable **`.xlsx`** that includes **every evaluation completed in this conversation**. Use **Code Interpreter** / Python (`openpyxl` strongly preferred for formatting). Do not claim a file is attached without actually generating it.

## Project name (workbook title and filename)

Do **not** use a generic “AutoSpec Evaluation Report” label unless the PRD or user explicitly names the project that way.

1. **Project name** (for the visible report title): Prefer the **PRD product or program name**—from PRD title page, document properties, or the main H1. If missing, derive from the **PRD filename** (strip `.docx` or `.md`, trim “PRD”, “v2”, dates). If still unclear, ask once: *What project name should appear on the Excel report?*
2. **Report title (sheet 1, row 1):** **`{Project Name} Evaluation Report`** (merge cells, bold, 14–16 pt).
3. **Download filename:** **`{ProjectSlug}_Evaluation_Report_<YYYYMMDD>.xlsx`**
   - **ProjectSlug:** Project name made file-safe: replace spaces with underscores, remove characters `\ / * ? : [ ] < > | "`, keep letters, digits, underscores, hyphens; collapse repeated underscores; trim length if needed (keep it readable).

## User-facing names (never “Step 1/2/3/4” on sheets)

| Internal step | User-facing name |
|---------------|------------------|
| Step 1 | **PRD Analysis** |
| Step 2 | **Engineering ERD Comparison** |
| Step 3 | **Scoped alignment review** |
| Step 4 | **System Context comparison** |

Use these names in sheet titles, section headers, and the overview—**not** “Step 1”, etc.

## Workbook size: at most 5 sheets (when Step 4 completed)

| # | Sheet purpose | Included when |
|---|----------------|---------------|
| 1 | **Report overview** — metadata + score summary only | Always (if any evaluation ran) |
| 2 | **PRD Analysis** — full detail | PRD Analysis was completed |
| 3 | **Engineering ERD Comparison** — full detail | Engineering ERD comparison was completed |
| 4 | **Scoped alignment review** — full detail | Scoped alignment (Step 3) was completed |
| 5 | **System Context comparison** — full detail | System Context comparison (Step 4) was completed |

**Default cap:** **≤4 sheets** when Step 4 has **not** run (omit sheet 5). When Step 4 **has** run, use **≤5 sheets** (add **System Context comparison**).

- **4 sheets total** if Steps 1–3 completed but Step 4 did **not** run.
- **3 sheets total** if scoped alignment was **not** run (Overview + PRD Analysis + Engineering ERD Comparison).
- **2 sheets** if only PRD Analysis was completed (Overview + PRD Analysis).
- **1 sheet** only if you must export with no detail sheets (avoid if possible).

Do **not** split one evaluation across many tiny sheets; consolidate detail per phase into **one sheet per phase**.

---

## Sheet naming (professional, Excel-safe)

Use **short, clear names** (≤31 characters, no `\ / * ? : [ ]`):

| Recommended sheet name | Role |
|------------------------|------|
| `Report_Overview` | Dashboard: metadata + scores |
| `PRD_Analysis` | Step 1 detail |
| `Eng_ERD_Comparison` | Step 2 detail |
| `Scoped_Alignment` | Step 3 detail |
| `System_Context_Comp` | Step 4 detail (optional fifth sheet) |

Alternative if you prefer numeric prefixes: `01_Overview`, `02_PRD_Analysis`, `03_Eng_ERD_Comparison`, `04_Scoped_Alignment`, `05_System_Context_Comp`.

---

## Sheet 1 — `Report_Overview`

### A) Title and metadata (top of sheet)

- **Row 1:** Report title **`{Project Name} Evaluation Report`** — merge **A1:B1** or **A1:D1**, **bold**, font 14–16 pt.
- **Leave row 2 blank** for breathing room.
- **Row 3 onward — “Document metadata”** (user-facing section title in **A3**, bold, optional light fill on **A3:D3**):

| Label | Value |
|-------|--------|
| PRD document | filename or “uploaded” |
| Generated ERD | filename |
| Engineering reference ERD | filename or “Not evaluated” |
| Report generated | ISO date or session date |
| Evaluations included | e.g. “PRD Analysis; Engineering ERD Comparison; Scoped alignment review; System Context comparison” |

Use **two columns**: **A = label**, **B = value**. Left-align labels; wrap long paths in B.

### B) Blank row, then “Score summary”

- Section header row: **Score summary** (bold, same style as “Document metadata”).
- Table with **exactly two data columns: `Metric` | `Score`** (headers bold, fill light gray **#D9D9D9** or similar).

List **headline metrics only** (percentages or Pass/Fail as appropriate)—one row per line item. Group with **blank row + small caps section title** between groups:

1. **PRD Analysis** (if run): e.g. Structural completeness, PRD coverage, Traceability accuracy, Hallucination rate, Content fidelity, Overall confidence (short text OK in Score column).
2. **Engineering ERD Comparison** (if run): Structural alignment %, Content alignment %, Combined alignment %, Overall confidence.
3. **Scoped alignment review** (if run): same three % lines + overall confidence (scoped).
4. **System Context comparison** (if run): **per-ERD** context–PRD alignment % and relevance % (ERD A vs ERD B), then pair metrics (thematic consistency %, inter-ERD consistency score %, optional mention overlap, overall Step 4 %), each with confidence notes as in the Markdown report.

Do **not** duplicate full ledgers here—only the **summary numbers** users need at a glance.

---

## Sheet 2 — `PRD_Analysis` (if Step 1 completed)

One sheet; **vertical sections** separated by **blank rows** and **section header rows** (bold, optional fill **#E7E6E6** on the header row across columns A–E or A–H).

Suggested section order:

1. **Executive summary** — overall result, one-line summary.
2. **Quantitative metrics** — full table (metric, result, basis, confidence, why) if present in report, including **System Context** rows when evaluated.
3. **Counting basis** — how denominators were defined (optional).
4. **Completeness gaps**
5. **Hallucinations / unsourced content**
6. **Traceability**
7. **Recommendations**

**Formatting:** Header row for each table = bold + light gray fill. **Freeze panes** below the first table header row (e.g. row after section title) where useful. Set **column widths** so content is readable (e.g. min 12 for text columns).

---

## Sheet 3 — `Eng_ERD_Comparison` (if Step 2 completed)

Sections (same separation rules: blank row + section header row):

1. **Summary** — headline metrics, overall structure match, interpretation.
2. **Per-section scores** — table.
3. **Structure check ledgers** — all sections including **System Context** when evaluated (long format).
4. **Content facet ledgers** — all sections (long format).
5. **Alignment recommendations**

---

## Sheet 4 — `Scoped_Alignment` (if Step 3 completed)

Sections:

1. **Scope** — declared scope, inference, generated slice, fairness vs full-document comparison.
2. **Scoped summary metrics**
3. **Per-section scoped scores**
4. **Structure check ledgers (scoped)**
5. **Content facet ledgers (scoped)**
6. **Recommendations**

---

## Sheet 5 — `System_Context_Comp` (if Step 4 completed)

Sections:

1. **Summary** — documents compared (generated vs engineering and/or other upload), PRD reference.
2. **Quantitative metrics — Table A** — per-ERD columns: Context–PRD alignment %, Context relevance % (and optional structural completeness) for **ERD A** and **ERD B**; include **Confidence / How obtained / Why** (extra columns or notes column).
3. **Quantitative metrics — Table B** — pair metrics: thematic consistency %, inter-ERD consistency score %, optional mention overlap, qualitative relevance consistency, overall Step 4 %; **Confidence / How / Why** each.
4. **PRD mapping / contradiction / mention ledgers** — per `system_context_schema.md`.

---

## Professional design (implementation)

When using **openpyxl**:

- **Fonts:** Calibri or Arial 11 pt body; **bold** for section titles and table headers.
- **Fills:** Section banners **#E7E6E6**; table header row **#D9D9D9**; avoid dark colors (print-friendly).
- **Borders:** Thin border on data tables (`thin` on all cells of tabular blocks).
- **Column widths:** Set `sheet.column_dimensions['A'].width = …` for A–H so tables do not look cramped.
- **Row height:** Slightly taller for section title rows (optional).
- **Freeze:** On detail sheets, freeze first header row of the first major table or row under the sheet title block.
- **Alignment:** Labels column left; numbers/scores center or right for `%` columns.

If using **pandas** only, export then **post-process** with openpyxl for formatting—or build with openpyxl from the start for Overview + detail sheets.

---

## Rules

1. **All completed evaluations** appear: Overview scores + every detail sheet for phases that ran (up to **five** detail phases when Step 4 ran).
2. **Missing phase:** Omit that detail sheet; list what was included under metadata on **Report_Overview**.
3. **Source of truth:** Copy from Markdown reports in the thread; do not invent scores.
4. **No “Step N”** in user-visible labels—use the **user-facing names** table above.

## Filename

`{ProjectSlug}_Evaluation_Report_<YYYYMMDD>.xlsx` — see **Project name** above.

---

## If context is incomplete

If earlier phases are missing from context, state under **Document metadata** on **Report_Overview** which evaluations could not be exported; include only sheets you can populate from the thread.

---

## Uploading the workbook to SharePoint (Power Automate)

When the user wants the **same `.xlsx`** on SharePoint (not only a chat download), use the connected **Upload file to SharePoint via Power Automate** action after the file exists in the analysis environment.

1. **Binary → Base64:** Read the generated `.xlsx` as **bytes**, encode with standard Base64, and send **only** the encoded string in **`fileContentBase64`** (no `data:...;base64,` prefix, no line breaks).
2. **Required body fields:** **`siteUrl`**, **`folderPath`**, **`fileName`** (with the real extension, e.g. **`.xlsx`** or **`.md`**), **`fileContentBase64`** — these map to the flow’s SharePoint **Create file** step; file content is decoded with `base64ToBinary(triggerBody()?['fileContentBase64'])`.
3. **Paths:** Obtain **`siteUrl`** and **`folderPath`** from the user when not explicit; do not guess.

Full rules, troubleshooting, and field semantics: **`power_automate_sharepoint_upload.md`**.
