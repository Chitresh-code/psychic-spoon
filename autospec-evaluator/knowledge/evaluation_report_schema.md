# Step 1: PRD vs Generated ERD – Evaluation Report Schema

Use this structure for the evaluation report after comparing the PRD to the generated ERD (**DOCX** or **Markdown**).

**System Context:** When the ERD includes a System Context block, follow **`system_context_schema.md`** for additional metrics and rules (no external GitHub/Confluence verification).

## Section order (strict)

1. Evaluation Summary
2. Quantitative Metrics
3. Completeness (Gaps)
4. Hallucination / Unsourced Content
5. Traceability Summary
6. Recommendations

---

## Metric definitions (Step 1)

All percentages are **0–100** (one decimal place is enough). State **numerator / denominator** next to each percentage so the user can audit the score. If a denominator is **0**, report the metric as **N/A** with a short reason.

| Metric | Meaning | How to compute |
|--------|---------|----------------|
| **Structural completeness** | How fully the generated ERD implements the **canonical ERD shape** (sections present, in sensible order, populated enough to be usable). | Use the same **seven** areas as Step 2 when System Context is present: Document Metadata, Monthly Delivery Summary, Functional Engineering Deliverables, Non-Functional Engineering Deliverables, Engineering Epics, E2E Testing Plan, **System Context**. If the generated ERD has **no** System Context block, use **six** areas only (same formula with divisor 6) and state that System Context was absent. Per area: **1.0** = present and appropriately structured; **0.5** = present but wrong format, wrong table shape, or clearly incomplete; **0** = missing or not identifiable. **Structural completeness %** = (sum of scores / N) × 100 where N is 6 or 7. **Detail:** See `system_context_schema.md` for System Context shape checks. |
| **PRD coverage** | Share of PRD-trackable scope reflected in the ERD. | Build a **denominator**: count discrete PRD items you treat as in-scope (capabilities, functional requirements, explicit deliverables, and other explicit commitments in the PRD—be consistent and list the counting rule in the report). **Numerator**: how many have a **clear, defensible** reflection in the ERD (any reasonable section). **PRD coverage %** = (numerator / denominator) × 100. |
| **Traceability accuracy** | Quality of ID/content mapping from ERD back to PRD. | **Denominator**: number of trace links you assess (e.g. ENG rows, FR references, epics tied to requirements—exclude bare AutoSpec ID tokens as “errors” by themselves). **Numerator**: links that **correctly** map to a PRD item. Broken, missing, or invented mappings count in the denominator as **not** accurate. **Traceability accuracy %** = (numerator / denominator) × 100. |
| **Hallucination rate** | Share of reviewed ERD **substance** (outside System Context) that is a **true** hallucination: not supported by the PRD and **not** plausibly sourced from the **System Context** block. | **Denominator**: count of **substantive** ERD elements you review for this metric—typically **deliverables, epics, NFRs, and similar rows outside the System Context section** (same counting rule as before; **not** AutoSpec-generated IDs alone, not empty cells). **Do not** double-count the System Context *section’s own* narrative rows in this denominator; those use **Unsupported / contradictory context claims** and other System Context metrics. **Numerator**: elements that lack PRD support **and** for which you **cannot** find an adequate match in the **System Context** text—list the rest in **§4b**, not here. **Hallucination rate %** = (numerator / denominator) × 100. If the generated ERD has **no** System Context block, use PRD-only unsourced logic for the numerator. If zero items qualify, the rate can be **0%** with denominator stated. |
| **Content fidelity (PRD)** | Single rollup of how faithful ERD content is to the PRD, combining coverage and hallucinations. | **Default formula:** Content fidelity % = **PRD coverage % × (1 − hallucination rate / 100)**, using the **hallucination rate** that **excludes** context-traced items (above). If hallucination rate is N/A, use **PRD coverage %** as fidelity and say so. Optionally add **−5** percentage points per **severe** traceability break (max −15), documented in the metrics notes. |
| **Capability label accuracy** | Correctness of the Capability field in each ENG-XXX row against the capability names defined in the PRD. | Build a **canonical capability list** from the PRD (one entry per distinct capability heading, scope area, or feature grouping as stated in the PRD). **Denominator**: total ENG-XXX rows that carry a Capability field. **Numerator**: rows whose Capability value **matches** (case-insensitive, minor punctuation tolerance) one canonical capability name. A paraphrased, merged, or invented capability label counts as inaccurate. **Accuracy %** = (numerator / denominator) × 100. List every mismatch in §5 Traceability alongside the correct PRD capability name. |
| **Section classification accuracy** | Whether deliverables are placed in the correct ERD section — Functional Engineering Deliverables vs Non-Functional Engineering Deliverables. | Scan every ENG-XXX and NFR-XXX item across both sections. For each, assess whether its description primarily concerns **internal monitoring, alerting, reporting, observability, SLA tracking, operational dashboards, or audit compliance** — these are non-functional by nature and belong in Non-Functional Deliverables. Items that describe **new features, API endpoints, workflow logic, or user-facing capabilities** belong in Functional Deliverables. **Denominator**: total items assessed. **Numerator**: items in the correct section. **Classification accuracy %** = (numerator / denominator) × 100. Flag every misplaced item in §3 Completeness with the recommended target section. |

