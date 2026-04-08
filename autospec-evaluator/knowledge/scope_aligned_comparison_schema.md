# Step 3: Scope-Aligned Comparison – Generated ERD vs Engineering ERD (Subset Scope)

Use this schema **only after Step 2 is complete** and the user has **accepted** a scope-aligned re-analysis (or explicitly asked for it).

**System Context:** See **`system_context_schema.md`** for scoped evaluation of the seventh section.

## When Step 3 applies

Engineering ERDs often reflect **a slice of work** (e.g. one or two months, a single release, a subset of epics or deliverables) while the **generated ERD** may reflect **full PRD scope**. Step 2 compares **whole documents**, which can **unfairly lower scores** (extra rows, broader epics, fuller NFR/E2E tables in the generated ERD).

Step 3 re-runs the same **spirit** as Step 2—engineering ERD remains the reference—but only for the **overlap scope** the engineering ERD actually covers.

**System Context:** When present, include **System Context** as the **seventh** section (same checks + five facets, **scoped**). See **`system_context_schema.md`**.

## Preconditions

- Step 1 (PRD + generated ERD) and Step 2 (engineering ERD) are already done.
- You have **inferred the engineering ERD’s effective scope** (see below) and the user confirmed they want Step 3 (or asked for “scoped” / “month-only” comparison).

## 1. Declared comparison scope (required, first section)

State clearly:

- **Engineering ERD scope (inferred):** e.g. “January–February delivery”, “Epics A/B only”, “Functional deliverables rows referencing FR-010–FR-015”, or “Monthly summary names Project X only”.
- **How scope was inferred:** cite evidence (dates in metadata or Monthly Delivery Summary, epic titles/milestones, row labels, Jira keys, explicit month headers).
- **Generated ERD slice:** which parts of the generated ERD you are treating as **in scope** for this pass (same rows/topics/time window). If the generated ERD does not segment cleanly, state your **best-effort mapping** and **confidence**.

If scope cannot be inferred with reasonable confidence, say so, list options, and ask the user to **name the slice** (month, epic IDs, or FR range) before scoring.

## 2. Lenient rules (Step 3 only)

Apply these **instead of** strict whole-document expectations from Step 2:

| Topic | Step 2 (strict) | Step 3 (lenient) |
|--------|------------------|------------------|
| **Extra content in generated ERD** | Can read as misalignment | **Ignore or lightly note** deliverables/epics/NFRs/E2E **outside** the declared engineering scope. Do not score “does not match” solely because the generated ERD is broader. |
| **Row / epic counts** | Compared to full reference | Compare **only within the scoped subset** (counts, granularity, style). |
| **Empty or thin sections in engineering ERD** | May drag scores | If the engineering artifact is empty for a section **because scope is intentionally narrow**, use **N/A** for that section’s metrics (re-normalize), or explain **low check counts**—not a fake **0** for “missing volume.” |
| **Structural match** | Full table parity | Run **structure checks** only on **scoped rows/columns**; see §3. |
| **Content alignment** | Full-section style match | Score **five facets** only on **in-scope** text/rows; see §3. |

**Still strict where it matters:** invented content, wrong milestone format, or **in-scope** rows that contradict the engineering ERD should still be flagged. Lenience is about **scope mismatch**, not factual or format nonsense inside the slice.

## 3. Metric definitions (Step 3)

Use the **same computation model as Step 2** (`structure_analysis_schema.md`): **structure %** from **passed/applicable checks**, **content %** from **five facets × 0 / 0.5 / 1**, per section. **Scope difference:** checks and facets are evaluated only on the **declared slice** (in-scope rows, epics, months).

- **Do not** replace numbers with only 0/50/100. Always show **% + basis** (checks and facet sums).
- **Ledgers (required):** Same **Structure check ledger** and **Content facet ledger** tables as Step 2 (`structure_analysis_schema.md` → Auditability), evaluated **only on the declared slice**. Do not report `x/y` or `n/5` without ledgers.
- **N/A** sections: exclude from averages; re-normalize.
- **Scope fairness note** (required): compare Step 3 headline **combined %** to Step 2’s combined % when both exist (e.g. “Step 2 combined 42%; Step 3 scoped combined 78%; delta driven by excluding 35 out-of-scope generated rows from structure checks”).

**Summary table (required):**

| Metric | Result | How obtained | Confidence | Why |
|--------|--------|--------------|------------|-----|
| Structural alignment (scoped) | …% | Mean of section structure % (slice only) | Low / Med / High | e.g. fuzzy slice boundary |
| Content alignment (scoped) | …% | Mean of section content % (slice only) | Low / Med / High | … |
| Combined engineering alignment (scoped) | …% | (Structural + Content) / 2 | Low / Med / High | … |

**Overall confidence** (required): **Low / Medium / High** + sentence (slice inference quality, row mapping uncertainty).

**Per-section table** (same columns as Step 2, label title **“Scoped to declared engineering scope”**):

| Section | Structure % | Structure basis (checks, slice only) | Content % | Content basis (5 facets) | Band (optional) |
|---------|-------------|----------------------------------------|-----------|----------------------------|-------------------|
| … | …% | x/y | …% | e.g. 3.5/5 | … |

Optional **Step 2 vs Step 3** mini-table: headline metrics side-by-side with one-line explanation.

## 4. Section-by-section analysis

Mirror Step 2’s section order (Document Metadata through **System Context**). For each:

- **Structure check ledger** and **Content facet ledger** (required for scored sections): same table formats as Step 2; label checks/facets as **scoped** (e.g. “STATUS column for in-scope functional rows”).
- **Structure:** reference = engineering ERD **within scope**; assess generated ERD **only for the matching slice**; ledger rows must match **x/y**.
- **Content:** score **five facets** on in-scope content only; ledger rows must match facet sum.
- **Out-of-scope generated content:** optional bullet list “Present in generated ERD but outside engineering slice—not scored as gaps in Step 3.”

## 5. Scoped alignment recommendations

Same rules as Step 2: recommend changes **to the generated ERD** so **in-scope** parts align with the engineering ERD. You may add a **non-blocking** note if trimming or labeling out-of-scope work in the generated ERD would make future Step 2 comparisons clearer.

## 6. Closing

Remind the user: Step 3 scores are **valid only for the declared slice**; Step 1 (full PRD vs generated) remains the authority for **full PRD coverage** and hallucinations across the whole generated ERD.

Then apply **Step 3 closing** in `workflow_operations.md`: offer **Excel export** per `xlsx_report_export.md` when the user asks—**Report_Overview** plus detail sheets for every completed evaluation (not scoped sheet only).

---

**Output:** Markdown only (plus optional **.xlsx**: see `xlsx_report_export.md` for sheet limits; Step 4 may add a fifth sheet when completed).
