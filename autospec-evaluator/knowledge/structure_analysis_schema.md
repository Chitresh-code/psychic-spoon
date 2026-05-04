# Step 2: Generated vs team ERD — content-first comparison

Use this schema when the user provides an **existing ERD** (**DOCX** or **Markdown**) from the engineering team.

**Reader-facing name:** **Generated vs team ERD** (Excel worksheet tab **`generated_vs_team_erd`** — same tab as reduced scope; see `xlsx_report_export.md` for the **unified** reduced-format Excel layout; only the **Scope** narrative differs by mode). Use this schema for **full-scope** Step 2 only—after the scope gate in `workflow_operations.md` when the mode is **full scope** (or auto-selected full-program). For **reduced scope**, use `scope_aligned_comparison_schema.md` instead.

**System Context** (seventh section): See **`system_context_schema.md`** for Step 1–2 and Step 3 (two ERDs) rules and no-external-verification rules.

- **Reference standard:** The **engineering ERD is the reference.** Your job is to assess **how well the generated ERD** matches it. Do not judge or assess the engineering ERD. Do not say the generated ERD is "better" or "more complete"; frame all findings as gaps or changes needed **in the generated ERD** to align with the engineering template.
- **Content-first comparison:** Judge alignment primarily on **content**—how the engineering ERD analyzes and articulates requirements in each section, how the generated ERD compares, and what the generated ERD could adopt. **Do not** compute or report a **structural alignment %**; note layout, column, or ordering gaps **briefly in narrative** (and in recommendations) when they materially affect usability. Do not re-evaluate PRD coverage.
- **System Context:** When present, evaluate per **`system_context_schema.md`** (seventh canonical section). **No** live GitHub or Confluence verification.

### Fidelity — no hallucination or padding (required)

- **Source of truth:** Only the **uploaded** engineering ERD and **uploaded** generated ERD for this step. **Do not** infer missing pages, unseen attachments, or “what the team surely meant.”
- **Missing or empty sections:** If either document **lacks** a comparable section (or it is effectively **blank** / **TBD only**), state that in plain language, apply **N/A** to the section score when the schema says to, and **do not** fabricate alignment bullets, gaps, or numeric bases. **Do not** invent checks or scores to reach a tidy `x/y`.
- **Bullet lists (“Same in both” / “Engineering includes; generated misses”):** Each bullet must rest on **concrete wording or structure you actually saw** in the documents. If there is **nothing substantive** to list under a heading, use **one** honest bullet (e.g. **No substantive overlap to report from the source text in this section**) or **Nothing identified in the engineering ERD for this subsection**—**not** placeholder examples, generic best practices, or made-up row titles.
- **Recommendations (§9):** Only recommend changes that follow from **documented** gaps or misalignment. If the only issue is “section absent,” recommend **adding or labeling** that section—not fictional content to fill it.

## Canonical ERD sections (in order)

1. Document Metadata  
2. Monthly Delivery Summary  
3. Functional Engineering Deliverables  
4. Non-Functional Engineering Deliverables  
5. Engineering Epics  
6. E2E Testing Plan  
7. **System Context** (Confluence/GitHub summaries **as written** in the document)

(Change Log is optional for structure comparison.)

## Report section order (strict)

1. Generated vs team ERD — summary (includes **Quantitative Metrics**)
2. Document Metadata (Content analysis; optional short structure notes)
3. Monthly Delivery Summary (Content analysis; optional short structure notes)
4. Functional Engineering Deliverables (Content analysis; optional short structure notes)
5. Non-Functional Engineering Deliverables (Content analysis; optional short structure notes)
6. Engineering Epics (Content analysis; optional short structure notes)
7. E2E Testing Plan (Content analysis; optional short structure notes)
8. System Context (Content analysis; optional short structure notes)
9. Alignment Recommendations  

---

## Metric definitions (Step 2)

