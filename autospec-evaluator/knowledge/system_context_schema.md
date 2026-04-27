# System Context — Steps 1–2 (full or reduced scope) and Step 4

Use this schema whenever the ERD contains a **System Context** block: narrative summaries labeled as being informed by **Confluence** and/or **GitHub**, typically including tables for wiki pages (titles, space, dates, freshness, key findings) and for repositories (branch, files read, findings, tech stack, ownership).

## Hard constraint (no external verification)

The evaluator has **no access to GitHub or Confluence**. Do **not** score “sourcing quality” against live repos or wiki pages. Treat all Confluence/GitHub content as **text inside the uploaded document** (DOCX or Markdown). Verification is **PRD ↔ ERD** and **ERD ↔ ERD** only.

---

## Steps 1–2 — System Context slice

### Where it appears in reports

- **Step 1:** **Do not** put System Context in the §2 numeric summary table. Summarize it in **System Context (when present)** narrative after **Counting basis** (see `evaluation_report_schema.md`). Also use System Context in **Completeness**, **Hallucination**, **Traceability**, and **Recommendations** when gaps concern context.

### Hallucination triage vs System Context (Step 1)

When flagging **unsourced** or **hallucinated** content in **Functional / NFR / Epics / E2E** (or similar) **outside** the System Context block:

1. **First** check the **PRD** for a defensible basis.
2. If missing in the PRD, **second** check whether the **same or clearly equivalent substance** appears in the generated ERD’s **System Context** section (Confluence/GitHub summaries **as written in the document**—no live verification).
3. **If** it appears in System Context: put it in **`evaluation_report_schema.md` §4b** (*Traced to System Context, not in PRD*) in the **Markdown** report, with a short pointer to where in System Context it appears, and the required disclaimer that the evaluator **cannot verify** external correctness. **Do not** count it in the **Hallucination rate** numerator; **do not** list it in §4a. **Do not** add §4b items to the Excel **`PRD_vs_generated`** sheet (that sheet lists **true hallucinations / §4a only**).
4. **If** it appears in **neither** PRD nor System Context: treat as a **hallucination** in §4a and count it in the **Hallucination rate** numerator (subject to the usual “no IDs / boilerplate” rules in `evaluation_report_schema.md`).

This does **not** remove the need to assess **unsupported or contradictory context claims** and overall fitness of the **System Context block**—only that there is **no merged % headline** in §2.
- **Step 2 (full scope):** Treat **System Context** as the **seventh canonical section** (after E2E Testing Plan). Follow **content-first** rules per `structure_analysis_schema.md` (§8): **Content facet ledger** required; structure notes optional.
- **Step 2 (reduced scope):** Apply the same **scoped** rules as other sections: evaluate System Context rows/themes only inside the **declared engineering slice** (see `scope_aligned_comparison_schema.md`).

### Assessment dimensions (Step 1 — System Context, narrative)

Use these to write **System Context (when present)** in the Step 1 report. You may include **optional %** per dimension in prose (same formulas); there is **no** §2 table row and **no** merged score.

All percentages **0–100** when shown. State **numerator / denominator** (or **passed / applicable**). If denominator is **0**, report **N/A** for that dimension in prose.

| Dimension | Meaning | How to compute |
|--------|---------|----------------|
| **Structural completeness** | The System Context block matches the **expected shape** (subsections + tables/fields). | **Checks** (adapt to the doc): Confluence summary subsection present; GitHub summary subsection present; Confluence rows have roles for title, space, last modified, freshness, key findings; GitHub area has repo, branch, files read (or equivalent), findings; optional tech stack and ownership tables. **%** = (passed / applicable) × 100. |
| **Context–PRD alignment** | How well **key findings and themes** in System Context **reflect** PRD requirements (capabilities, scope, dependencies, constraints, milestones). | **Denominator:** discrete context items scored (rows or themed paragraphs—state counting rule). **Numerator:** items that **map** to at least one PRD requirement/theme (cite PRD section or ID). **%** = (numerator / denominator) × 100. |
| **Context relevance** | Whether context is **on-topic** for this program vs generic or tangential filler. | Same denominator (or documented sample). **Numerator:** items **materially relevant** to the PRD problem domain and delivery narrative. **Relevance %** = (numerator / denominator) × 100. |
| **Unsupported / contradictory context claims** | Claims that **contradict the PRD** or add **major substance** with **no PRD basis** (no external verification required). | **Denominator:** substantive claims in System Context. **Numerator:** contradictions or unsupported major assertions. **Rate %** = (numerator / denominator) × 100. |

**Confidence:** Optional **Low / Med / High** for the System Context subsection as a whole, with one reason.

**Counting basis:** When you cite % in narrative, document what counted as one “context item” and one “substantive claim.”

### Step 2 (full scope) — System Context (seventh section)

- **Reference:** Engineering ERD only. Assess **generated** against **engineering** for this block.
- **Content-first (same as Step 2 overall):** Judge alignment using **content %** and the **Content facet ledger** only. You may note layout/column gaps briefly in narrative if they block usability; **do not** compute a separate **structure %** rollup for System Context or include structure in the Step 2 headline **Content alignment**.
- **Content %:** **Five facets** (document which five), each **0 / 0.5 / 1**, e.g.: subsections aligned; comparable theme coverage; compatible repo/stack **names as written**; compatible ownership narrative; table/style alignment. **No** live repo/wiki verification.
- **Ledgers:** **Content facet ledger** required; structure notes optional (short bullets if material).

