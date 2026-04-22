# System Context — Steps 1–3 and Step 4

Use this schema whenever the ERD contains a **System Context** block: narrative summaries labeled as being informed by **Confluence** and/or **GitHub**, typically including tables for wiki pages (titles, space, dates, freshness, key findings) and for repositories (branch, files read, findings, tech stack, ownership).

## Hard constraint (no external verification)

The evaluator has **no access to GitHub or Confluence**. Do **not** score “sourcing quality” against live repos or wiki pages. Treat all Confluence/GitHub content as **text inside the uploaded document** (DOCX or Markdown). Verification is **PRD ↔ ERD** and **ERD ↔ ERD** only.

---

## Steps 1–3 — System Context slice

### Where it appears in reports

- **Step 1:** Integrate System Context into **Quantitative Metrics** (see `evaluation_report_schema.md`) and, where useful, **Completeness**, **Hallucination**, **Traceability**, and **Recommendations** when gaps concern context.

### Hallucination triage vs System Context (Step 1)

When flagging **unsourced** or **hallucinated** content in **Functional / NFR / Epics / E2E** (or similar) **outside** the System Context block:

1. **First** check the **PRD** for a defensible basis.
2. If missing in the PRD, **second** check whether the **same or clearly equivalent substance** appears in the generated ERD’s **System Context** section (Confluence/GitHub summaries **as written in the document**—no live verification).
3. **If** it appears in System Context: put it in **`evaluation_report_schema.md` §4b** (*Traced to System Context, not in PRD*) in the **Markdown** report, with a short pointer to where in System Context it appears, and the required disclaimer that the evaluator **cannot verify** external correctness. **Do not** count it in the **Hallucination rate** numerator; **do not** list it in §4a. **Do not** add §4b items to the Excel **`PRD_Analysis`** sheet or to **`submitEvaluationReport`** (those channels list **true hallucinations / §4a only**).
4. **If** it appears in **neither** PRD nor System Context: treat as a **hallucination** in §4a and count it in the **Hallucination rate** numerator (subject to the usual “no IDs / boilerplate” rules in `evaluation_report_schema.md`).

This does **not** remove the need for **Unsupported / contradictory context claims** and other System Context quality metrics for the **content of the System Context block itself**.
- **Step 2:** Treat **System Context** as the **seventh canonical section** (after E2E Testing Plan). Include **Structure check ledger** and **Content facet ledger** for System Context per `structure_analysis_schema.md` (§8).
- **Step 3:** Apply the same **scoped** rules as other sections: evaluate System Context rows/themes only inside the **declared engineering slice** (see `scope_aligned_comparison_schema.md`).

### Metric definitions (Step 1 — System Context)

All percentages **0–100**. State **numerator / denominator** (or **passed / applicable**). If denominator is **0**, report **N/A** with reason.

| Metric | Meaning | How to compute |
|--------|---------|----------------|
| **System Context structural completeness** | The System Context block matches the **expected shape** (subsections + tables/fields). | **Checks** (adapt to the doc): Confluence summary subsection present; GitHub summary subsection present; Confluence rows have roles for title, space, last modified, freshness, key findings; GitHub area has repo, branch, files read (or equivalent), findings; optional tech stack and ownership tables. **%** = (passed / applicable) × 100. |
| **Context–PRD alignment** | How well **key findings and themes** in System Context **reflect** PRD requirements (capabilities, scope, dependencies, constraints, milestones). | **Denominator:** discrete context items scored (rows or themed paragraphs—state counting rule). **Numerator:** items that **map** to at least one PRD requirement/theme (cite PRD section or ID). **%** = (numerator / denominator) × 100. |
| **Context relevance** | Whether context is **on-topic** for this program vs generic or tangential filler. | Same denominator (or documented sample). **Numerator:** items **materially relevant** to the PRD problem domain and delivery narrative. **Relevance %** = (numerator / denominator) × 100. |
| **Unsupported / contradictory context claims** | Claims that **contradict the PRD** or add **major substance** with **no PRD basis** (no external verification required). | **Denominator:** substantive claims in System Context. **Numerator:** contradictions or unsupported major assertions. **Rate %** = (numerator / denominator) × 100. |

**Confidence:** Low / Med / High per metric and for System Context overall, with one reason.

**Counting basis:** Document what counted as one “context item” and one “substantive claim.”

### Step 2 — System Context (seventh section)

- **Reference:** Engineering ERD only. Assess **generated** against **engineering** for this block.
- **Structure %:** Countable checks vs engineering (presence of matching subsections, table shapes, column roles).
- **Content %:** **Five facets** (document which five), each **0 / 0.5 / 1**, e.g.: subsections aligned; comparable theme coverage; compatible repo/stack **names as written**; compatible ownership narrative; table/style alignment. **No** live repo/wiki verification.
- **Ledgers:** Same **Structure check ledger** and **Content facet ledger** formats as other sections.

### Step 3 — System Context (scoped)

- Same metric model as Step 2; **checks and facets** only on **in-scope** System Context rows/themes per declared slice.
- **N/A** if engineering ERD has no System Context for the slice; re-normalize section averages.

### Rollups

- **Step 1:** Report System Context metrics **alongside** core Step 1 metrics (see `evaluation_report_schema.md`).
- **Step 2 / Step 3:** **Structural alignment** and **Content alignment** rollups are the **mean** of applicable section **structure %** and **content %**, including **System Context** when not N/A (typically **seven** sections).

---

## Step 4 — System Context only (two ERDs)