Do **not** report only coarse **0 / 50 / 100** labels. Headline percentages must come from **counted bases** using the internal comparison-check rubric (sum / count). You may add a **readability band** (Match / Partial / Does not match) **derived from** the section **content %** using the thresholds below.

**N/A:** If a section is **absent in the engineering ERD** (e.g. no Epics table), do **not** reward the generated ERD for having more. For that section, set **content** to **N/A** and exclude from averages (re-normalize over sections that apply).

### Per-section content % (concrete basis)

For each section, compute one **alignment score** (same math as before; internal rubric only):

- **Scale:** percentage (`0–100%`) or `N/A` when section does not apply
- **Scoring:** use **4–6 internal comparison checks** per section (default `5`); each check scores `1` (strong match), `0.5` (partial), or `0` (clear mismatch). **Do not** publish the check names or per-check scores in the Markdown report—only the rollup **%** and **basis** (e.g. `3.0/5`).

The alignment score is what appears in the §1 per-section summary table (**Content %** column).

**Comparison check ideas** (pick **4–6** per section **internally**; do **not** list individual check names or scores in the Markdown report):

| Section | Internal check ideas |
|---------|-----------------|
| Document Metadata | naming consistency; date/version format; program/project fields; TBD vs filled; squad/owner style |
| Monthly Delivery Summary | scope clarity; project boundaries; assumptions; dependencies; time horizon |
| Functional Engineering Deliverables | row granularity vs ref; Jira/ticket style; STATUS/acceptance phrasing; trace wording; deliverable naming; owner/team attribution; capability label accuracy vs PRD |
| Non-Functional Engineering Deliverables | category style; measurability; link to epics; priority/severity; specificity; section classification (no misplaced functional items) |
| Engineering Epics | epic sizing; milestone `M#` style; dependency phrasing; PRD link style; notes column |
| E2E Testing Plan | scenario naming; step granularity; expected results; coverage linkage; table vs link pattern |
| System Context | subsection alignment (Confluence vs GitHub blocks); table shapes vs ref; theme coverage vs ref; repo/stack **names as written**; ownership narrative style; freshness/labeling style |

- **Content % (section)** = (sum of internal check scores / number of checks) × 100.
- Basis example: `3.0/5 = 60%` (show only the fraction in tables and section headers, not each check).
- In the per-section summary table, list the **basis** briefly (e.g. `2.5/5`).

### Rollups (headline metrics)

| Metric | Meaning | How to compute |
|--------|---------|----------------|
| **Content alignment** | Style/convention vs engineering ERD. | **Average** of the per-section **content %** values (**six or seven** sections—exclude N/A). **This is the only headline %** for Step 2; do not report structural or combined rollups. |

### Readability band (optional, derived from %)

| Band | Section % range |
|------|-----------------|
| Match | ≥ 80% |
| Partial | 40–79% |
| Does not match | < 40% |

Use bands only **after** showing the **numeric %** and **basis**; bands are not a substitute for counts.

### Confidence (Step 2)

- **Headline Content alignment:** add **Confidence** (Low / Medium / High) and **one reason** (e.g. messy tables, Markdown vs Word table mismatch, wrong input format—PDF instead of DOCX/Markdown, minimal reference ERD).
- **Overall confidence** in Step 2 scores (required): one sentence + Low/Medium/High.

### Auditability: section analysis (required) and structure notes (optional)

Rollup numbers alone (e.g. `2.5/5`) are **not sufficient** for readers—each §2–§8 needs the **score + two comparison lists** below.

**Where:** In **each** of report sections **§2–§8** (including **System Context**), unless that section is **N/A** (then one sentence: why no score).

**1) Alignment score (required)**

One short block (plain sentences or a single line—**no** table for this block):

- **Alignment score:** …% (basis: **x/y**, the sum of internal comparison checks over **y** checks—same internal rubric as **Comparison check ideas** in **Metric definitions** above). If you **cannot** score honestly (e.g. no reference text), use **N/A** for the section per schema rules—**do not** assign a fake percentage.
- **Confidence:** Low / Med / High (optional one phrase).

