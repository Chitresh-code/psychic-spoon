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
| **Hallucination rate** | Share of reviewed ERD **substance** that is unsourced. | **Denominator**: count of **substantive** ERD elements you reviewed (e.g. deliverable descriptions, epic narratives, NFR statements—**not** AutoSpec-generated IDs alone, not empty cells). **Numerator**: elements you classify as **unsourced / hallucinated** per the rules in §4. **Hallucination rate %** = (numerator / denominator) × 100. If you flagged zero unsourced items, the rate can be **0%** with denominator stated. |
| **Content fidelity (PRD)** | Single rollup of how faithful ERD content is to the PRD, combining coverage and hallucinations. | **Default formula:** Content fidelity % = **PRD coverage % × (1 − hallucination rate / 100)**, using the percentages above (cap at 100, floor at 0). If hallucination rate is N/A, use **PRD coverage %** as fidelity and say so. Optionally add **−5** percentage points per **severe** traceability break (max −15), documented in the metrics notes. |

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
| System Context structural completeness | …% or N/A | passed / applicable checks | Low / Med / High | omit row if no System Context block |
| Context–PRD alignment | …% or N/A | e.g. 12 / 15 context items | Low / Med / High | … |
| Context relevance | …% or N/A | e.g. 13 / 15 relevant | Low / Med / High | … |
| Unsupported / contradictory context claims | …% or N/A | e.g. 1 / 20 claims | Low / Med / High | … |

**Overall confidence in Step 1 scores** (required): **Low / Medium / High** plus **one sentence** on what limits certainty (e.g. PRD length, table extraction quality, overlapping requirements).

**Audit trail (recommended for defensibility):** After the summary table, add a short **“Counting basis”** subsection: how PRD items were enumerated for coverage, what counted as a “substantive ERD element” for hallucination rate, and what counted as a “trace link” for traceability—so percentages are not opaque.

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

## 4. Hallucination / Unsourced Content

- **Do not treat generated IDs as hallucination.** AutoSpec assigns FR-001, ENG-001, EPIC-001, etc. when the PRD has no explicit IDs; that is expected. List only **content** that has no basis in the PRD: invented capabilities, deliverable/epic names or descriptions, milestone labels, or other substantive text (not the IDs themselves).
- **Process/generation metadata need not be flagged.** Boilerplate such as the Change Log line "Generated from approved PRD Analysis, Engineering Spec, and JIRA Work Plan" describes the AutoSpec workflow, not PRD-derived content; do not list it as unsourced unless you are explicitly evaluating that claim.
- Format: table or bullets with:
  - **ERD location** (section + row or element)
  - **Content** (short description of the unsourced item – not ID tokens like FR-001)
  - **Reason** (e.g. "No matching PRD capability or requirement")
- If none: state "No unsourced or hallucinated content found."

## 5. Traceability Summary

- Count of PRD requirements (by section or capability) and how many have a clear trace in the ERD. ERD may use AutoSpec-generated IDs (FR-xxx, ENG-xxx) when the PRD has no explicit IDs; assess whether each such ID **maps correctly** to a PRD requirement, not whether the ID appears in the PRD text.
- Note any broken or missing mappings (e.g. ENG with no logical PRD requirement).
- Milestone format check: confirm use of `M<number>` only; list any violations.

## 6. Recommendations

- Prioritized list of actions (e.g. "Add NFR for X from PRD section Y", "Remove epic Z (unsourced)", "Fix milestone format in Epics table").
- Reminder: "If you have an existing ERD from your engineering team (**DOCX** or **Markdown**), you can share it for a structure comparison (Document Metadata, Monthly Delivery Summary, Functional/Non-Functional Deliverables, Engineering Epics, E2E Testing Plan, **System Context** when present)."

---

**Output:** Markdown only. No raw JSON in the user-facing report.
