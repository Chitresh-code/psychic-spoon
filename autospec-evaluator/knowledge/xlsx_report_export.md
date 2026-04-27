# Structured Excel export (.xlsx)

When the user asks for **Excel**, **Excel export**, **export report**, **download xlsx**, or **structured spreadsheet**, produce **one** downloadable **`.xlsx`** that includes **every evaluation completed in this conversation**. Use **Code Interpreter** / Python (`openpyxl` strongly preferred for formatting). The user **downloads the file manually** from the chat; do not claim a file is attached without actually generating it, and do not promise delivery through external systems.

## Project name (workbook title and filename)

Do **not** use a generic “AutoSpec Evaluation Report” label unless the PRD or user explicitly names the project that way.

1. **Project name** (for the visible report title): Prefer the **PRD product or program name**—from PRD title page, document properties, or the main H1. If missing, derive from the **PRD filename** (strip `.docx` or `.md`, trim “PRD”, “v2”, dates). If still unclear, ask once: *What project name should appear on the Excel report?*
2. **Report title (sheet 1, row 1):** **`{Project Name} Evaluation Report`** (merge cells, bold, 14–16 pt).
3. **Download filename:** **`{ProjectSlug}_Evaluation_Report_<YYYYMMDD>.xlsx`**
   - **ProjectSlug:** Project name made file-safe: replace spaces with underscores, remove characters `\ / * ? : [ ] < > | "`, keep letters, digits, underscores, hyphens; collapse repeated underscores; trim length if needed (keep it readable).

## Reader-facing names (never internal step numbers on sheets)

Use language that explains **what was compared**, not evaluator jargon. Same labels in **sheet tabs**, **section banners** on each sheet, **Scores at a glance** group titles, and the **Evaluations included** metadata line.

| Internal step | Reader-facing name (headings / tabs) | One-line meaning (for tooltips or row 2 hint) |
|---------------|--------------------------------------|-----------------------------------------------|
| Step 1 | **PRD vs generated ERD** | Does the generated engineering spec reflect the PRD (coverage, traceability, risk of invented content)? |
| Step 2 (full scope) | **Generated vs team ERD** | How closely does the generated ERD match the **reference engineering ERD** (whole documents, strict)? |
| Step 2 (reduced scope) | **Same delivery scope** | Same spirit as **Generated vs team ERD**, but only for the **overlap period or slice** the team ERD actually covers (lenient). |
| Step 4 | **Research context (two ERDs)** | Compare **Confluence/GitHub summary** sections between two ERDs against the PRD—no live wiki/repo checks. |


## Workbook size: at most 5 sheets (when Step 4 completed)

| # | Sheet purpose | Included when |
|---|----------------|---------------|
| 1 | **Summary** — metadata + score snapshot | Always (if any evaluation ran) |
| 2 | **PRD vs generated ERD** — full detail | Step 1 completed |
| 3 | **Generated vs team ERD** — full detail | Step 2 completed with **full scope** |
| 4 | **Same delivery scope** — full detail | Step 2 completed with **reduced scope** |
| 5 | **Research context (two ERDs)** — full detail | Step 4 completed |

**Default cap:** **≤4 sheets** when Step 4 has **not** run (omit sheet 5). When Step 4 **has** run, use **≤5 sheets**.

- **4 sheets total** when Step 1 + Step 2 ran **twice** with different modes (full and reduced) + Step 4 did **not** run—rare.
- **3 sheets total** when Step 1 and **one** Step 2 outcome exist (Summary + PRD vs generated + **`Vs_team_ERD`** *or* **`Same_scope`**).
- **2 sheets** if only Step 1 was completed (Summary + PRD vs generated).
- **1 sheet** only if you must export with no detail sheets (avoid if possible).

Do **not** split one evaluation across many tiny sheets; consolidate detail per phase into **one sheet per phase**.

---

## Sheet tab names (Excel-safe, ≤31 characters, no `\ / * ? : [ ]`)

