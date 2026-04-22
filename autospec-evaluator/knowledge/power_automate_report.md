# SharePoint — upload evaluation report (Power Automate HTTP trigger)

Use this when the **SharePoint report upload** action (`submitEvaluationReport`) is connected and the user asks to **upload the report to SharePoint**, **push the evaluation to SharePoint**, **send the report to SharePoint**, or similar. The **Power Automate** OpenAPI spec (`power-automate-report.openapi.json`) is the source of truth: it defines the path, **query** parameters, and the JSON **body** schema. The flow (e.g. create or update **SharePoint** list items) receives **`WorkbookSheetPayload`** in the request body.

## JSON body: `WorkbookSheetPayload` only

- The **body** of each call is **only** the object defined as **`WorkbookSheetPayload`** in the OpenAPI file—no extra top-level properties.
- **Never** put trigger **query** parameters in the JSON body: not `api-version`, `sp`, `sv`, or `sig`, and not placeholders (e.g. `connected-action`, `sig: "1"`). Those belong in the **HTTP request** as defined by the action (query string), not inside the body.

## Query parameters: use the OpenAPI action—do not invent values

- **`submitEvaluationReport`** is defined in `power-automate-report.openapi.json` with **query** parameters (`api-version`, `sp`, `sv`, `sig`, etc.) as the platform expects. When calling the **connector / action**, supply **only** the values the OpenAPI spec and the Custom GPT **Actions** UI expose (defaults from a correct import match your working Power Automate trigger URL).
- **Do not** guess or invent `sig` or any other query value. If the UI shows fixed defaults from the spec, use those. Do not substitute narrative text or made-up tokens for `sig`.
- **Do not** merge the whole trigger URL or its query into the request **body** as key-value fields.

## If the call fails: `api-version` (and retriable parameters)

- When the service responds with an error that **lists supported or valid** `api-version` values (or a clear message that the `api-version` is wrong), **read that response** and call **`submitEvaluationReport` again** with:
  - the **same** `WorkbookSheetPayload` body (unchanged), and
  - the **query** parameters updated to use a **`api-version` value** from the error message (or the single value it recommends), leaving **`sp`**, **`sv`**, and **`sig`** as specified by the working OpenAPI import unless the response says otherwise.
- If the error does not mention `api-version` (e.g. network timeout, 401, invalid `sig`), do **not** fabricate a new `api-version`—report the error, offer a **manual retry** after the user or admin confirms the trigger URL and action configuration.

---

## Payload shape (single schema, one flow)

Every POST uses the same **`WorkbookSheetPayload`** body:

1. **Common fields** (repeat on **every** call): `projectName`, `projectSlug`, `reportDate`, `prdFilename`, `generatedErdFilename`, `engineeringErdFilename`, `workbookTitle`, and usually `engineeringErdDisplay`, `evaluationsIncludedLabels`, `completedSteps`, `overallResult`, `overallConfidence`.
2. **`sheetName`** — which Excel-equivalent sheet this request carries (matches `xlsx_report_export.md` tab names).
3. **Exactly one sheet-content key** is populated; **omit** all other content keys (do not send large empty objects).

| `sheetName` | Populate this key only | Excel sheet |
|-------------|------------------------|-------------|
| `Report_Overview` | `reportOverview` | Report_Overview |
| `PRD_Analysis` | `prdAnalysis` | PRD_Analysis |
| `Eng_ERD_Comparison` | `engineeringErdComparison` | Eng_ERD_Comparison |
| `Scoped_Alignment` | `scopedAlignmentReview` | Scoped_Alignment |
| `System_Context_Comp` | `systemContextComparison` | System_Context_Comp |
| `Recommendations` | `recommendations` (array) | Consolidated recommendations (not a separate Excel tab; use this name for the final list POST) |

**Inactive keys:** Leave them **out of the JSON** (preferred). Do not duplicate `null` for every unused key unless your client requires it.

---

## When to call the action

- After the user asks to upload to SharePoint (or push the report through the flow), send **one POST per sheet** that exists for the completed evaluations.
- **Skip sheets** for phases that did not run (e.g. no `System_Context_Comp` if Step 4 did not run).

## Recommended send order

Send **detail sheets first** (so scores exist), then **overview**, then **recommendations**:

1. `PRD_Analysis` (if Step 1 completed)
2. `Eng_ERD_Comparison` (if Step 2 completed)
3. `Scoped_Alignment` (if Step 3 completed)
4. `System_Context_Comp` (if Step 4 completed)
5. `Report_Overview` — full `overviewScoreSummary` headline table (same rows as Excel Sheet 1 section B)
6. `Recommendations` — consolidated `recommendations` array

If you must send **`Report_Overview` first** (e.g. to register the run), use `reportOverview.overviewScoreSummary`: `[]` and send **`Report_Overview` again** after all phases with the full score rows, **or** send **`Report_Overview` only once** after detail sheets (preferred).

Use the **same** `projectSlug` and `reportDate` on every call in one session.

---

## Parity with Excel (`xlsx_report_export.md`)

