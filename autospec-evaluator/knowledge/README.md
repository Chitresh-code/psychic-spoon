# Knowledge docs for AutoSpec Evaluator

Preload **all** of these in your Custom GPT **Knowledge** so **Instructions** (`prompt.md`) can stay short.

| File | Used in |
|------|---------|
| `workflow_operations.md` | Step-by-step rules: **DOCX or Markdown** inputs, hallucination vs IDs, **Step 2 scope gate** (full vs reduced scope), Step 2 reference stance, **capability label accuracy**, **section classification**, **standing recommendations** (owner attribution, repo coverage), **explicit next-step option lists** after Steps 1, 2, and 4, Excel + closings, **SharePoint upload** (optional) |
| `evaluation_report_schema.md` | Step 1 – PRD vs generated ERD; slim **§2** metrics; **System Context** narrative only (no §2 row); **Traceability §5** holds capability + placement audits; **hallucination triage** (§4a/§4b); Excel: **§4a only** |
| `system_context_schema.md` | **System Context** (Steps 1–2 full or reduced, and **Step 4** two ERDs); **content-first** Step 2 rollups; slim Step 4 tables; no live GitHub/Confluence verification |
| `structure_analysis_schema.md` | Step 2 **full scope** – **content alignment** headline (seven sections when System Context applies); **content facet ledgers** in §2–§8; optional structure bullets; **capability labels** and **section classification** in §4/§5; **standing recommendations** in §9 |
| `scope_aligned_comparison_schema.md` | Step 2 **reduced scope** – same delivery scope / overlap slice; **content-first** like full-scope Step 2; System Context when in slice; **capability labels and section classification** still strict in slice |
| `xlsx_report_export.md` | **≤4 sheets** default, **≤5** when Step 4 completed; reader-friendly tab names (`Summary`, `PRD_vs_generated`, …) + plain-language banners; **`{Project Name} Evaluation Report`** title and filename; **manual download** only; **PRD_vs_generated** sheet includes **capability label audit** and **misclassified deliverables** sub-tables |
| `power_automate_report.md` | **Optional.** When the **SharePoint report upload** action is connected: **`submitEvaluationReport`** (body = **`WorkbookSheetPayload` only**; do not invent `sig` / query values), **retry with server-provided `api-version`** if the error response lists valid values, one POST per sheet, field mapping, payload size, flow design |