Use these **exact worksheet names** so tabs read clearly without evaluator context:

| Worksheet tab | Role |
|---------------|------|
| `Summary` | Metadata + score snapshot |
| `PRD_vs_generated` | Step 1 — PRD vs generated ERD |
| `Vs_team_ERD` | Step 2 **full scope** — Generated vs team/reference ERD |
| `Same_scope` | Step 2 **reduced scope** — Overlap / delivery-slice comparison |
| `Context_compare` | Step 4 — Confluence/GitHub context in two ERDs |

Optional numeric prefixes if you prefer sort order on disk: `01_Summary`, `02_PRD_vs_generated`, `03_Vs_team_ERD`, `04_Same_scope`, `05_Context_compare`.

**SharePoint / API:** If **`submitEvaluationReport`** is used, `sheetName` in the JSON body may still follow **legacy tokens** from `power-automate-report.openapi.json` (`Report_Overview`, `PRD_Analysis`, …). **Manual Excel** in chat uses the **worksheet tab names** in the table above.

---

## Sheet 1 — `Summary`

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
| Evaluations included | List only phases that ran—e.g. “PRD vs generated ERD; Generated vs team ERD” **or** “PRD vs generated ERD; Same delivery scope”; add “Research context (two ERDs)” when Step 4 completed |

Use **two columns**: **A = label**, **B = value**. Left-align labels; wrap long paths in B.

### B) Blank row, then “Scores at a glance”

