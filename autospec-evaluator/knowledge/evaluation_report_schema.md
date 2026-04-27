# Step 1: PRD vs Generated ERD – Evaluation Report Schema

Use this structure for the evaluation report after comparing the PRD to the generated ERD (**DOCX** or **Markdown**).

**Reader-facing name:** In chat, filenames, and handoffs, call this deliverable **PRD vs generated ERD** (same vocabulary as `xlsx_report_export.md`). Avoid leading with “Step 1” unless you are speaking to operators who use internal step numbers.

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
| **PRD coverage** | Share of PRD-trackable scope reflected in the ERD. | Build a **denominator**: count discrete PRD items you treat as in-scope (capabilities, functional requirements, explicit deliverables, and other explicit commitments in the PRD—be consistent and list the counting rule in the report). **Numerator**: how many have a **clear, defensible** reflection in the ERD (any reasonable section). **PRD coverage %** = (numerator / denominator) × 100. |
| **Traceability accuracy** | Quality of **ID/content mapping** from the ERD back to the PRD (FR/ENG/EPIC and similar links). | **Denominator**: number of trace links you assess (e.g. ENG rows, FR references, epics tied to requirements—exclude bare AutoSpec ID tokens as “errors” by themselves). **Numerator**: links that **correctly** map to a PRD item. Broken, missing, or invented mappings count in the denominator as **not** accurate. **Traceability accuracy %** = (numerator / denominator) × 100. **Capability labels** and **Functional vs NFR placement** are **not** separate rows in §2; they are scored and listed under **§5 Traceability** (subsections **5.2** and **5.3**) with their own numerators/denominators for audit. |
| **Hallucination rate** | Share of reviewed ERD **substance** (outside System Context) that is a **true** hallucination: not supported by the PRD and **not** plausibly sourced from the **System Context** block. | **Denominator**: count of **substantive** ERD elements you review for this metric—typically **deliverables, epics, NFRs, and similar rows outside the System Context section** (same counting rule as before; **not** AutoSpec-generated IDs alone, not empty cells). **Do not** include the System Context *section’s own* narrative rows in this denominator; treat that block separately in **System Context (narrative)** after the §2 table when present. **Numerator**: elements that lack PRD support **and** for which you **cannot** find an adequate match in the **System Context** text—list the rest in **§4b**, not here. **Hallucination rate %** = (numerator / denominator) × 100. If the generated ERD has **no** System Context block, use PRD-only unsourced logic for the numerator. If zero items qualify, the rate can be **0%** with denominator stated. |
| **Content fidelity (PRD)** | Single rollup of how faithful ERD content is to the PRD, combining coverage and hallucinations. | **Default formula:** Content fidelity % = **PRD coverage % × (1 − hallucination rate / 100)**, using the **hallucination rate** that **excludes** context-traced items (above). If hallucination rate is N/A, use **PRD coverage %** as fidelity and say so. Optionally add **−5** percentage points per **severe** traceability break (max −15), documented in the metrics notes. |

### System Context (Step 1) — narrative only (no §2 table row)

If the generated ERD includes a **System Context** block, **do not** add any System Context line to the §2 **Quantitative Metrics** summary table. **Do not** verify against live GitHub or Confluence.

After **Counting basis** (and before or within the **Interpretation** bullets), add a short subsection **System Context (when present)** using `system_context_schema.md`: cover **shape/structure**, **alignment to PRD**, **relevance**, and **unsupported or contradictory claims** (you may state **optional** percentages per dimension in prose for auditability—**no** merged headline score).

---

## 1. Evaluation Summary

- **Documents evaluated:** PRD (filename or “uploaded”), Generated ERD (filename or “uploaded”).
- **Overall result:** Pass / Pass with gaps / Fail.
- **One-line summary:** e.g. "All PRD capabilities are covered; 2 invented epics and 1 missing NFR."

## 2. Quantitative Metrics

Present a **summary table** (required). **Every row must include a confidence** for that metric (not only a global note).

| Metric | Result | Basis (counts) | Confidence | Why (1 short phrase) |
|--------|--------|----------------|------------|----------------------|
| PRD coverage | …% | e.g. 18 / 20 PRD items | Low / Med / High | e.g. PRD unstructured |
| Traceability accuracy | …% | e.g. 16 / 18 links | Low / Med / High | e.g. few trace rows |
| Hallucination rate | …% | e.g. 2 / 45 substantive elements | Low / Med / High | e.g. sampled subset of cells |
| Content fidelity (PRD) | …% | formula + adjustments | Low / Med / High | inherits weakest input or doc noise |