### System Context metrics (Step 1)

If the generated ERD includes a **System Context** block (Confluence/GitHub summaries as written in the document), add the following. **Do not** verify against live GitHub or Confluence—see `system_context_schema.md`.

| Metric | Meaning | How to compute |
|--------|---------|----------------|
| **System Context structural completeness** | Expected subsections/tables for context summaries. | Checklist in `system_context_schema.md`; **%** = (passed / applicable) × 100. **N/A** if no System Context block. |
| **Context–PRD alignment** | Context themes map to PRD requirements. | Numerator / denominator of context items mapping to PRD; **%** as in `system_context_schema.md`. **N/A** if no block. |
| **Context relevance** | On-topic vs filler for this program. | **Relevance %** per `system_context_schema.md`. **N/A** if no block. |
| **Unsupported / contradictory context claims** | Contradicts PRD or lacks PRD basis. | Rate % per `system_context_schema.md`. **N/A** if no substantive claims to score. |

---

## 1. Evaluation Summary

- **Documents evaluated:** PRD (filename or “uploaded”), Generated ERD (filename or “uploaded”).
- **Overall result:** Pass / Pass with gaps / Fail.
- **One-line summary:** e.g. "All PRD capabilities are covered; 2 invented epics and 1 missing NFR."

## 2. Quantitative Metrics

Present a **summary table** (required). **Every row must include a confidence** for that metric (not only a global note).

| Metric | Result | Basis (counts) | Confidence | Why (1 short phrase) |
|--------|--------|----------------|------------|----------------------|
| Structural completeness | …% | e.g. sum 6.5 / 7 section weights | Low / Med / High | e.g. one section ambiguous in the uploaded document |
| PRD coverage | …% | e.g. 18 / 20 PRD items | Low / Med / High | e.g. PRD unstructured |
| Traceability accuracy | …% | e.g. 16 / 18 links | Low / Med / High | e.g. few trace rows |
| Hallucination rate | …% | e.g. 2 / 45 substantive elements | Low / Med / High | e.g. sampled subset of cells |
| Content fidelity (PRD) | …% | formula + adjustments | Low / Med / High | inherits weakest input or doc noise |
| Capability label accuracy | …% | e.g. 12 / 14 ENG rows match PRD capability | Low / Med / High | e.g. PRD capability names ambiguous |
| Section classification accuracy | …% | e.g. 42 / 45 items in correct section | Low / Med / High | e.g. monitoring items in Functional |
| System Context structural completeness | …% or N/A | passed / applicable checks | Low / Med / High | omit row if no System Context block |
| Context–PRD alignment | …% or N/A | e.g. 12 / 15 context items | Low / Med / High | … |
| Context relevance | …% or N/A | e.g. 13 / 15 relevant | Low / Med / High | … |
| Unsupported / contradictory context claims | …% or N/A | e.g. 1 / 20 claims | Low / Med / High | … |