### Step 2 — System Context (reduced scope / scoped)

- Same **content-first** model as full-scope Step 2; **facets** only on **in-scope** System Context rows/themes per declared slice.
- **N/A** if engineering ERD has no System Context for the slice; re-normalize section averages.

### Rollups

- **Step 1:** System Context has **no** numeric rollup in §2—narrative only (`evaluation_report_schema.md`).
- **Step 2 (either mode):** **Content alignment** (or **Content alignment (scoped)**) rollup is the **mean** of applicable section **content %** values only (**six or seven** sections, including **System Context** when not N/A). **Do not** compute or report a separate **structural alignment** %.

---

## Step 4 — Research context (two ERDs)

**Reader-facing name:** **Research context (two ERDs)** (Excel tab `Context_compare`). Internal “Step 4” is fine for operators only.

Run **after Step 2** completes (or when the user explicitly asks for Step 4 and Step 2 is already done). Requires **PRD** from Step 1 and **two ERDs’** System Context sections:

- **Option A:** Generated ERD vs **engineering ERD** (from Step 2), or  
- **Option B:** User uploads **another ERD** (DOCX or Markdown); compare that ERD’s System Context to the **generated** ERD (or clarify pair if user specifies).

**No external verification.** Use PRD + in-document text only.

### Report section order (strict)

1. Step 4 Summary (documents compared, option A or B, PRD used)
2. **Quantitative Metrics** — use **two tables** (format below); do **not** hide per-ERD scores inside parity-only wording
3. PRD mapping / alignment ledger (optional but recommended)
4. Contradiction ledger
5. Interpretation (narrative); **do not** duplicate the metric tables here—reference §2

### Quantitative Metrics — required layout (Step 4)

Show **scores for each ERD explicitly**, then a **small pair table**. **Every row** must include **Confidence** (Low / Med / High), **How obtained** (counts, formula, or facet sum), and **Why** (1–2 short phrases: what drives the number, what limits certainty).

#### Table A — Per-ERD metrics (required)

Compute **separately** for **ERD A** and **ERD B** using the **same PRD**. Use the **same definitions** as **Context–PRD alignment** and **Context relevance** in the Step 1 dimension table above (no merged headline).

| Metric | ERD A (%) | ERD B (%) | Confidence | How obtained | Why |
|--------|-----------|-----------|------------|--------------|-----|
| **Context–PRD alignment** | … | … | Low / Med / High | Numerator/denominator per ERD; cite counting rule | e.g. thin rows on B, placeholder text |
| **Context relevance** | … | … | Low / Med / High | Numerator/denominator per ERD | e.g. one doc mostly placeholders |

Use **N/A** in an ERD column only if that ERD truly has no scorable System Context block (state why).

**Derived (optional row):** **PRD alignment gap** = \|ERD A − ERD B\| percentage points on **Context–PRD alignment** (not a substitute for showing both scores).

#### Table B — Pair metrics (required, slim)

| Metric | Result | Confidence | How obtained | Why |
|--------|--------|------------|--------------|-----|
| **Relevance consistency (qualitative)** | e.g. Mixed — large mismatch | Low / Med / High | Compare Table A **relevance %** A vs B | short justification |
| **Overall Step 4 score** | e.g. 72% | Low / Med / High | **Default:** arithmetic mean of the **four** Table A percentages (alignment A, alignment B, relevance A, relevance B)—exclude any **N/A** and re-normalize; **state formula** | which inputs dominated |

**Do not** report **Cross-ERD thematic consistency**, **Inter-ERD consistency score**, or **Mention overlap** in chat or Excel exports for this product—those metrics were retired as low-signal. Use the **contradiction ledger** for substantive tensions between the two documents.

**Do not** replace Table A with parity-only metrics (e.g. “15% parity”) **without** also showing **ERD A** and **ERD B** **Context–PRD alignment** and **relevance** in Table A.

### Metric definitions (Step 4 — reference)

| Metric | Meaning | How to compute |
|--------|---------|----------------|
| **Context–PRD alignment** (per ERD) | Same as Step 1 dimension. | Table A: two independent percentages. |
| **Context relevance** (per ERD) | Same as Step 1 dimension. | Table A: two independent percentages. |
| **Overall Step 4 score** | Headline for the pair. | **Default:** mean of the four Table A % values (see Table B); **state formula**. |

### Auditability (Step 4)

- **PRD mapping table:** context items vs PRD ref, note ERD A / ERD B alignment (can reference internal alignment %).
- **Contradiction table:** claim in A, claim in B, brief note.

### Confidence

**Per row** in Table A and Table B: **Confidence**, **How obtained**, and **Why** are **required**—not a single global paragraph unless you also repeat overall confidence in §6 Interpretation.

---

**Output:** Markdown for Step 4; follow `workflow_operations.md` for **Step 4 closing** and `xlsx_report_export.md` if Excel includes Step 4.
