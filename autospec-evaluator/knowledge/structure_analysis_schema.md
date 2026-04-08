# Step 2: Structure and Content Analysis – Generated ERD vs Existing Engineering ERD

Use this schema when the user provides an **existing ERD** (**DOCX** or **Markdown**) from the engineering team.

**System Context** (seventh section): See **`system_context_schema.md`** for Step 1–3 metric alignment and no-external-verification rules.

- **Reference standard:** The **engineering ERD is the reference.** Your job is to assess **how well the generated ERD** matches it. Do not judge or assess the engineering ERD. Do not say the generated ERD is "better" or "more complete"; frame all findings as gaps or changes needed **in the generated ERD** to align with the engineering template.
- **Two parts per section:** (1) **Structure** – sections, order, tables, headers. (2) **Content analysis** – how the engineering ERD analyzes and articulates requirements in that section; how the generated ERD compares; what the generated ERD could adopt. Do not re-evaluate PRD coverage; content analysis is about how requirements are expressed inside the ERD.
- **System Context:** When present, evaluate per **`system_context_schema.md`** (seventh canonical section). **No** live GitHub or Confluence verification.

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

1. Structure and Content Summary (includes **Quantitative Metrics**)
2. Document Metadata (Structure + Content analysis)
3. Monthly Delivery Summary (Structure + Content analysis)
4. Functional Engineering Deliverables (Structure + Content analysis)
5. Non-Functional Engineering Deliverables (Structure + Content analysis)
6. Engineering Epics (Structure + Content analysis)
7. E2E Testing Plan (Structure + Content analysis)
8. System Context (Structure + Content analysis)
9. Alignment Recommendations  

---

## Metric definitions (Step 2)

Do **not** report only coarse **0 / 50 / 100** labels. Headline percentages must come from **counted bases** (structure checks; content facets). You may add a **readability band** (Match / Partial / Does not match) **derived from** the computed % using the thresholds below.

**N/A:** If a section is **absent in the engineering ERD** (e.g. no Epics table), do **not** reward the generated ERD for having more. For that section, set **structure** and **content** to **N/A** and exclude from averages (re-normalize over sections that apply).

### Per-section structure % (concrete basis)

For each of the **seven** sections (when System Context exists in either doc; otherwise six), define **structure checks** against the **engineering ERD** (reference). Checks must be **countable**. Adapt lists to what the documents actually contain.

| # | Check (examples—customize) | Pass = 1, else 0 |
|---|----------------------------|------------------|
| 1 | **Presence:** Comparable section exists in generated (same role in doc flow). | |
| 2 | **Layout class:** Table vs narrative vs hybrid matches reference intent (e.g. both tabular). | |
| 3+ | **Columns / fields:** For tables, each **required** column (or metadata field) from the engineering ERD appears in generated with same **semantic** role (name may differ slightly—justify). Count **one check per required column/field**. | |

- **Structure % (section)** = (passed checks / applicable checks) × 100. State **passed / applicable** (e.g. `8/9`; exclude checks that do not apply and say why).
- If there is **no table** in the reference but both use prose, use a **smaller checklist** (presence + paragraph structure + bullets) still expressed as **x/y**.

### Per-section content % (concrete basis)

For each section, score **exactly five content facets** (if a facet does not apply, replace with another facet for that section and keep **five** scored items). Each facet: **1** = strong match to engineering style, **0.5** = partial, **0** = clear mismatch.

**Example facets (pick 5 per section, document which five in the report):**

| Section | Facet ideas (choose 5) |
|---------|------------------------|
| Document Metadata | naming consistency; date/version format; program/project fields; TBD vs filled; squad/owner style |
| Monthly Delivery Summary | scope clarity; project boundaries; assumptions; dependencies; time horizon |
| Functional Engineering Deliverables | row granularity vs ref; Jira/ticket style; STATUS/acceptance phrasing; trace wording; deliverable naming |
| Non-Functional Engineering Deliverables | category style; measurability; link to epics; priority/severity; specificity |
| Engineering Epics | epic sizing; milestone `M#` style; dependency phrasing; PRD link style; notes column |
| E2E Testing Plan | scenario naming; step granularity; expected results; coverage linkage; table vs link pattern |
| System Context | subsection alignment (Confluence vs GitHub blocks); table shapes vs ref; theme coverage vs ref; repo/stack **names as written**; ownership narrative style; freshness/labeling style |