**Overall confidence in Step 1 scores** (required): **Low / Medium / High** plus **one sentence** on what limits certainty (e.g. PRD length, table extraction quality, overlapping requirements).

**Audit trail (recommended for defensibility):** After the summary table, add a short **“Counting basis”** subsection: how PRD items were enumerated for coverage, what counted as a “substantive ERD element” for hallucination rate (and, when System Context exists, that **context-traced** non-PRD items are **excluded** from the hallucination numerator and listed in §4b), and what counted as a “trace link” for traceability—so percentages are not opaque.

Then add **2–4 bullets**:

- **Interpretation:** What the headline numbers mean for this run (e.g. high coverage but high hallucination rate ⇒ risky).
- **Per-metric caveats:** Where a percentage is fragile (thin denominators, inferred PRD item list).

## 3. Completeness (Gaps)

- List every PRD requirement, capability, or explicit deliverable that is **missing** from the generated ERD.
- Format: table or bullets with columns/fields:
  - **PRD reference** (e.g. FR-001, section name)
  - **Description** (what is missing)
  - **Suggested ERD location** (e.g. Functional Engineering Deliverables, Engineering Epics)
- If none: state "No gaps; all PRD requirements are reflected in the ERD."

### Misclassified deliverables

- List every ENG-XXX or NFR-XXX item that appears in the **wrong section** (Functional vs Non-Functional). For each:
  - **ERD ID** (e.g. ENG-013)
  - **Current section** (e.g. Functional Engineering Deliverables)
  - **Recommended section** (e.g. Non-Functional Engineering Deliverables)
  - **Reason** (e.g. "describes internal monitoring and alerting — non-functional by nature")
- Items whose descriptions primarily concern **internal monitoring, alerting, reporting, observability, SLA tracking, operational dashboards, or audit compliance** should be classified as non-functional. Items describing **new features, API endpoints, workflow logic, or user-facing capabilities** should be classified as functional.
- If none: state "All deliverables are in the correct section."

### Structural recommendations

Surface these as standing recommendations when applicable (the evaluator cannot verify correctness, but can flag the omission):

- **Owner attribution:** If the Functional Engineering Deliverables table has **no Owner or Team column**, recommend adding one so each ENG-XXX row names a responsible team or individual. The evaluator cannot determine correct owners from the PRD and ERD alone, but the absence of the column is a structural gap that engineering teams consistently flag.
- **Repository coverage:** If any ENG-XXX row describes **frontend, UI, banner, dashboard, or client-side** work but the ERD (System Context, Dependencies, or deliverable description) declares **no frontend/UI repository**, flag this as a potential gap. The evaluator cannot verify which repositories exist, but the mismatch between UI-scoped deliverables and backend-only repo declarations is worth surfacing.

## 4. Hallucination / Unsourced Content

**Scope:** This section has two parts when the generated ERD includes a **System Context** block; otherwise use **§4a** only (same as historical behavior).

### 4a. Hallucinations (not in PRD, not in System Context)

- **Do not treat generated IDs as hallucination.** AutoSpec assigns FR-001, ENG-001, EPIC-001, etc. when the PRD has no explicit IDs; that is expected. List only **content** with **no** basis in the PRD and **no** substantiating match in the **System Context** block: invented capabilities, deliverable/epic names or descriptions, milestone labels, or other substantive text outside System Context (not the IDs themselves).
- **“Substantive match” in System Context** means the **same or clearly equivalent** fact, number, product name, repo, endpoint detail, or theme appears in the System Context narrative/tables (Confluence/GitHub summaries **as written in the ERD**). The evaluator does **not** access live wikis or GitHub; matching is **text-in-document** only. Vague or generic context (“we use modern APIs”) does **not** automatically absolve a specific unsourced ERD claim—use judgment and state the link briefly when you move an item to §4b.
- **Process/generation metadata need not be flagged.** Boilerplate such as the Change Log line "Generated from approved PRD Analysis, Engineering Spec, and JIRA Work Plan" describes the AutoSpec workflow, not PRD-derived content; do not list it as unsourced unless you are explicitly evaluating that claim.
- Format: table or bullets with:
  - **ERD location** (section + row or element; exclude System Context subsections from this table *unless* the same text is duplicated in main deliverables and unsourced)
  - **Content** (short description of the unsourced item – not ID tokens like FR-001)
  - **Reason** (e.g. "No matching PRD content; not stated in System Context")