**Overall confidence in Step 1 scores** (required): **Low / Medium / High** plus **one sentence** on what limits certainty (e.g. PRD length, table extraction quality, overlapping requirements).

**Audit trail (recommended for defensibility):** After the summary table, add a short **“Counting basis”** subsection: how PRD items were enumerated for coverage, what counted as a “substantive ERD element” for hallucination rate (and, when System Context exists, that **context-traced** non-PRD items are **excluded** from the hallucination numerator and listed in §4b), and what counted as a “trace link” for **traceability accuracy**. Major **section** omissions (missing Epics table, etc.) belong in **§3 Completeness**, not as extra rows in §2.

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

- **Not in Excel export:** This sub-section is for the **in-chat Markdown report** only. The Excel **`PRD_vs_generated`** sheet lists **§4a / true hallucinations only**—see `xlsx_report_export.md`.
- Use this sub-section for substantive ERD claims (in **Functional / NFR / Epics / E2E** or other **non–System-Context** tables) that **do not** appear in the **PRD** but **do** have a **clear** basis in the **System Context** text in the same generated ERD.
- For each row:
  - **ERD location** (section + row)
  - **Content** (the claim)
  - **System Context reference** (short pointer: e.g. “GitHub — repo X, key findings row”, “Confluence — page title Y”)
  - **Note (required, fixed intent):** State that the content **was not found in the PRD** but **was traced to the System Context** section; the **evaluator cannot verify** whether it is factually **correct** (no access to Confluence/GitHub). This is **not** scored as a hallucination; it is **excluded** from the **Hallucination rate** numerator; it **does** help readers see sourcing beyond the PRD.
- If there are no such items: state "No non-PRD substantive claims were solely explained by System Context" or omit the sub-table if the block is empty.
- If the generated ERD has **no** System Context section, **omit §4b** entirely.

## 5. Traceability Summary

**§5 is the home for all trace-related audit:** ID mapping (headline **Traceability accuracy** in §2), **capability labels**, and **Functional vs NFR placement**. Do **not** duplicate capability or placement as separate rows in §2.

### 5.1 ID and content mapping (supports §2 Traceability accuracy)

- Count of PRD requirements (by section or capability) and how many have a clear trace in the ERD. ERD may use AutoSpec-generated IDs (FR-xxx, ENG-xxx) when the PRD has no explicit IDs; assess whether each such ID **maps correctly** to a PRD requirement, not whether the ID appears in the PRD text.
- Note any broken or missing mappings (e.g. ENG with no logical PRD requirement).
- Milestone format check: confirm use of `M<number>` only; list any violations.

### 5.2 Capability label audit

- **Optional headline here (not in §2):** Report **Capability label match %** = (matching ENG-XXX Capability fields / ENG-XXX rows with a Capability field) × 100, with numerator/denominator—same rule as historical “capability label accuracy.”
- Build the **canonical capability list** from the PRD (one entry per distinct capability heading or feature grouping).
- For each ENG-XXX row, check whether the Capability field value matches one of the canonical names (case-insensitive, minor punctuation tolerance).
- List every **mismatch** as a table row:
  - **ENG ID** (e.g. ENG-006)
  - **Current capability label** (what the ERD says)
  - **Nearest PRD capability** (what it should say, or "no match found")
  - **Issue** (e.g. "paraphrased", "merged two capabilities", "invented label")
- If all labels match: state "All ENG-XXX capability labels match PRD-defined capabilities."

### 5.3 Section placement (Functional vs Non-Functional)

- **Optional headline here (not in §2):** Report **Section placement %** = (items in the correct section / total ENG-XXX and NFR-XXX items assessed) × 100—same rules as historical “section classification accuracy” (monitoring/alerting/observability → NFR; features/APIs/workflows → Functional). Misplaced items must also appear in **§3 Misclassified deliverables**.
- If all items are correctly placed: state "All assessed deliverables are in the correct section."

## 6. Recommendations

- Prioritized list of actions (e.g. "Add NFR for X from PRD section Y", "Remove epic Z (unsourced)", "Fix milestone format in Epics table"). Distinguish **true** unsourced items (§4a) from **context-only** sourcing (§4b)—recommendations may ask product/engineering to **align PRD** with context-backed facts *or* to **narrow** System Context if it introduces scope not in the PRD.
- Reminder: "If you have an existing ERD from your engineering team (**DOCX** or **Markdown**), you can share it for a structure comparison (Document Metadata, Monthly Delivery Summary, Functional/Non-Functional Deliverables, Engineering Epics, E2E Testing Plan, **System Context** when present)."

---

**Output:** Markdown only. No raw JSON in the user-facing report.
