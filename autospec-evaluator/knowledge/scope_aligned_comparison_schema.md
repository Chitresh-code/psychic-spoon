# Step 2 (reduced scope): Same delivery scope — generated vs team ERD (subset)

Use this schema when **Step 2’s scope gate** (`workflow_operations.md`) is satisfied and the user chose **reduced scope** (overlap slice only)—not the separate historical “Step 3 after Step 2” flow.

**Reader-facing name:** **Same delivery scope** (Excel tab `Same_scope`). Explains that only the **overlap slice** of the team ERD is scored.

**System Context:** See **`system_context_schema.md`** for scoped evaluation of the seventh section.

## When this schema applies

Engineering ERDs often reflect **a slice of work** (e.g. one or two months, a single release, a subset of epics or deliverables) while the **generated ERD** may reflect **full PRD scope**. A **full-scope** whole-document comparison can **unfairly lower scores** (extra rows, broader epics, fuller NFR/E2E tables in the generated ERD).

This pass uses the same **spirit** as full-scope Step 2—engineering ERD remains the reference—but only for the **overlap scope** the engineering ERD actually covers.

**System Context:** When present, include **System Context** as the **seventh** section (same **five content facets**, **scoped**; optional structure bullets). See **`system_context_schema.md`**.

## Preconditions

- Step 1 (PRD + generated ERD) is done; the user provided the engineering ERD for Step 2.
- The scope gate identified **partial PRD coverage** (or ambiguous scope with user choice) and the user replied **reduced scope** (or equivalent).
- You have **inferred the engineering ERD’s effective scope** (see §1 below) and will declare it in the report.

## 1. Declared comparison scope (required, first section)

State clearly:

- **Engineering ERD scope (inferred):** e.g. “January–February delivery”, “Epics A/B only”, “Functional deliverables rows referencing FR-010–FR-015”, or “Monthly summary names Project X only”.
- **How scope was inferred:** cite evidence (dates in metadata or Monthly Delivery Summary, epic titles/milestones, row labels, Jira keys, explicit month headers).
- **Generated ERD slice:** which parts of the generated ERD you are treating as **in scope** for this pass (same rows/topics/time window). If the generated ERD does not segment cleanly, state your **best-effort mapping** and **confidence**.

If scope cannot be inferred with reasonable confidence, say so, list options, and ask the user to **name the slice** (month, epic IDs, or FR range) before scoring.

## 2. Lenient rules (reduced scope only)

Apply these **instead of** strict whole-document expectations from **full-scope** Step 2 (`structure_analysis_schema.md`):

| Topic | Full scope (strict) | Reduced scope (lenient) |
|--------|------------------|------------------|
| **Extra content in generated ERD** | Can read as misalignment | **Ignore or lightly note** deliverables/epics/NFRs/E2E **outside** the declared engineering scope. Do not score “does not match” solely because the generated ERD is broader. |
| **Row / epic counts** | Compared to full reference | Compare **only within the scoped subset** (counts, granularity, style). |
| **Empty or thin sections in engineering ERD** | May drag scores | If the engineering artifact is empty for a section **because scope is intentionally narrow**, use **N/A** for that section’s metrics (re-normalize), or explain **low check counts**—not a fake **0** for “missing volume.” |
| **Layout / columns** | Full-table parity expected | Note **material** layout/column gaps only in **short bullets** for the scoped slice—**no** structure %. |
| **Content alignment** | Full-section style match | Score **five facets** only on **in-scope** text/rows; see §3. |

**Still strict where it matters:** invented content, wrong milestone format, wrong capability labels, misclassified deliverables (monitoring/alerting in Functional instead of NFR), or **in-scope** rows that contradict the engineering ERD should still be flagged. Lenience is about **scope mismatch**, not factual, classification, or format errors inside the slice.

## 3. Metric definitions (reduced scope)

Use the **same content-first model as full-scope Step 2** (`structure_analysis_schema.md`): **content %** from **five facets × 0 / 0.5 / 1**, per section. **Scope difference:** facets are evaluated only on the **declared slice** (in-scope rows, epics, months).

- **Do not** replace numbers with only 0/50/100. Always show **content % + facet basis** (e.g. `3.5/5`).
- **Ledgers (required):** Same **Content facet ledger** as full-scope Step 2, evaluated **only on the declared slice**. Optional short **structure bullets** where material. Do not report facet sums without the five ledger rows.
- **N/A** sections: exclude from averages; re-normalize.
- **Scope fairness note** (required): **If** the user also completed a **full-scope** Step 2 pass on the same pair in this session, compare **Content alignment (scoped)** % to that pass’s **Content alignment** % (e.g. “Full-scope content 42%; reduced-scope content 78%; delta driven by scoring only the January–February slice”). **Otherwise** one sentence: scores reflect **only** the declared slice, not whole-document parity—no delta table required.

**Summary table (required):**

| Metric | Result | How obtained | Confidence | Why |
|--------|--------|--------------|------------|-----|
| Content alignment (scoped) | …% | Mean of section content % (slice only) | Low / Med / High | … |

**Overall confidence** (required): **Low / Medium / High** + sentence (slice inference quality, row mapping uncertainty).

**Per-section table** (same columns as Step 2, label title **“Scoped to declared engineering scope”**):

| Section | Content % | Content basis (5 facets) | Band (optional) |
|---------|-----------|----------------------------|-----------------|
| … | …% | e.g. 3.5/5 | … |

Optional **full-scope vs reduced-scope** mini-table: headline metrics side-by-side **only when both passes exist** in the session; otherwise omit.

## 4. Section-by-section analysis

Mirror Step 2’s section order (Document Metadata through **System Context**). For each:

- **Content facet ledger** (required for scored sections): same table format as Step 2; label facets as **scoped** (e.g. “granularity for in-scope functional rows only”).
- **Structure:** optional bullets only—reference = engineering ERD **within scope**; assess generated ERD **only for the matching slice** when layout gaps matter.
- **Content:** score **five facets** on in-scope content only; ledger rows must match facet sum.
- **Out-of-scope generated content:** optional bullet list “Present in generated ERD but outside engineering slice—not scored as gaps in this reduced-scope pass.”

## 5. Scoped alignment recommendations

Same rules as Step 2: recommend changes **to the generated ERD** so **in-scope** parts align with the engineering ERD. You may add a **non-blocking** note if trimming or labeling out-of-scope work in the generated ERD would make future Step 2 comparisons clearer.

## 6. Closing

Remind the user: reduced-scope scores are **valid only for the declared slice**; Step 1 (full PRD vs generated) remains the authority for **full PRD coverage** and hallucinations across the whole generated ERD.

Then apply **Step 2 closing** in `workflow_operations.md`: offer **Excel export** per `xlsx_report_export.md` when the user asks—**Summary** sheet plus detail sheets for every completed evaluation.

---

**Output:** Markdown only (plus optional **.xlsx**: see `xlsx_report_export.md` for sheet limits; Step 4 may add a fifth sheet when completed).