- Section header row: **Scores at a glance** (bold, same style as “Document metadata”)—readers should understand this is the numeric snapshot before they open detail tabs.
- Table with **exactly two data columns: `Metric` | `Score`** (headers bold, fill light gray **#D9D9D9** or similar).

List **headline metrics only** (percentages or Pass/Fail as appropriate)—one row per line item. Group with **blank row + small caps section title** between groups:

1. **PRD vs generated ERD** (if run): PRD coverage, Traceability accuracy, Hallucination rate, Content fidelity, Overall confidence (short text OK in Score column). System Context findings stay **narrative** in the Markdown report—do not add a System Context % row here. Omitted from this summary (detail only on **`PRD_vs_generated`** sheet): capability label % and section placement % under **Traceability** in the Markdown report.
2. **Generated vs team ERD** (if run): **Content alignment %** only, Overall confidence.
3. **Same delivery scope** (if run): **Content alignment (scoped) %**, Overall confidence (scoped).
4. **Research context (two ERDs)** (if run): **Context–PRD alignment** % and **Context relevance** % for ERD A and ERD B, **Overall Step 4** %, qualitative **Relevance consistency**, Overall confidence—aligned with `system_context_schema.md` Step 4 (no retired pair metrics).

Use these **same phrases** as **small caps section titles** above each score group so readers see one vocabulary from metadata through scores.

Do **not** duplicate full ledgers here—only the **summary numbers** users need at a glance.

---

## Sheet 2 — `PRD_vs_generated` (if Step 1 completed)

**Top of sheet (recommended):** merged banner — title **PRD vs generated ERD**; optional subtitle row explaining PRD vs generated coverage and risk; **blank row**; then the sections below.

Vertical sections: **blank rows** + **section header rows** (bold, optional fill **#E7E6E6** on the header row across columns A–E or A–H).

Suggested section order:

1. **Executive summary** — overall result, one-line summary.
2. **Quantitative metrics** — full table (metric, result, basis, confidence, why) if present in report (slim §2 set per `evaluation_report_schema.md`).
3. **Counting basis** — how denominators were defined (optional).
4. **System Context (when present)** — prose subsection from the Markdown report (shape, PRD alignment, relevance, unsupported claims; optional dimension % in text—**not** a row in the quantitative table).
5. **Completeness gaps** — including **Misclassified deliverables** sub-table (ERD ID, current section, recommended section, reason) and **Structural recommendations** (owner attribution, repository coverage).
6. **Capability label audit** — mismatches between ENG-XXX capability labels and PRD-defined capability names.
7. **Hallucinations / unsourced content** — list **true hallucinations only** (per `evaluation_report_schema.md` **§4a**): not in the PRD and not substantively sourced from System Context. **Do not** include **§4b** (*Traced to System Context, not in PRD*) in the workbook; that material may appear in the **Markdown** report only.
8. **Traceability** — ID mapping summary plus **§5.2** capability label audit (and optional capability **%**) and **§5.3** section placement (and optional placement **%**).
9. **Recommendations**

**Formatting:** Header row for each table = bold + light gray fill. **Freeze panes** below the first table header row (e.g. row after section title) where useful. Set **column widths** so content is readable (e.g. min 12 for text columns).

---

## Sheet 3 — `Vs_team_ERD` (if Step 2 **full scope** completed)

**Top of sheet (recommended):** merged banner — **Generated vs team ERD**; optional subtitle: *Generated ERD compared to the team’s reference engineering ERD; findings are changes for the generated doc.*

Sections (same separation rules: blank row + section header row):

1. **At a glance** — headline **Content alignment** %, overall content match, interpretation.
2. **Per-section scores** — **content %** table only (no structure % columns).
3. **Content facet ledgers** — all sections (long format).
4. **Optional structure notes** — concise bullets if the Markdown report included them.
5. **Alignment recommendations**

---

## Sheet 4 — `Same_scope` (if Step 2 **reduced scope** completed)

**Top of sheet (recommended):** merged banner — **Same delivery scope**; optional subtitle: *Same as “Generated vs team ERD,” but only for the **delivery period or requirement slice** the team ERD actually covers.*

Sections:

1. **Scope** — declared scope, inference, generated slice, fairness vs full-document comparison.
2. **Scoped summary metrics** — **Content alignment (scoped)** only
3. **Per-section scoped scores** — content % only
4. **Content facet ledgers (scoped)**
5. **Optional scoped structure bullets**
6. **Recommendations**

---

## Sheet 5 — `Context_compare` (if Step 4 completed)

**Top of sheet (recommended):** merged banner — **Research context (two ERDs)**; optional subtitle: *Confluence/GitHub summary sections written into each ERD, compared in light of the PRD; no live wiki or repo verification.*

Sections:

1. **Documents compared** — which two ERDs, PRD reference, option A/B if applicable.
2. **Quantitative metrics — Table A** — **Context–PRD alignment** % and **Context relevance** % for **ERD A** and **ERD B**; include **Confidence / How obtained / Why** (extra columns or notes column).
3. **Quantitative metrics — Table B** — **Relevance consistency** (qualitative), **Overall Step 4** %; **Confidence / How / Why** each.
4. **PRD mapping / contradiction ledgers** — per `system_context_schema.md` (omit mention-overlap lists).

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

If using **pandas** only, export then **post-process** with openpyxl for formatting—or build with openpyxl from the start for **Summary** + detail sheets.

---

## Rules

1. **All completed evaluations** appear: **Summary** plus every detail sheet for phases that ran (at most **four** detail tabs—PRD vs generated; **either** Generated vs team **or** Same delivery scope from Step 2, **or both** if the user ran Step 2 twice; Research context when Step 4 ran).
2. **Missing phase:** Omit that detail sheet; list what was included under metadata on **Summary**.
3. **Source of truth:** Copy from Markdown reports in the thread; do not invent scores.
4. **No “Step N”** on workbook faces—use the **reader-facing names** and **worksheet tab names** from the tables above (and optional banners on each sheet).

## Filename

`{ProjectSlug}_Evaluation_Report_<YYYYMMDD>.xlsx` — see **Project name** above.

---

## If context is incomplete

If earlier phases are missing from context, state under **Document metadata** on **Summary** which evaluations could not be exported; include only sheets you can populate from the thread.

---

## Delivery

Offer the finished **`.xlsx`** as a **file the user downloads from the conversation** (manual save). Do not integrate with external upload, automation flows, or third-party messaging.