**Do not** output headings or labels **Metric used**, **Why this metric**, **Scope-aligned content score**, or **Parameters considered**. Do **not** enumerate each internal check name with its 0 / 0.5 / 1 score.

**2) Section comparison — two subsections (required, bullet lists only)**

Use **Markdown bullets only** (no tables). Keep each list tight and concrete.

1. **Same in both ERDs** — what aligns in substance, naming, or intent for this section.
2. **Engineering ERD includes; generated ERD misses or underplays** — gaps the **generated** ERD should close to match the reference.

If a list truly has **no** document-grounded items, a **single** bullet such as **None identified from the source documents** (or **Engineering ERD has no comparable content here**) is required—**not** invented items to look thorough.

Treat **alignment score** + these **two lists** as the three reader-facing parts of each section (score + subsection A + subsection B)—**no** separate parameter tables.

**3) Structure / layout (optional)** — no separate **structure %**. If missing columns, wrong section order, or table shape mismatches **materially** affect the generated ERD, add **up to 3 short bullets** citing concrete differences. Skip if nothing material.

**4) Short narrative (optional)** — one short paragraph if it helps tie the bullets to **Suggestions for generated ERD** themes from each section’s guidance below.

**§1 Quantitative Metrics** keeps the per-section summary table; **§2–§8** must show **alignment score + two bullet subsections** (and optional structure bullets / narrative).

---

## 1. Generated vs team ERD — summary

- **Documents:** Generated ERD vs Engineering ERD (filenames or "uploaded").
- **Reference:** Engineering ERD is the reference; we assess the generated ERD against it.
- **Overall content match:** Aligned / Partial / Not aligned (from the perspective of the generated ERD matching the engineering ERD’s **content style and conventions**).
- **Match terms (content):** **Match** = generated ERD is close to engineering ERD for that section’s **articulation** of requirements. **Partial** = useful overlap but clear gaps in granularity, phrasing, or conventions. **Does not match** = generated ERD should change substantially to align with how engineering expresses that section.
- **Content analysis summary (2–3 lines):** How does the engineering ERD tend to analyze/articulate requirements overall? (e.g. granularity of deliverables, use of Jira IDs, narrative vs table style, epic scoping, E2E scenario style.) One or two high-level suggestions for what the generated ERD could adopt.
- **One-line summary:** e.g. "Generated ERD is close on metadata tone; adopt engineering ERD’s deliverable granularity and STATUS phrasing in Functional Deliverables."

### Quantitative Metrics (required)

**Summary table (headline):**

| Metric | Result | How obtained | Confidence | Why |
|--------|--------|--------------|------------|-----|
| Content alignment | …% | Mean of section content % (6 or 7, exclude N/A) | Low / Med / High | … |

**Overall confidence** (required): one line **Low / Medium / High** + rationale.

**Per-section scores (required):**

| Section | Content % | Content basis (x/y checks) | Band (optional) |
|---------|-----------|-----------------------------------|-----------------|
| Document Metadata | …% | e.g. 2.5/5 | Match / Partial / Does not match |
| Monthly Delivery Summary | …% | … | … |
| Functional Engineering Deliverables | …% | … | … |
| Non-Functional Engineering Deliverables | …% | … | … |
| Engineering Epics | …% | … | … |
| E2E Testing Plan | …% | … | … |
| System Context | …% | … | … |

Add **2–3 bullets**: interpretation (largest gaps by **numeric** gap, not only band), and any **per-section** confidence callouts if one section was hard to parse.

## 2. Document Metadata

**Alignment score** + **two bullet subsections** (required) → optional **structure/layout bullets** → optional short narrative.

**Structure**
- **Reference (engineering ERD):** Describe its metadata block (e.g. PROGRAM, DOMAIN, PROJECTS, SQUAD, TRACK, STEL, Engineering Manager, Technical Lead, Architect).
- **Generated ERD assessment:** Does the generated ERD have the same metadata fields and naming as the engineering ERD? List what the **generated ERD** is missing or has extra; recommend changes to the generated ERD only.
- **Table format:** N/A (metadata is usually not a table).