- **Content % (section)** = (sum of five facet scores / 5) × 100. Basis example: `facets 4.0/5 = 80%`.
- In the per-section table, list **facet points** briefly (e.g. `4.0/5`) or sub-bullets for **0.5** facets.

### Rollups (headline metrics)

| Metric | Meaning | How to compute |
|--------|---------|----------------|
| **Structural alignment** | Layout vs engineering ERD. | **Average** of the per-section **structure %** values (**six or seven** sections—exclude N/A). |
| **Content alignment** | Style/convention vs engineering ERD. | **Average** of the per-section **content %** values (**six or seven**—exclude N/A). |
| **Combined engineering alignment** | Single headline. | **(Structural alignment % + Content alignment %) / 2** |

### Readability band (optional, derived from %)

| Band | Section % range |
|------|-----------------|
| Match | ≥ 80% |
| Partial | 40–79% |
| Does not match | < 40% |

Use bands only **after** showing the **numeric %** and **basis**; bands are not a substitute for counts.

### Confidence (Step 2)

- **Per headline metric:** add **Confidence** (Low / Medium / High) and **one reason** (e.g. messy tables, Markdown vs Word table mismatch, wrong input format—PDF instead of DOCX/Markdown, minimal reference ERD).
- **Structural vs content confidence** (optional if split): e.g. High structural / Medium content when reference is sparse on prose.
- **Overall confidence** in Step 2 scores (required): one sentence + Low/Medium/High.

### Auditability: check and facet ledgers (required)

Rollup numbers alone (e.g. `11/12 checks`, `2.5/5 facets`) are **not sufficient**. Readers must see **what** was checked and **what failed**.

**Where:** In **each** of report sections **§2–§8** (per canonical section, including **System Context**), place **two tables** *before* the narrative bullets—**unless** that section is **N/A** (then one sentence: why no ledger; no scores).

**1) Structure check ledger** — one row per check that counts toward `y` in `x/y`:

| # | Check (what was verified) | Reference (engineering ERD) | Generated ERD | Pass? |
|---|---------------------------|------------------------------|---------------|-------|
| 1 | … | Concrete fact from reference | Concrete fact from generated | Yes / No |

- **Check** names must be **specific** (e.g. “STATUS column present”, “Month field in metadata”), not vague (“layout ok”).
- **Reference** / **Generated**: one short **factual** phrase each (not judgment).
- Rows must **match** the same checks used to compute structure % for that section. **x** = count of Yes; **y** = applicable rows.

**2) Content facet ledger** — **exactly five** rows:

| Facet | Score (0 / 0.5 / 1) | Rationale (one sentence) |
|-------|----------------------|--------------------------|
| … | … | Why this score |

- Facet names must match the **five** facets used for the section **content %** sum.

**§1 Quantitative Metrics** may keep only the **summary** row per section; **§2–§8** contain the **full ledgers**.

---

## 1. Structure and Content Summary

- **Documents:** Generated ERD vs Engineering ERD (filenames or "uploaded").
- **Reference:** Engineering ERD is the reference; we assess the generated ERD against it.
- **Overall structure match:** Aligned / Partial / Not aligned (from the perspective of the generated ERD matching the engineering ERD).
- **Match terms (structure):** **Match** = generated ERD aligns with engineering ERD for that section. **Partial** = some alignment but specific gaps (e.g. missing column, different order). **Does not match** = structure or format differs in important ways; generated ERD should be changed to align.
- **Content analysis summary (2–3 lines):** How does the engineering ERD tend to analyze/articulate requirements overall? (e.g. granularity of deliverables, use of Jira IDs, narrative vs table style, epic scoping, E2E scenario style.) One or two high-level suggestions for what the generated ERD could adopt.
- **One-line summary:** e.g. "Generated ERD matches section order; add STATUS column and consider engineering ERD’s deliverable granularity in Functional Deliverables."

