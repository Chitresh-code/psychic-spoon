# Structured Excel export (.xlsx)

When the user asks for **Excel**, **Excel export**, **export report**, **download xlsx**, or **structured spreadsheet**, produce **one** downloadable **`.xlsx`** that includes **every evaluation completed in this conversation**. Use **Code Interpreter** / Python (`openpyxl` strongly preferred for formatting). The user **downloads the file manually** from the chat; do not claim a file is attached without actually generating it, and do not promise delivery through external systems.

## Project name (workbook title and filename)

Do **not** use a generic “AutoSpec Evaluation Report” label unless the PRD or user explicitly names the project that way.

1. **Project name** (for the visible report title): Prefer the **PRD product or program name**—from PRD title page, document properties, or the main H1. If missing, derive from the **PRD filename** (strip `.docx` or `.md`, trim “PRD”, “v2”, dates). If still unclear, ask once: *What project name should appear on the Excel report?*
2. **Report title (sheet 1, row 1):** **`{Project Name} Evaluation Report`** — merge **A1:C1** (or through the last column of the **Scores at a glance** table), **bold**, **14–16 pt**.
3. **Download filename:** **`{ProjectSlug}_Evaluation_Report_<YYYYMMDD>.xlsx`**
   - **ProjectSlug:** Project name made file-safe: replace spaces with underscores, remove characters `\ / * ? : [ ] < > | "`, keep letters, digits, underscores, hyphens; collapse repeated underscores; trim length if needed (keep it readable).

## Reader-facing names (never internal step numbers on sheets)

Use language that explains **what was compared**, not evaluator jargon. Same labels in **section banners** on each sheet, **Scores at a glance** group titles, and the **Evaluations included** metadata line where applicable.

**Worksheet tab `generated_vs_team_erd`** always uses the **banner title Generated vs team ERD** (both full and reduced Markdown paths). In **chat/Markdown**, Step 2 reduced scope may still use the reader name **Same delivery scope**—that name is **not** a separate Excel tab.

| Internal step | Reader-facing name (headings / banners) | One-line meaning (for tooltips or row 2 hint) |
|---------------|----------------------------------------|-----------------------------------------------|
| Step 1 | **PRD vs generated ERD** | Does the generated engineering spec reflect the PRD (coverage, traceability, risk of invented content)? |
| Step 2 (full scope) | **Generated vs team ERD** | How closely does the generated ERD match the **reference engineering ERD** (whole documents, strict)? |
| Step 2 (reduced scope) | **Same delivery scope** (Markdown/chats only) | Same spirit as **Generated vs team ERD**, but only for the **overlap period or slice** the team ERD actually covers (lenient). **Excel:** same tab as full scope — **`generated_vs_team_erd`**. |
| Step 3 | **Research context (two ERDs)** | Compare **Confluence/GitHub summary** sections **between the two ERDs only** (in-document text); **no PRD** in this phase—no live wiki/repo checks. |


## Workbook size: at most **5** worksheets

| Order (typical) | Tab / purpose | Included when |
|-----------------|---------------|---------------|
| First | **`Summary`** — metadata + score snapshot | Always (if any evaluation ran) |
| Next | **`PRD_vs_generated`** — PRD vs generated ERD full detail | Step 1 completed |
| Next | **`generated_vs_team_erd`** — Generated vs team ERD (**one** tab for all Step 2 outcomes) | Step 2 completed (any scope; **stack** two passes on this tab if the user ran Step 2 twice) |
| Next (when applicable) | **`Context_compare`** — Research context (two ERDs) | Step 3 completed |
| **Last (required)** | **`Recommendations`** — consolidated, prioritized actions from every completed phase | **Always** when exporting a workbook after **Step 1 and/or Step 2 and/or Step 3** has completed—**do not omit** this tab. |

**Hard cap:** **≤5 tabs** total. Order is always: **`Summary`** → phase detail sheets in the table order above (omit rows for phases that did not run) → **`Recommendations`** last.

- **Five tabs** when Step 1 + Step 2 + Step 3 all completed (`Summary` + three detail + `Recommendations`).
- **Four tabs** when Step 1 + Step 2 (no Step 3): omit `Context_compare`.
- **Three tabs** when Step 1 only: `Summary` + `PRD_vs_generated` + `Recommendations`.
- **Rare** (e.g. Step 3 without Step 1): include only sheets you can populate; **still** add **`Recommendations`** if any phase in the export produced recommendation content.

