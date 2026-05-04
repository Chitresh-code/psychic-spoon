# System Context — Steps 1–2 (full or reduced scope) and Step 3 (two ERDs)

Use this schema whenever the ERD contains a **System Context** block: narrative summaries labeled as being informed by **Confluence** and/or **GitHub**, typically including tables for wiki pages (titles, space, dates, freshness, key findings) and for repositories (branch, files read, findings, tech stack, ownership).

## Hard constraint (no external verification)

The evaluator has **no access to GitHub or Confluence**. Do **not** score “sourcing quality” against live repos or wiki pages. Treat all Confluence/GitHub content as **text inside the uploaded document** (DOCX or Markdown). **Step 1** uses **PRD ↔ ERD** (and System Context only for triage per `evaluation_report_schema.md`). **Step 2** uses **generated ↔ team ERD**. **Step 3** uses **ERD A ↔ ERD B** System Context text **only**—**do not** use the PRD for Step 3 metrics, tables, or ledgers.

---

## Steps 1–2 — System Context slice

### Where it appears in reports

- **Step 1:** **Do not** put System Context in the §2 numeric summary table. **Do not** add a **System Context (when present)** subsection or any standalone System Context narrative or dimension **%** block (see `evaluation_report_schema.md`). Use System Context **in-document text** only for **§4a / §4b** triage, **Hallucination** counting rules, and **short inline** mentions in **Completeness**, **Traceability**, or **Recommendations** when a specific gap references context.

### Hallucination triage vs System Context (Step 1)

When flagging **unsourced** or **hallucinated** content in **Functional / NFR / Epics / E2E** (or similar) **outside** the System Context block:

1. **First** check the **PRD** for a defensible basis.
2. If missing in the PRD, **second** check whether the **same or clearly equivalent substance** appears in the generated ERD’s **System Context** section (Confluence/GitHub summaries **as written in the document**—no live verification).
3. **If** it appears in System Context: put it in **`evaluation_report_schema.md` §4b** (*Traced to System Context, not in PRD*) in the **Markdown** report, with a short pointer to where in System Context it appears, and the required disclaimer that the evaluator **cannot verify** external correctness. **Do not** count it in the **Hallucination rate** numerator; **do not** list it in §4a. **Do not** add §4b items to the Excel **`PRD_vs_generated`** sheet (that sheet lists **true hallucinations / §4a only**).
4. **If** it appears in **neither** PRD nor System Context: treat as a **hallucination** in §4a and count it in the **Hallucination rate** numerator (subject to the usual “no IDs / boilerplate” rules in `evaluation_report_schema.md`).

Unsupported or contradictory context claims may still inform **§4b** and **Recommendations**; Step 1 does **not** include a separate System Context assessment section or merged % headline in §2.
- **Step 2 (full scope):** Treat **System Context** as the **seventh canonical section** (after E2E Testing Plan). Follow **content-first** rules per `structure_analysis_schema.md` (§8): **alignment score** + **two bullet subsections** required; structure notes optional.
- **Step 2 (reduced scope):** Apply the same **scoped** rules as other sections: evaluate System Context rows/themes only inside the **declared engineering slice** (see `scope_aligned_comparison_schema.md`).

### Assessment dimensions (Step 1 — internal triage only)

These dimensions are **not** reported as a Step 1 subsection or prose **%** block. Use them **only** as private judgment to support **§4b** rows, **Recommendations**, or **inline** completeness notes when a **specific** context claim matters—without publishing a System Context audit section.

| Dimension | Meaning (internal) |
|--------|---------|
| **Structural completeness** | Whether the block has expected subsections/tables (Confluence/GitHub style). |
| **Context–PRD alignment** | Whether themes map to PRD requirements. |
| **Context relevance** | Whether items are on-topic vs filler. |
| **Unsupported / contradictory context claims** | Contradictions or major substance with no PRD basis. |

**Do not** output Step 1 percentages or a confidence paragraph for System Context as a whole.

### Step 2 (full scope) — System Context (seventh section)

- **Fidelity:** Compare **only** text that appears **inside** the two uploaded ERDs. **Do not** invent wiki/repo facts, URLs, or findings not written in the documents; if a block is empty or missing, say so (see **`structure_analysis_schema.md`** — *Fidelity — no hallucination or padding*).
- **Reference:** Engineering ERD only. Assess **generated** against **engineering** for this block.
- **Content-first (same as Step 2 overall):** Judge alignment using **content %** and the **alignment score + two bullet subsections** for this section. You may note layout/column gaps briefly in narrative if they block usability; **do not** compute a separate **structure %** rollup for System Context or include structure in the Step 2 headline **Content alignment**.
- **Content %:** Use the same internal comparison-check rubric as other Step 2 sections (**0 / 0.5 / 1** per check, typically **4–6** checks). **Do not** list individual check names or scores in the Markdown report—only **alignment score** % and **basis** (x/y). **No** live repo/wiki verification.
- **Evidence format:** **Alignment score** + **two bullet subsections** per `structure_analysis_schema.md` (**Same in both ERDs**; **Engineering includes; generated misses or underplays**). **Do not** output **Metric used**, **Why this metric**, **Parameters considered**, or **Scope-aligned content score**. Structure notes remain optional (short bullets if material).