### Quantitative Metrics (required)

**Summary table (headline):**

| Metric | Result | How obtained | Confidence | Why |
|--------|--------|--------------|------------|-----|
| Structural alignment | …% | Mean of section structure % (6 or 7, exclude N/A) | Low / Med / High | … |
| Content alignment | …% | Mean of section content % (6 or 7, exclude N/A) | Low / Med / High | … |
| Combined engineering alignment | …% | (Structural + Content) / 2 | Low / Med / High | weakest link or overall doc quality |

**Overall confidence** (required): one line **Low / Medium / High** + rationale.

**Per-section scores (required):**

| Section | Structure % | Structure basis (e.g. checks passed) | Content % | Content basis (e.g. facet points) | Band (optional) |
|---------|-------------|--------------------------------------|-----------|-----------------------------------|-------------------|
| Document Metadata | …% | x/y checks | …% | e.g. 4.0/5 | Match / Partial / Does not match |
| Monthly Delivery Summary | …% | … | …% | … | … |
| Functional Engineering Deliverables | …% | … | …% | … | … |
| Non-Functional Engineering Deliverables | …% | … | …% | … | … |
| Engineering Epics | …% | … | …% | … | … |
| E2E Testing Plan | …% | … | …% | … | … |
| System Context | …% | … | …% | … | … |

Add **2–3 bullets**: interpretation (largest gaps by **numeric** gap, not only band), and any **per-section** confidence callouts if one section was hard to parse.

## 2. Document Metadata

**Structure check ledger** (required) → then **Content facet ledger** (required) → then narrative.

**Structure**
- **Reference (engineering ERD):** Describe its metadata block (e.g. PROGRAM, DOMAIN, PROJECTS, SQUAD, TRACK, STEL, Engineering Manager, Technical Lead, Architect).
- **Generated ERD assessment:** Does the generated ERD have the same metadata fields and naming as the engineering ERD? List what the **generated ERD** is missing or has extra; recommend changes to the generated ERD only.
- **Table format:** N/A (metadata is usually not a table).

**Content analysis**
- **Engineering ERD:** How does it use metadata (e.g. real values vs TBD, project vs program naming, version/date style)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt from the engineering ERD’s metadata style or conventions?

## 3. Monthly Delivery Summary

**Structure check ledger** (required) → then **Content facet ledger** (required) → then narrative.

**Structure**
- **Reference (engineering ERD):** How is this section structured (e.g. single paragraph vs project-by-project blocks)?
- **Generated ERD assessment:** Does the generated ERD match placement and format? What should the generated ERD change to align?

**Content analysis**
- **Engineering ERD:** How does it articulate delivery scope (e.g. by project/release, business value, assumptions, dependencies)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. project-by-project breakdown, explicit assumptions)?

## 4. Functional Engineering Deliverables

**Structure check ledger** (required) → then **Content facet ledger** (required) → then narrative.

**Structure**
- **Reference (engineering ERD):** Table columns and order (e.g. CAPABILITY | FUNCTIONAL ENGINEERING DELIVERABLES | STATUS | DESCRIPTION).
- **Generated ERD assessment:** Does the generated ERD have the same columns and order? List changes the **generated ERD** should make to match (e.g. add STATUS column, reorder columns).

**Content analysis**
- **Engineering ERD:** How does it analyze/articulate deliverables (e.g. granularity, phrasing, use of Jira/ticket IDs, traceability to PRD/FR, one row per deliverable vs grouped)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. same level of granularity, deliverable-naming style, STATUS or acceptance phrasing)?

## 5. Non-Functional Engineering Deliverables

**Structure check ledger** (required) → then **Content facet ledger** (required) → then narrative.