Do **not** split one evaluation across many tiny sheets; consolidate detail per phase into **one sheet per phase**. **Two Step 2 passes** (e.g. full then reduced): **one** **`generated_vs_team_erd`** tab with **Pass 1** and **Pass 2** stacked blocks (see below)—**not** two worksheets.

**`Recommendations` tab (mandatory):** Pull from Step 1 §6, Step 2 per-pass §Recommendations, and Step 3 **Interpretation** bullets that are **actionable** (dedupe near-duplicates). Columns: **`Source`** (e.g. `PRD vs generated ERD`, `Generated vs team ERD (pass 1)`, `Research context (two ERDs)`), **`Priority`** (High / Medium / Low if known; else Medium), **`Recommendation`** (concise text), **`Rationale or pointer`** (optional one line—section or ID). Sort **High → Medium → Low**, then by source order Step 1 → Step 2 → Step 3. If the Markdown reports truly list **no** recommendations: one data row—*`No prioritized recommendations captured—see detail sheets for findings.`* Detail sheets may still keep their in-place **Recommendations** sections for context; this tab is the **exec-ready** consolidated list.

---

## Sheet tab names (Excel-safe, ≤31 characters, no `\ / * ? : [ ]`)

Use these **exact worksheet names** for **manual Excel** in chat:

| Worksheet tab | Role |
|---------------|------|
| `Summary` | Metadata + score snapshot |
| `PRD_vs_generated` | Step 1 — PRD vs generated ERD |
| `generated_vs_team_erd` | Step 2 — Generated vs team/reference ERD (**unified**; layout below applies to **full** and **reduced** scope) |
| `Context_compare` | Step 3 — Confluence/GitHub context in two ERDs (ERD-only comparison) |
| `Recommendations` | Consolidated prioritized actions (all completed phases)—**always last** |

Optional numeric prefixes if you prefer sort order on disk: `01_Summary`, `02_PRD_vs_generated`, `03_generated_vs_team_erd`, `04_Context_compare`, `05_Recommendations`.

**SharePoint / API:** If **`submitEvaluationReport`** is used, `sheetName` in the JSON body may still follow **legacy tokens** from `power-automate-report.openapi.json` (`Report_Overview`, `PRD_Analysis`, …). **Manual Excel** in chat uses the **worksheet tab names** in the table above.

---

## Sheet 1 — `Summary`

### A) Title and metadata (top of sheet)

- **Row 1:** Report title **`{Project Name} Evaluation Report`** — merge **A1:C1** (align with **Scores at a glance** width), **bold**, font 14–16 pt.
- **Leave row 2 blank** for breathing room.
- **Row 3 onward — “Document metadata”** (user-facing section title in **A3**, bold, optional light fill on **A3:D3**):

| Label | Value |
|-------|--------|
| PRD document | filename or “uploaded” |
| Generated ERD | filename |
| Engineering reference ERD | filename or “Not evaluated” |
| Report generated | ISO date or session date |
| Step 2 scope selection | **Required whenever Step 2 completed**—copy from the session, **never** a default. Examples: `Full scope (whole documents)`; `Auto: full-program (scope gate—no user question)`; `Reduced scope (overlap slice) — user: "reduced scope"`. If Step 2 ran **twice**, add **`Step 2 scope selection (pass 2)`** or one wrapped cell listing **both** passes in order. Omit only when Step 2 did **not** run. |
| Evaluations included | List phases that ran using **one** Step 2 product line: always include **“Generated vs team ERD”** when Step 2 completed (any scope). Add **“PRD vs generated ERD”** when Step 1 ran; add **“Research context (two ERDs)”** when Step 3 completed. **Do not** use a separate evaluation name for “Same delivery scope” in this cell—**scope** is already in **Step 2 scope selection**. |

Use **two columns**: **A = label**, **B = value**. Left-align labels; wrap long paths in B.

### B) Blank row, then “Scores at a glance”