**Content analysis**
- **Engineering ERD:** How does it use metadata (e.g. real values vs TBD, project vs program naming, version/date style)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt from the engineering ERD’s metadata style or conventions?

## 3. Monthly Delivery Summary

**Alignment score** + **two bullet subsections** (required) → optional **structure/layout bullets** → optional short narrative.

**Structure**
- **Reference (engineering ERD):** How is this section structured (e.g. single paragraph vs project-by-project blocks)?
- **Generated ERD assessment:** Does the generated ERD match placement and format? What should the generated ERD change to align?

**Content analysis**
- **Engineering ERD:** How does it articulate delivery scope (e.g. by project/release, business value, assumptions, dependencies)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. project-by-project breakdown, explicit assumptions)?

## 4. Functional Engineering Deliverables

**Alignment score** + **two bullet subsections** (required) → optional **structure/layout bullets** → optional short narrative.

**Structure**
- **Reference (engineering ERD):** Table columns and order (e.g. CAPABILITY | FUNCTIONAL ENGINEERING DELIVERABLES | STATUS | DESCRIPTION).
- **Generated ERD assessment:** Does the generated ERD have the same columns and order? List changes the **generated ERD** should make to match (e.g. add STATUS column, reorder columns).
- **Owner/Team column check:** If the engineering ERD includes an Owner or Team column (or attributes deliverables to specific teams in descriptions), check whether the generated ERD does the same. If the engineering ERD has owner attribution and the generated ERD does not, flag this under **optional structure notes** or **Alignment Recommendations**.

**Content analysis**
- **Engineering ERD:** How does it analyze/articulate deliverables (e.g. granularity, phrasing, use of Jira/ticket IDs, traceability to PRD/FR, one row per deliverable vs grouped)?
- **Generated ERD:** How does it compare?
- **Capability label accuracy:** Check whether the generated ERD's Capability column values match the PRD-defined capability names. List any mismatches (paraphrased, merged, or invented labels) and note what the correct PRD capability name should be.
- **Section classification:** Check whether any items in the generated ERD's Functional Deliverables table are **non-functional by nature** (internal monitoring, alerting, reporting, observability, dashboards, audit compliance). If so, flag them as misclassified and recommend moving them to Non-Functional Deliverables.
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. same level of granularity, deliverable-naming style, STATUS or acceptance phrasing, owner attribution)?

## 5. Non-Functional Engineering Deliverables

**Alignment score** + **two bullet subsections** (required) → optional **structure/layout bullets** → optional short narrative.

**Structure**
- **Reference (engineering ERD):** Table columns and order.
- **Generated ERD assessment:** Does the generated ERD match? List changes the **generated ERD** should make to match.

**Content analysis**
- **Engineering ERD:** How does it articulate NFRs (e.g. categories, concrete targets vs TBD, linkage to tickets or epics)?
- **Generated ERD:** How does it compare?
- **Section classification (reverse check):** Check whether any items in the generated ERD's Non-Functional Deliverables are actually **functional by nature** (new API endpoints, workflow logic, user-facing features). If so, flag them as misclassified and recommend moving to Functional Deliverables. Also check: are there items in the **Functional** section (§4) that should be here instead (monitoring, alerting, reporting, observability, dashboards, audit compliance)? Cross-reference with §4 findings.
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. NFR categorization, level of specificity)?

## 6. Engineering Epics

If **N/A** (no comparable reference section): state why; **no alignment score**. Otherwise: **Alignment score** + **two bullet subsections** (required) → optional structure bullets → optional narrative.

**Structure**
- **Reference (engineering ERD):** Does it have a dedicated Epics section/table? If yes, column names and order. If no, state that the engineering ERD does not use a separate Epics table.
- **Generated ERD assessment:** Should the generated ERD match the engineering ERD (e.g. add/remove Epics section, match columns)? List changes for the **generated ERD** only. Do not suggest the engineering ERD add an Epics table because the generated one has it. If the engineering ERD has no Epics section, recommend removing or de-emphasizing it in the generated ERD; never say the generated ERD is "more structured" or "better" in this area.

