# Knowledge docs for AutoSpec Evaluator

Preload **all** of these in your Custom GPT **Knowledge** so **Instructions** (`prompt.md`) can stay short.

| File | Used in |
|------|---------|
| `workflow_operations.md` | Step-by-step rules: **DOCX or Markdown** inputs, hallucination vs IDs, Step 2 reference stance, **capability label accuracy**, **section classification**, **standing recommendations** (owner attribution, repo coverage), **explicit next-step option lists** after Steps 1–4, Excel + closings, **Teams** |
| `evaluation_report_schema.md` | Step 1 – PRD vs generated ERD evaluation report (quantitative metrics incl. **capability label accuracy** and **section classification accuracy**, completeness with **misclassified deliverables** and **structural recommendations**, traceability with **capability label audit**) |
| `system_context_schema.md` | **System Context** block (Steps 1–3) and **Step 4** (two-ERD context comparison); no live GitHub/Confluence verification |
| `structure_analysis_schema.md` | Step 2 – structure + content alignment (seven sections when System Context applies); check ledger + facet ledger in §2–§8; **capability label accuracy** in §4, **section classification** cross-check in §4/§5, **standing recommendations** in §9 |
| `scope_aligned_comparison_schema.md` | Step 3 – scoped comparison; same metric model as Step 2, scoped; System Context when in slice; **capability labels and section classification** still strict in slice |
| `xlsx_report_export.md` | **≤4 sheets** default, **≤5** when Step 4 completed; overview + detail per phase; **`{Project Name} Evaluation Report`** title and filename; **manual download** only; PRD Analysis sheet includes **capability label audit** and **misclassified deliverables** sub-tables |
| `teams_channel_message.md` | **Optional.** When the **Teams Channel Message** action is connected: **`sendChannelMessage`** payload, structured **HTML tables** with inline styling for all evaluation metrics, per-step templates (Steps 1–4), 28 KB message limit handling, OAuth note |