| Excel workbook | JSON |
|----------------|------|
| Common metadata on **Report_Overview** (filenames, evaluations list, title) | Root fields: `prdFilename`, `generatedErdFilename`, `engineeringErdFilename`, `engineeringErdDisplay`, `reportDate`, `evaluationsIncludedLabels`, `workbookTitle` |
| **Report_Overview** — Score summary | `sheetName` `Report_Overview` → `reportOverview.overviewScoreSummary` |
| **PRD_Analysis** | `sheetName` `PRD_Analysis` → `prdAnalysis` |
| **Eng_ERD_Comparison** | `sheetName` `Eng_ERD_Comparison` → `engineeringErdComparison` |
| **Scoped_Alignment** | `sheetName` `Scoped_Alignment` → `scopedAlignmentReview` |
| **System_Context_Comp** | `sheetName` `System_Context_Comp` → `systemContextComparison` |
| Consolidated recommendations | `sheetName` `Recommendations` → `recommendations` |

**Where numeric scores live:** Same as before — e.g. `prdAnalysis.metrics[].resultPercent`, `engineeringErdComparison.headlineMetrics`, `systemContextComparison.perErdMetrics` / `crossErdMetrics`.

---

## Hard content rules

1. **Use exact scores from the Markdown report.** Copy metric values, basis counts, and confidence levels verbatim. Do not round, paraphrase, or approximate.
2. **Keep individual field values short.** Descriptions ≤ 200 characters. Rationales and reasons ≤ 150 characters. Summaries ≤ 400 characters. If a finding is longer, split across the `description` and `reason` fields or truncate the detail (the full report lives in the Markdown; JSON is structured data, not prose).
3. **Use -1 for N/A percentages.** When a metric is not applicable, set `resultPercent`, `structurePercent`, `contentPercent`, `erdAPercent`, or `erdBPercent` to **-1** (not null, not an empty string). Add `"N/A"` in the `basis` or `why` field with a short reason.
4. **Empty arrays for clean findings.** When there are no gaps, no hallucinations, no misclassifications, etc., send an **empty array** `[]` — not null, not a missing key.
5. **No HTML.** All values are plain text. No inline styles, no tags.
6. **No chat-session references.** Do not mention ChatGPT, the conversation, tool failures, or export issues in any field value.

---

## Example: `Report_Overview` POST (body only)

```json
{
  "projectName": "Exact PRD project name",
  "projectSlug": "example-project",
  "reportDate": "2026-04-16",
  "prdFilename": "ProjectX_PRD.docx",
  "generatedErdFilename": "ProjectX_Generated_ERD.docx",
  "engineeringErdFilename": "ProjectX_Eng_ERD.docx",
  "engineeringErdDisplay": "ProjectX_Eng_ERD.docx",
  "workbookTitle": "Exact PRD project name Evaluation Report",
  "evaluationsIncludedLabels": ["PRD Analysis", "Engineering ERD Comparison"],
  "completedSteps": ["Step 1", "Step 2"],
  "overallResult": "Pass with gaps",
  "overallConfidence": "Medium",
  "sheetName": "Report_Overview",
  "reportOverview": {
    "overviewScoreSummary": [
      { "phase": "PRD Analysis", "metric": "Structural completeness", "score": "87%" },
      { "phase": "PRD Analysis", "metric": "Overall confidence", "score": "Medium" }
    ]
  }
}
```

---

## Building `prdAnalysis` (Step 1)

Source: the Markdown report from `evaluation_report_schema.md`.

### `summary`

| Field | Source |
|-------|--------|
| `overallResult` | §1 Evaluation Summary overall result |
| `oneLiner` | §1 one-line summary |
| `overallConfidence` | §2 overall confidence |
| `confidenceRationale` | §2 confidence sentence |

### `metrics`

One `MetricRow` per row in the §2 Quantitative Metrics table. Include all core metrics **and** System Context metrics when applicable:

- Structural completeness
- PRD coverage
- Traceability accuracy
- Hallucination rate
- Content fidelity (PRD)
- Capability label accuracy
- Section classification accuracy
- System Context structural completeness (if block exists)
- Context–PRD alignment (if block exists)
- Context relevance (if block exists)
- Unsupported / contradictory context claims (if block exists)

### `countingBasis`

From the §2 "Counting basis" subsection. One short sentence per rule.

### `completenessGaps`

One object per row in §3 Completeness gaps table. Empty array if no gaps.

### `misclassifiedDeliverables`

One object per row in §3 Misclassified deliverables table. Empty array if all correct.

### `capabilityLabelMismatches`

One object per row in §5 Capability label audit table. Empty array if all match.

### `hallucinations`

One object per **§4a** row only (true hallucinations). **Omit** §4b (*Traced to System Context, not in PRD*); the flow does not carry those. Empty array if none.

### `traceability`

From §5 Traceability Summary: totals, accuracy, broken links, milestone violations.

### `structuralFlags`

From §3 Structural recommendations: `ownerAttributionMissing` and `repositoryCoverageGap` as booleans, with short detail strings.