**Content analysis**
- **Engineering ERD:** How does it articulate epics (e.g. scoping, dependency phrasing, PRD/FR linkage, milestone usage, NOTES style)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. epic description style, dependency or milestone conventions)?

## 7. E2E Testing Plan

**Alignment score** + **two bullet subsections** (required) → optional **structure/layout bullets** → optional short narrative.

**Structure**
- **Reference (engineering ERD):** How is this section presented (e.g. full table vs link to wiki)?
- **Generated ERD assessment:** Should the generated ERD match (e.g. replace in-document table with a link, or add columns)? List changes for the **generated ERD** only.

**Content analysis**
- **Engineering ERD:** How does it articulate test scenarios (e.g. scenario naming, step granularity, expected results, link to deliverables or coverage)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. scenario style, level of detail, column usage)?

## 8. System Context

If **N/A** (no comparable System Context in the engineering ERD and no generated block to compare): state why; **no alignment score**. Otherwise: **Alignment score** + **two bullet subsections** (required) → optional structure bullets → optional narrative. Full rules: **`system_context_schema.md`**.

**Structure**
- **Reference (engineering ERD):** Confluence/GitHub summary subsections, table shapes, column roles.
- **Generated ERD assessment:** What should the generated ERD change to align (layout, missing columns, subsection labels)?

**Content analysis**
- **Engineering ERD:** How themes, repo/stack labels, and findings are expressed (**as written**—no external verification).
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** Alignment with engineering style and naming only; do not judge engineering ERD.

## 9. Alignment Recommendations

- **One direction only:** Prioritized list of changes **to the generated ERD** so it matches the **engineering ERD**. Include both:
  - **Structure:** e.g. "Add STATUS column to Functional Deliverables", "Add Owner/Team column to match engineering ERD", "Use same title/version header format as engineering ERD".
  - **Content/style:** e.g. "Adopt engineering ERD’s granularity in Functional Deliverables", "Match engineering ERD’s project-by-project breakdown in Monthly Delivery Summary".
  - **Classification:** e.g. "Move ENG-013 (monitoring) and ENG-014 (dashboards) from Functional to Non-Functional Deliverables", "Fix capability label on ENG-006 to match PRD capability name".
- Do not recommend that the engineering ERD adopt anything from the generated ERD. Do not suggest new PRD content; content recommendations are about how the generated ERD expresses and analyzes requirements (style, conventions, granularity).

### Standing recommendations (always include when applicable)

These recommendations address common ERD quality gaps that engineering teams consistently flag. Include them when the condition is met, even if the engineering ERD does not explicitly model them:

- **Owner attribution:** If the generated ERD’s Functional Deliverables table has no Owner or Team column, recommend adding one. Engineering teams need to know who is responsible for each deliverable at a glance.
- **Repository coverage:** If any ENG-XXX describes frontend, UI, banner, dashboard, or client-side work but the ERD (System Context, Dependencies, or deliverable descriptions) declares no frontend/UI repository, flag the mismatch. The evaluator cannot verify which repositories exist, but the absence of a frontend repo alongside UI-scoped deliverables is worth surfacing.

---

## Relationship to reduced scope (same delivery scope)

This schema compares **entire documents** (strict). If the **engineering ERD only covers a subset of the PRD** while the **generated ERD reflects broader PRD scope**, whole-document scores can be **misleadingly low**. **Before** running this analysis, follow `workflow_operations.md`: infer coverage vs PRD; if partial, give a **short** coverage list and let the user choose **full scope** (this schema) or **reduced scope** (`scope_aligned_comparison_schema.md`—same **content-first** model and **alignment score** math, but **scoped slice + lenient constraints**).

---

**Output:** Markdown only. Use tables where helpful for column/section comparison.