Run **only** when the user accepts after **Step 3** (or explicitly asks for Step 4 after Step 3). Requires **PRD** from Step 1 and **two ERDs’** System Context sections:

- **Option A:** Generated ERD vs **engineering ERD** (from Step 2), or  
- **Option B:** User uploads **another ERD** (DOCX or Markdown); compare that ERD’s System Context to the **generated** ERD (or clarify pair if user specifies).

**No external verification.** Use PRD + in-document text only.

### Report section order (strict)

1. Step 4 Summary (documents compared, option A or B, PRD used)
2. **Quantitative Metrics** — use **two tables** (format below); do **not** hide per-ERD scores inside parity-only wording
3. PRD mapping / alignment ledger (optional but recommended)
4. Contradiction ledger
5. Optional mention-overlap ledger (labeled **document-only**)
6. Interpretation (narrative); **do not** duplicate the metric tables here—reference §2

### Quantitative Metrics — required layout (Step 4)

Show **scores for each ERD explicitly**, then **pair-level** metrics. **Every row** must include **Confidence** (Low / Med / High), **How obtained** (counts, formula, or facet sum), and **Why** (1–2 short phrases: what drives the number, what limits certainty).

#### Table A — Per-ERD metrics (required)

Same definitions as Step 1 System Context (`system_context_schema.md` Steps 1–3), computed **separately** for **ERD A** and **ERD B** using the **same PRD**.

| Metric | ERD A (%) | ERD B (%) | Confidence | How obtained | Why |
|--------|-----------|-----------|------------|--------------|-----|
| **Context–PRD alignment** | … | … | Low / Med / High | Numerator/denominator per ERD; cite counting rule | e.g. thin rows on B, placeholder text |
| **Context relevance** | … | … | Low / Med / High | Numerator/denominator per ERD | e.g. one doc mostly placeholders |
| **System Context structural completeness** (optional but recommended) | … | … | Low / Med / High | Passed/applicable checks per ERD | … |
| **Unsupported / contradictory context claims rate** (optional) | … | … | Low / Med / High | Numerator/denominator per ERD | … |

Use **N/A** in an ERD column only if that ERD truly has no scorable System Context block (state why).

**Derived (optional row):** **PRD alignment gap** = \|ERD A − ERD B\| percentage points on **Context–PRD alignment** (not a substitute for showing both scores).

#### Table B — Cross-ERD / pair metrics (required)

These apply to the **pair**, not to one ERD alone. Use a **single Result** column (%, label, or short note).

| Metric | Result | Confidence | How obtained | Why |
|--------|--------|------------|--------------|-----|
| **Cross-ERD thematic consistency** | e.g. 20% | Low / Med / High | Five facets 0/0.5/1; sum/5 × 100 | e.g. one populated, one placeholder |
| **Inter-ERD consistency score** | e.g. 80% | Low / Med / High | Denominator = comparable claim pairs; numerator = contradictions/tensions; score = 100% − rate | e.g. mostly omission vs direct conflict |
| **Mention overlap (optional)** | e.g. 0% or N/A | Low / Med / High | Jaccard on titles/repos **in the document** | label **document-only** |
| **Relevance consistency (qualitative)** | e.g. Mixed — large mismatch | Low / Med / High | Compare Table A relevance % A vs B | short justification |
| **Overall Step 4 score** | e.g. 38.3% | Low / Med / High | State **exact formula** (e.g. mean of pair metrics, or weighted blend) | which inputs dominated |

**Do not** replace Table A with parity-only metrics (e.g. “15% parity”) **without** also showing **ERD A** and **ERD B** alignment % in Table A. Parity or min/max may appear in Table B as a **supplement**, not the only headline for PRD alignment.

### Metric definitions (Step 4 — reference)

| Metric | Meaning | How to compute |
|--------|---------|----------------|
| **Context–PRD alignment** (per ERD) | Same as Step 1. | Table A: two independent percentages. |
| **Context relevance** (per ERD) | Same as Step 1. | Table A: two independent percentages. |
| **Cross-ERD thematic consistency** | Coherent story where both speak. | **Five facets** (0 / 0.5 / 1): e.g. no direct conflict on program scope; compatible dependency/owner narrative; compatible migration/governance emphasis; comparable risk themes; compatible stack/repo **as stated**. **Thematic consistency %** = (sum / 5) × 100. |
| **Mention overlap (optional)** | Label similarity in the uploaded document. | **Jaccard** on normalized page titles and `org/repo` strings. Supporting only. |
| **Inter-ERD consistency score** | Penalize contradictions where both make comparable claims. | **Denominator:** comparable claim pairs. **Numerator:** contradictions or strong tensions. **Score** = 100% − (numerator/denominator × 100). |
| **Overall Step 4 score** | Headline rollup. | Combine pair metrics (Table B) as documented; optionally fold in gap from Table A; **state formula**. |

### Auditability (Step 4)

- **PRD mapping table:** context items vs PRD ref, note ERD A / ERD B alignment.
- **Contradiction table:** claim in A, claim in B, brief note.
- **Mention overlap list (optional):** in A / in B / both — label **document-only, not verified externally**.

### Confidence

**Per row** in Table A and Table B: **Confidence**, **How obtained**, and **Why** are **required**—not a single global paragraph unless you also repeat overall confidence in §6 Interpretation.

---

**Output:** Markdown for Step 4; follow `workflow_operations.md` for **Step 4 closing** and `xlsx_report_export.md` if Excel includes Step 4.