- Section header row: **Scores at a glance** (bold, same style as “Document metadata”)—readers should understand this is the numeric snapshot before they open detail tabs.
- Table with **three data columns: `Metric` | `What this measures (brief)` | `Score`** (headers bold, fill light gray **#D9D9D9** or similar). The middle column is **one short sentence each row** (not empty)—it explains the **aim of the metric** so executives do not need to open the schema.

List **headline metrics only** (percentages or Pass/Fail as appropriate)—one row per line item. Group with **blank row + merged or left-aligned group title row** (bold, fill **#E7E6E6**) between groups—the group title sits in **column A** spanning the table width visually (merge **A:C** for that title row if using openpyxl).

**Standard “What this measures” hints** (adapt wording if the metric name in the Markdown report differs slightly; keep each middle cell ≤ **120 characters**):

| Metric (typical label) | What this measures (brief) — template |
|------------------------|----------------------------------------|
| PRD coverage | Share of PRD-trackable scope that clearly appears in the generated ERD. |
| Traceability accuracy | Whether FR/ENG/EPIC-style links in the ERD map correctly back to PRD items. |
| Hallucination rate | Share of reviewed substantive ERD content outside System Context with no PRD or System Context basis (true hallucinations only). |
| Content fidelity (PRD) | Rollup of how faithful ERD substance is to the PRD given coverage and hallucination rate. |
| Content alignment / Content alignment (scoped) | Mean alignment of canonical sections (generated vs reference ERD); scoped = overlap slice only. |
| Content similarity (System Context) | Overlap of System Context narrative/tables between the two ERDs (in-document text only). |
| Inter-ERD consistency (qualitative) | Whether the two System Context blocks agree, diverge, or contradict on themes—**no** second numeric headline. |
| Overall confidence (any phase) | How trustworthy these scores are given document quality, extraction limits, or thin counts. |

Per group:

1. **PRD vs generated ERD** (if run): PRD coverage, Traceability accuracy, Hallucination rate, Content fidelity, Overall confidence (short text OK in **Score**). Step 1 has **no** standalone System Context % in §2—do not add one here. Omitted from this summary (detail only on **`PRD_vs_generated`**): capability label % and section placement % unless they appear as explicit headline rows in the Markdown §2 table.
2. **Generated vs team ERD** (if Step 2 ran **once**): Under this **single** group title, copy headline rows from the Markdown report. Use the **Metric** text from the report (e.g. **Content alignment**, **Content alignment (scoped)**). Each row gets a **What this measures** cell (use the template rows above or a one-line paraphrase tied to that pass’s mode).
3. **Generated vs team ERD** (if Step 2 ran **twice**): Under the same group title, add clearly tagged rows—e.g. **Content alignment (pass 1)** / **Content alignment (pass 2)**—each with its own **What this measures** (note full vs reduced scope in that sentence).
4. **Research context (two ERDs)** (if run): **Content similarity (System Context)** %; **Inter-ERD consistency (qualitative)** (text in **Score**, e.g. *Mixed — see detail*); **Overall confidence** for Step 3. **Do not** add **Overall Step 3 score** or any extra % row that duplicates content similarity (`system_context_schema.md`).

Use **Generated vs team ERD** as the **one** Step 2 score group title (not separate “Same delivery scope” group titles).

Do **not** duplicate full subsection parameter point evidence here—only the **summary numbers** (and the brief **What this measures** column) users need at a glance.

---

## Sheet 2 — `PRD_vs_generated` (if Step 1 completed)

**Top of sheet (recommended):** merged banner — title **PRD vs generated ERD**; optional subtitle row explaining PRD vs generated coverage and risk; **blank row**; then the sections below.

Vertical sections: **blank rows** + **section header rows** (bold, optional fill **#E7E6E6** on the header row across columns A–E or A–H).

Suggested section order:

1. **Executive summary** — overall result, one-line summary.
2. **Quantitative metrics** — full table (metric, result, basis, confidence, why) if present in report (slim §2 set per `evaluation_report_schema.md`).
3. **Counting basis** — how denominators were defined (optional).
4. **Completeness gaps** — including **Misclassified deliverables** sub-table (ERD ID, current section, recommended section, reason) and **Structural recommendations** (owner attribution, repository coverage).
5. **Capability label audit** — mismatches between ENG-XXX capability labels and PRD-defined capability names.
6. **Hallucinations / unsourced content** — list **true hallucinations only** (per `evaluation_report_schema.md` **§4a**): not in the PRD and not substantively sourced from System Context. **Do not** include **§4b** (*Traced to System Context, not in PRD*) in the workbook; that material may appear in the **Markdown** report only.
7. **Traceability** — ID mapping summary plus **§5.2** capability label audit (and optional capability **%**) and **§5.3** section placement (and optional placement **%**).
8. **Recommendations**

**Formatting:** Header row for each table = bold + light gray fill. **Freeze panes** below the first table header row (e.g. row after section title) where useful. Set **column widths** so content is readable (e.g. min 12 for text columns).

---

## Detail sheet — `generated_vs_team_erd` (if Step 2 completed)

**Single worksheet for both full-scope and reduced-scope Step 2.** Do **not** use the old full-scope-only Excel layout (e.g. **At a glance** first). **Always** use the **same section order and table shapes** as the historical **`Same_scope`** spec below. The **only** content that differs by comparison mode is the **Scope** block (declared comparison scope narrative); all other sections follow this unified layout. Copy numeric labels and table cells from the **Markdown report for that pass** (full-scope reports may say **Content alignment** without “(scoped)” in the metric name—still place that table under **Scoped summary metrics** here).

**Top of sheet (recommended):** merged banner — **Generated vs team ERD**; optional subtitle: *Generated ERD compared to the team’s reference engineering ERD; findings are changes for the generated doc.*

**Per pass** (one pass most sessions; **two passes** = repeat the entire block below with a **Pass 1 —** / **Pass 2 —** section banner and blank row between stacks):

1. **Comparison mode / Scope applied (required)** — one line matching **Summary → Step 2 scope selection** for this pass (e.g. `Full scope (whole documents)`, `Auto: full-program`, or `Reduced scope (overlap slice)` + optional user quote).
2. **Scope** — **Mode-specific prose only:**  
   - **Reduced scope:** Declared slice per `scope_aligned_comparison_schema.md` §1 (engineering ERD scope inferred, how inferred, generated ERD slice, fairness note as in Markdown).  
   - **Full scope / auto full-program:** State plainly that comparison covers **whole documents** (strict) or **auto-selected full-program** per the scope gate—**do not** reuse overlap-slice wording from reduced path.  
   Same section heading **Scope** for all modes.
3. **Scoped summary metrics** — headline quantitative table from the Markdown report for this pass (same subsection title for all modes; mirror table shape from reduced-format reports).
4. **Per-section scoped scores** — content % table only (no structure % columns).
5. **Per-section alignment evidence (scoped)** — for each section: **alignment score** (% and basis); then the two Markdown bullet groups (**Same in both ERDs**; **Engineering includes; generated misses or underplays**) as plain lists—**no** `Parameters considered` tables or parameter-by-parameter score dumps.
6. **Optional scoped structure bullets**
7. **Recommendations**

**Two Step 2 passes in one session:** After the first pass’s block (1–7), insert a clear **Pass 2 —** banner and repeat **(1)**–**(7)** using the **second** Markdown report’s content and its own **Comparison mode / Scope applied** line.

---

## Detail sheet — `Context_compare` (if Step 3 completed)

**Top of sheet (recommended):** merged banner — **Research context (two ERDs)**; optional subtitle: *System Context sections compared **using only text inside the two ERDs**; no PRD alignment and no live wiki or repo verification.*

Sections:

1. **Documents compared** — which two ERDs (labels A/B), how the pair was chosen; **do not** require a PRD column for this sheet.
2. **Quantitative metrics — pair table** — **only** **Content similarity (System Context)** % and **Inter-ERD consistency** (qualitative), each with **Confidence / How obtained / Why**. **Do not** add **Overall Step 3** % (duplicate of content similarity).
3. **System Context Comparison** — shared / unique lists and similarity recap (from Markdown).
4. **Cross-ERD theme–claim ledger and contradiction ledgers** — per `system_context_schema.md` (omit mention-overlap lists).

---

## Professional design (implementation) — **quality bar**

The workbook must look **deliberately formatted**, not like a raw CSV dump. Unreadable exports (tiny columns, no wrap, overlapping text, default row height crushing wrapped paragraphs) are **incorrect**.

When using **openpyxl** (preferred):

- **Fonts:** **Calibri** 11 pt body; **12 pt bold** for worksheet banner titles; **11 pt bold** for section headers and table header rows.
- **Fills:** Section banners **#E7E6E6**; table header row **#D9D9D9**; **Summary** metadata header row same as banners. Avoid dark fills (print-friendly).
- **Borders:** **Thin** border on **every** cell inside rectangular data tables (including **Scores at a glance**).
- **Wrap text:** Set `alignment.wrap_text = True` and `vertical = 'top'` on **all** cells that may hold more than one line (metadata values, **What this measures**, recommendations, bullet lists). For **Summary** scores table, apply wrap to **B** and **C**.
- **Column widths (starting point—widen if content still clips):**  
  - **Summary:** A **26**, B **48** (aim column), C **14** (score). Title merge across A:C.  
  - **Detail sheets:** narrative columns **min 40**; ID columns **12–14**; short status **18**.  
  - **`Recommendations`:** Source **22**, Priority **12**, Recommendation **50**, Rationale **36**.
- **Row height:** Default **15** pt minimum for body rows; for wrapped blocks after writing content, call **`sheet.row_dimensions[i].height = None`** to auto-height **or** set explicit heights (e.g. **30–60**) for rows with long wrapped text—**verify** in export that no row shows `#######`.
- **Freeze panes:** **Summary** — freeze below the **Scores at a glance** header row (so metadata scrolls away but headers stay). **Detail sheets** — freeze below the sheet banner + blank row + first section header **or** first table header (choose one rule per sheet and apply consistently).
- **Alignment:** Labels left; **Score** column center or right; percentages readable (store as text `87%` **or** number + **%** number format—**do not** mix inconsistently within one sheet).
- **Merged cells:** Use merges **only** for sheet title rows (banner)—**do not** merge cells inside dense data tables (sorting/filter pain and unreadable exports).
- **Gridlines:** Optional `sheet.sheet_view.showGridLines = False` on **Summary** for a cleaner look; keep gridlines on detail sheets if it helps navigation—either choice is fine if readability is preserved.
- **Print / width sanity:** If a table is wider than useful, break into **continuation blocks** with a repeated mini-header rather than one row with 4000 characters.

If using **pandas** `to_excel`, you **must** post-process with **openpyxl** to apply the above (widths, wrap, borders, freezes)—pandas alone is **not** sufficient for this evaluator’s reports.

---

## Rules

1. **All completed evaluations** appear: **Summary** plus **`PRD_vs_generated`** (Step 1), **`generated_vs_team_erd`** (Step 2 once or twice stacked), **`Context_compare`** (Step 3) as applicable, and **`Recommendations`** last—**at most five** worksheets total.
2. **Missing phase:** Omit that detail sheet; list what was included under metadata on **Summary**. **Still include `Recommendations`** whenever at least one completed phase can supply rows (see **Workbook size** above).
3. **Source of truth:** Copy from Markdown reports in the thread; **do not** invent scores, bullets, or rows. If the Markdown report honestly says a section was missing or had nothing to list, reflect that—**do not** fabricate detail to fill the sheet.
4. **No “Step N”** on workbook faces—use the **reader-facing names** and **worksheet tab names** from the tables above (and optional banners on each sheet).
5. **Step 2 Excel layout is unified:** Never recreate a separate **At-a-glance-first** layout for full scope on **`generated_vs_team_erd`**—only the **Scope** narrative changes by mode.
6. **Step 3 metrics:** In **`Context_compare`** and **Summary**, never duplicate **Content similarity** as a separate “Overall Step 3” % row.

## Filename

`{ProjectSlug}_Evaluation_Report_<YYYYMMDD>.xlsx` — see **Project name** above.

---

## If context is incomplete

If earlier phases are missing from context, state under **Document metadata** on **Summary** which evaluations could not be exported; include only sheets you can populate from the thread.

---

## Delivery

Offer the finished **`.xlsx`** as a **file the user downloads from the conversation** (manual save). Do not integrate with external upload, automation flows, or third-party messaging.