**Structure**
- **Reference (engineering ERD):** Table columns and order.
- **Generated ERD assessment:** Does the generated ERD match? List changes the **generated ERD** should make to match.

**Content analysis**
- **Engineering ERD:** How does it articulate NFRs (e.g. categories, concrete targets vs TBD, linkage to tickets or epics)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. NFR categorization, level of specificity)?

## 6. Engineering Epics

If **N/A** (no comparable reference section): state why; **no ledgers**. Otherwise: **Structure check ledger** → **Content facet ledger** → narrative.

**Structure**
- **Reference (engineering ERD):** Does it have a dedicated Epics section/table? If yes, column names and order. If no, state that the engineering ERD does not use a separate Epics table.
- **Generated ERD assessment:** Should the generated ERD match the engineering ERD (e.g. add/remove Epics section, match columns)? List changes for the **generated ERD** only. Do not suggest the engineering ERD add an Epics table because the generated one has it. If the engineering ERD has no Epics section, recommend removing or de-emphasizing it in the generated ERD; never say the generated ERD is "more structured" or "better" in this area.

**Content analysis**
- **Engineering ERD:** How does it articulate epics (e.g. scoping, dependency phrasing, PRD/FR linkage, milestone usage, NOTES style)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. epic description style, dependency or milestone conventions)?

## 7. E2E Testing Plan

**Structure check ledger** (required) → then **Content facet ledger** (required) → then narrative.

**Structure**
- **Reference (engineering ERD):** How is this section presented (e.g. full table vs link to wiki)?
- **Generated ERD assessment:** Should the generated ERD match (e.g. replace in-document table with a link, or add columns)? List changes for the **generated ERD** only.

**Content analysis**
- **Engineering ERD:** How does it articulate test scenarios (e.g. scenario naming, step granularity, expected results, link to deliverables or coverage)?
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** What could the generated ERD adopt (e.g. scenario style, level of detail, column usage)?

## 8. System Context

If **N/A** (no comparable System Context in the engineering ERD and no generated block to compare): state why; **no ledgers**. Otherwise: **Structure check ledger** → **Content facet ledger** → narrative. Full metric definitions: **`system_context_schema.md`**.

**Structure**
- **Reference (engineering ERD):** Confluence/GitHub summary subsections, table shapes, column roles.
- **Generated ERD assessment:** What should the generated ERD change to align (layout, missing columns, subsection labels)?

**Content analysis**
- **Engineering ERD:** How themes, repo/stack labels, and findings are expressed (**as written**—no external verification).
- **Generated ERD:** How does it compare?
- **Suggestions for generated ERD:** Alignment with engineering style and naming only; do not judge engineering ERD.

## 9. Alignment Recommendations

- **One direction only:** Prioritized list of changes **to the generated ERD** so it matches the **engineering ERD**. Include both:
  - **Structure:** e.g. "Add STATUS column to Functional Deliverables", "Use same title/version header format as engineering ERD".
  - **Content/style:** e.g. "Adopt engineering ERD’s granularity in Functional Deliverables", "Match engineering ERD’s project-by-project breakdown in Monthly Delivery Summary".
- Do not recommend that the engineering ERD adopt anything from the generated ERD. Do not suggest new PRD content; content recommendations are about how the generated ERD expresses and analyzes requirements (style, conventions, granularity).

---

## Relationship to Step 3 (scope-aligned comparison)

Step 2 compares **entire documents**. If the **engineering ERD only covers a time window or requirement subset** (common for monthly team ERDs) while the **generated ERD reflects broader PRD scope**, Step 2 metrics can be **misleadingly low**. After Step 2, the agent should **detect** that situation (see `prompt.md`) and offer **Step 3**, which follows `scope_aligned_comparison_schema.md`—same **metric model** (checks + five facets) and **confidence**, but **scoped slice + lenient constraints**. Step 3 is an optional follow-on.

---

**Output:** Markdown only. Use tables where helpful for column/section comparison.