---

## Building `engineeringErdComparison` (Step 2)

Source: the Markdown report from `structure_analysis_schema.md`.

### `summary`

From §1 Structure and Content Summary: overall structure match, one-liner, content analysis summary, confidence, scope assessment.

### `headlineMetrics`

Exactly three `MetricRow` objects from the §1 headline table:
1. Structural alignment
2. Content alignment
3. Combined engineering alignment

### `sectionScores`

One `SectionScore` per canonical section (6 or 7 rows) from the §1 per-section table.

### `structureChecks` (flat array)

Collect **all** structure check ledger rows from **§2 through §8** into one flat array. Each row carries its `section` name so the flow can filter or group.

- Copy `check`, `reference`, `generated` values verbatim but keep each ≤ 150 characters.
- Preserve the `checkNumber` (1-based within each section).

### `contentFacets` (flat array)

Collect **all** content facet ledger rows from **§2 through §8** into one flat array. Each row carries its `section` name. Exactly 5 facets per scored section (skip N/A sections).

---

## Building `scopedAlignmentReview` (Step 3)

Source: the Markdown report from `scope_aligned_comparison_schema.md`.

Same structure as Step 2 but with:
- `declaredScope` from §1 (engineering scope, how inferred, generated slice)
- `scopeFairnessNote` from §3 (Step 2 vs Step 3 delta)
- Headline metrics labeled "(scoped)"
- Structure checks and content facets evaluated on the declared slice only

---

## Building `systemContextComparison` (Step 4)

Source: the Markdown report from `system_context_schema.md` Step 4.

### `summary`

ERD labels, option (A or B), PRD filename, confidence.

### `perErdMetrics`

One row per metric in Table A. Always show both `erdAPercent` and `erdBPercent`.

### `crossErdMetrics`

One row per metric in Table B. `result` is a string (percentage or qualitative label).

### `contradictions`

One row per entry in the contradiction ledger. Empty array if no contradictions.

### `mentionOverlap` (optional)

One row per mention overlap item. Omit the array if mention overlap was not computed.

---

## Building `recommendations`

Source: the consolidated Recommendations section from all completed steps.

One `Recommendation` object per row, de-duplicated and ordered by priority (High → Medium → Low). `sourceStep` identifies which evaluation step produced the recommendation. Use `sheetName` **`Recommendations`** and populate only the **`recommendations`** array.

---

## Payload size guidance

Each POST should stay **well under 100 KB**. With one sheet per request and short field values, typical payloads are:

| Sheet | Typical size |
|-------|----------------|
| `Report_Overview` | ~0.5–3 KB |
| `PRD_Analysis` | 3–10 KB |
| `Eng_ERD_Comparison` | 5–15 KB |
| `Scoped_Alignment` | 5–15 KB |
| `System_Context_Comp` | 2–8 KB |
| `Recommendations` | 1–5 KB |

If a sheet payload approaches 50 KB (very large ERD with many sections): drop `structureChecks` and `contentFacets` arrays from that call. Add a recommendation row: "Detailed ledger data exceeds payload limit — available in the full Markdown report."

---

## Power Automate flow design (for the human operator)

The flow receives each POST on the same HTTP trigger. Recommended design:

1. **Trigger:** "When an HTTP request is received" — paste the JSON schema from `power-automate-report.openapi.json` → **`WorkbookSheetPayload`** (or regenerate `power-automate-http-trigger-schema.json`).
2. **Switch** on `triggerBody()?['sheetName']`:
   - `Report_Overview` — store/update overview score summary, initialize correlation keys from root fields.
   - `PRD_Analysis` — store Step 1 data.
   - `Eng_ERD_Comparison` — store Step 2 data.
   - `Scoped_Alignment` — store Step 3 data.
   - `System_Context_Comp` — store Step 4 data.
   - `Recommendations` — store recommendations; optionally email summary or other notifications per your design.
3. **Response:** Return `{"status": "accepted", "message": "Sheet received"}` with HTTP 200.

### Custom GPT / operator setup (do not edit the OpenAPI in-repo)

- Import **`power-automate-report.openapi.json`** in **Actions** as provided (working Power Automate spec). The human operator ensures **server base URL** and **action** defaults match the real **HTTP request** trigger from Power Platform.
- **SAS / `sig`:** No Bearer token; `sig` is a query parameter in the spec. The agent only invokes the **published action**—it does not rewrite the spec file in the repository.

---

## After a successful submission

Confirm to the user in chat: data for **{sheet name}** was **submitted to the flow** (for SharePoint storage per the flow). If sending multiple sheets, confirm after each call completes (or summarize).

If a call fails: state the error briefly. **If** the response includes **valid `api-version` values** (or equivalent), call **`submitEvaluationReport` again** with the same **`WorkbookSheetPayload`** and the **corrected `api-version`** from the response (per **If the call fails** above). For other errors, offer a **user-driven retry** after they confirm the flow; do not invent query parameters.

Then show the **What would you like to do next?** options from `workflow_operations.md` if still relevant.