- If none: state "No hallucinated or unsourced content found outside of System Context–traceable material."

### 4b. Traced to System Context (not in PRD) — *only when a System Context block exists*

- **Not exported to Excel or Power Automate:** This sub-section is for the **in-chat Markdown report** only. The Excel **`PRD_Analysis`** sheet and the **`submitEvaluationReport`** / `prdAnalysis` JSON carry **`hallucinations` (§4a) only**—see `xlsx_report_export.md` and `power_automate_report.md`.
- Use this sub-section for substantive ERD claims (in **Functional / NFR / Epics / E2E** or other **non–System-Context** tables) that **do not** appear in the **PRD** but **do** have a **clear** basis in the **System Context** text in the same generated ERD.
- For each row:
  - **ERD location** (section + row)
  - **Content** (the claim)
  - **System Context reference** (short pointer: e.g. “GitHub — repo X, key findings row”, “Confluence — page title Y”)
  - **Note (required, fixed intent):** State that the content **was not found in the PRD** but **was traced to the System Context** section; the **evaluator cannot verify** whether it is factually **correct** (no access to Confluence/GitHub). This is **not** scored as a hallucination; it is **excluded** from the **Hallucination rate** numerator; it **does** help readers see sourcing beyond the PRD.
- If there are no such items: state "No non-PRD substantive claims were solely explained by System Context" or omit the sub-table if the block is empty.
- If the generated ERD has **no** System Context section, **omit §4b** entirely.

## 5. Traceability Summary

- Count of PRD requirements (by section or capability) and how many have a clear trace in the ERD. ERD may use AutoSpec-generated IDs (FR-xxx, ENG-xxx) when the PRD has no explicit IDs; assess whether each such ID **maps correctly** to a PRD requirement, not whether the ID appears in the PRD text.
- Note any broken or missing mappings (e.g. ENG with no logical PRD requirement).
- Milestone format check: confirm use of `M<number>` only; list any violations.

### Capability label audit

- Build the **canonical capability list** from the PRD (one entry per distinct capability heading or feature grouping).
- For each ENG-XXX row, check whether the Capability field value matches one of the canonical names (case-insensitive, minor punctuation tolerance).
- List every **mismatch** as a table row:
  - **ENG ID** (e.g. ENG-006)
  - **Current capability label** (what the ERD says)
  - **Nearest PRD capability** (what it should say, or "no match found")
  - **Issue** (e.g. "paraphrased", "merged two capabilities", "invented label")
- If all labels match: state "All ENG-XXX capability labels match PRD-defined capabilities."

## 6. Recommendations

- Prioritized list of actions (e.g. "Add NFR for X from PRD section Y", "Remove epic Z (unsourced)", "Fix milestone format in Epics table"). Distinguish **true** unsourced items (§4a) from **context-only** sourcing (§4b)—recommendations may ask product/engineering to **align PRD** with context-backed facts *or* to **narrow** System Context if it introduces scope not in the PRD.
- Reminder: "If you have an existing ERD from your engineering team (**DOCX** or **Markdown**), you can share it for a structure comparison (Document Metadata, Monthly Delivery Summary, Functional/Non-Functional Deliverables, Engineering Epics, E2E Testing Plan, **System Context** when present)."

---

**Output:** Markdown only. No raw JSON in the user-facing report.