### Step 2 — System Context (reduced scope / scoped)

- Same **content-first** model as full-scope Step 2; apply the internal check rubric only on **in-scope** System Context rows/themes per declared slice.
- **N/A** if engineering ERD has no System Context for the slice; re-normalize section averages.

### Rollups

- **Step 1:** System Context has **no** numeric rollup in §2 and **no** dedicated narrative section (`evaluation_report_schema.md`).
- **Step 2 (either mode):** **Content alignment** (or **Content alignment (scoped)**) rollup is the **mean** of applicable section **content %** values only (**six or seven** sections, including **System Context** when not N/A). **Do not** compute or report a separate **structural alignment** %.

---

## Step 3 — Research context (two ERDs)

**Reader-facing name:** **Research context (two ERDs)** (Excel tab `Context_compare`). Internal “Step 3” replaces the former **Step 4** in operator docs.

**When to run:** After **Step 2** completes, or when the user explicitly asks for this comparison and both ERDs are available. **Do not** require or use the **PRD** in this step—no **Context–PRD alignment**, no **Context relevance vs PRD**, no **PRD mapping** ledger. Compare **only** the **System Context** text that appears inside **ERD A** and **ERD B**.

**Pairing:** **(1)** Generated ERD vs **engineering ERD** from Step 2, and/or **(2)** vs **another** DOCX/Markdown ERD the user uploads. Label **ERD A** and **ERD B** clearly. If ambiguous, ask which two documents to compare.

### Quantitative Metrics — required layout (Step 3)

Use **one pair-level table** only (**no** per-ERD Table A). **Every row** must include **Confidence** (Low / Med / High), **How obtained** (counts, formula, or rule), and **Why** (1–2 short phrases). **Do not** cite the PRD in any formula, denominator, or rationale for Step 3.

#### Pair metrics table (required)

Use **exactly two scored rows** in this table—**do not** add **Overall Step 3 score** or any other headline % that duplicates **Content similarity** (that duplicate confuses readers in chat and Excel).

| Metric | Result | Confidence | How obtained | Why |
|--------|--------|------------|--------------|-----|
| **Content similarity (System Context)** | e.g. 38% or N/A | Low / Med / High | **Default:** Jaccard on normalized lines or comparable atomic items: \(\|A \cap B\| / \|A \cup B\|\) × 100; state normalization (trim, lowercase, table cell text). Alternative: theme-level overlap if line match is misleading—**state method**. If both blocks empty or unscorable, **N/A** with reason. | … |
| **Inter-ERD consistency (qualitative)** | e.g. Mixed — contradictions on repo count | Low / Med / High | Compare **claims and themes** across A and B **from the two documents only** (agree / diverge / contradict); cite example pairs | … |

**Do not** add **Structural completeness (System Context)** or **Substantive item rate (in-document)** as Step 3 metrics. **Do not** add **Context–PRD alignment**, **Context relevance (vs PRD)**, or **PRD alignment gap**.

**Do not** report retired low-signal pair metrics (**Cross-ERD thematic consistency**, **Inter-ERD consistency score**, **Mention overlap**) in chat or Excel. Use the **contradiction ledger** for substantive tensions.

### System Context Comparison (required subsection)

After the **pair metrics table**, add **“System Context Comparison”** with:

- **Content similarity** (repeat % from the table is OK) plus **short** lists: **Shared** (or strongly overlapping) items, **Unique to ERD A**, **Unique to ERD B** (use normalized lines or bullet extractions; keep lists readable, not a full dump).

### Metric definitions (Step 3 — reference)

| Metric | Meaning | How to compute |
|--------|---------|----------------|
| **Content similarity (System Context)** | How much the two blocks **overlap** lexically or thematically; the **only** numeric headline for this step. | Jaccard (default) or stated theme overlap; **ERD text only**. |
| **Inter-ERD consistency (qualitative)** | Whether the two System Context narratives **agree, diverge, or contradict** on substantive themes (no second numeric rollup). | Document-pair review only; use the **contradiction ledger** for tensions. |

### Auditability (Step 3)

- **Cross-ERD theme / claim ledger (recommended):** theme or short claim; how **ERD A** states it; how **ERD B** states it; relationship (**align** / **diverge** / **contradict**). **No PRD column.**
- **Contradiction ledger (required where applicable):** claim in A, claim in B, brief note—**from document text only**.

### Report section order (strict)

1. **Step 3 summary** — ERD A and ERD B labels (filenames), how the pair was chosen; **do not** list “PRD used” for this step.
2. **Quantitative Metrics** — pair metrics table only (**Content similarity**, **Inter-ERD consistency** qualitative)—**no** duplicate “overall” % row.
3. **System Context Comparison** — similarity + shared / unique lists.
4. **Cross-ERD theme / claim ledger** (recommended).
5. **Contradiction ledger**.
6. **Interpretation** — narrative only; **do not** duplicate the metric tables; **do not** argue PRD fit.

### Confidence

**Per row** in the pair metrics table: **Confidence**, **How obtained**, and **Why** are **required**.

---

**Output:** Markdown for Step 3; follow `workflow_operations.md` for **Step 3 closing** and `xlsx_report_export.md` if Excel includes **Research context (two ERDs)**.
