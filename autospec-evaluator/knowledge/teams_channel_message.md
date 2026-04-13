# Microsoft Teams — send evaluation report (Custom GPT Action)

Use this when the **Microsoft Teams Channel Message** OpenAPI action (`sendChannelMessage`) is connected and the user asks to **send the report to Teams**, **post to Teams**, **notify the channel**, or similar.

## Hard content rules

The Teams message is a **formal evaluation report**, not a conversation or status update. These rules are **non-negotiable**:

1. **No greetings or sign-offs.** Do not start with "Team —", "Hi team", "Hello", or similar. Do not end with "Let me know", "Reach out if", or any conversational close. The message starts with the report title and ends with the last data point.
2. **No narrative prose between sections.** Do not write "Key takeaways", "Summary", "Note", or free-form paragraphs that editorialize the scores. The data speaks for itself. If a finding needs context, put it in the Recommendations table.
3. **No references to the chat session.** Do not mention ChatGPT, the conversation thread, tool failures, export issues, or session state. The Teams message is a standalone artifact — readers may never see the chat.
4. **No references to the Excel workbook.** Do not say "see the Excel export" or "download from chat." If the Excel export succeeded or failed, that is irrelevant to the Teams report.
5. **All scores in tables.** Never render metrics as bullet lists or inline text. Every metric, score, basis, and confidence value goes into a styled HTML table. No exceptions.
6. **All recommendations in a numbered table.** Never render recommendations as free-form bullets. Use a table with columns: # | Recommendation | Priority.
7. **Include every completed evaluation phase.** If Steps 1–3 are done, the message must contain all three phases with their full metric tables. Do not omit a phase or reduce it to a one-liner.
8. **Use exact scores from the evaluation.** Copy metric values, basis counts, and confidence levels verbatim from the Markdown report. Do not round, paraphrase, or approximate.

---

## When to call the action

- After **any** evaluation step (1–4) when the user wants the report delivered to the configured Teams channel.
- The message includes **every evaluation phase completed so far** — not just the latest step.

## Graph API behavior

- Channel messages use **`body.content`** and **`body.contentType`** (`text` or `html`).
- Teams **does not** interpret Markdown. Always use **`contentType: "html"`** with inline-styled HTML tables.
- If the user explicitly wants **raw Markdown source** (rare), use **`contentType: "text"`**.

## Request shape

- **Method:** `POST`
- **Path:** `/teams/{teamId}/channels/{channelId}/messages`
- **Path parameters:** Use the **default** `teamId` and `channelId` from the OpenAPI schema (never invent IDs).
- **Headers:** `Authorization: Bearer <token>` is handled by the GPT Action auth configuration.
- **JSON body:**

```json
{
  "body": {
    "contentType": "html",
    "content": "<html content here>"
  }
}
```

---

## Table styling (required for all tables)

Teams does not support `<style>` blocks or external CSS. Every table must use inline styles:

```html
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Header</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px;">Value</td>
    </tr>
    <tr style="background-color:#F9F9F9;">
      <td style="border:1px solid #ccc; padding:6px 12px;">Alternating row</td>
    </tr>
  </tbody>
</table>
```

Rules:
- `border-collapse:collapse` on every `<table>`.
- `border:1px solid #999` on `<th>`, `border:1px solid #ccc` on `<td>`.
- `padding:8px 12px` on `<th>`, `padding:6px 12px` on `<td>`.
- `background-color:#E7E6E6` on `<thead>` rows, `#F9F9F9` on alternating `<tbody>` rows.
- Left-align text columns, center-align percentage and score columns.

---

## Message size limits

Teams channel messages via Graph API have a **28 KB** body limit.

1. **Under 28 KB (typical):** Send the **full structured report** as a single message.
2. **Over 28 KB:** Split into **one message per completed phase** — each self-contained with its own title. Send sequentially.
3. **Single phase over 28 KB (rare):** Keep all metric and recommendation tables. Remove only the detailed ledger tables (structure check ledgers, content facet ledgers) and add one row to the recommendations table: "Detailed ledger tables available in the full evaluation report."

---

## Report structure (strict)

The Teams message must follow this exact structure. Every section is an HTML table — no prose sections, no bullet lists.

### 1. Report header

```html
<h2>{Project Name} — AutoSpec Evaluation Report</h2>
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px; width:200px;"><strong>PRD</strong></td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{PRD filename}</td>
    </tr>
    <tr style="background-color:#F9F9F9;">
      <td style="border:1px solid #ccc; padding:6px 12px;"><strong>Generated ERD</strong></td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{Generated ERD filename}</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px;"><strong>Engineering ERD</strong></td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{Engineering ERD filename or "Not evaluated"}</td>
    </tr>
    <tr style="background-color:#F9F9F9;">
      <td style="border:1px solid #ccc; padding:6px 12px;"><strong>Report date</strong></td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{YYYY-MM-DD}</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px;"><strong>Evaluations completed</strong></td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{e.g. PRD Analysis, Engineering ERD Comparison, Scoped Alignment Review}</td>
    </tr>
  </tbody>
</table>
```

### 2. PRD Analysis (Step 1 — if completed)

```html
<h3>PRD Analysis</h3>
<p><strong>Overall result:</strong> {Pass / Pass with gaps / Fail}</p>
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Metric</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">Result</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Basis</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">Confidence</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px;">Structural completeness</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{e.g. 7/7 sections}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{High/Med/Low}</td>
    </tr>
    <tr style="background-color:#F9F9F9;">
      <td style="border:1px solid #ccc; padding:6px 12px;">PRD coverage</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{e.g. 27/32 items}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{confidence}</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px;">Traceability accuracy</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{basis}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{confidence}</td>
    </tr>
    <tr style="background-color:#F9F9F9;">
      <td style="border:1px solid #ccc; padding:6px 12px;">Hallucination rate</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{basis}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{confidence}</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px;">Content fidelity</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{formula}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{confidence}</td>
    </tr>
    <tr style="background-color:#F9F9F9;">
      <td style="border:1px solid #ccc; padding:6px 12px;">Capability label accuracy</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{basis}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{confidence}</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px;">Section classification accuracy</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{basis}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{confidence}</td>
    </tr>
    <!-- Add System Context rows when applicable -->
  </tbody>
</table>
<p><strong>Overall confidence:</strong> {Low/Medium/High}</p>

<!-- Completeness gaps table (if any gaps exist) -->
<h4>Completeness Gaps</h4>
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">PRD Reference</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Description</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Suggested ERD Location</th>
    </tr>
  </thead>
  <tbody>
    <!-- One row per gap. If no gaps: replace entire table with <p>No gaps identified.</p> -->
  </tbody>
</table>

<!-- Misclassified deliverables table (if any) -->
<h4>Misclassified Deliverables</h4>
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">ERD ID</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Current Section</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Recommended Section</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Reason</th>
    </tr>
  </thead>
  <tbody>
    <!-- One row per misclassified item. If none: replace entire table with <p>All deliverables correctly classified.</p> -->
  </tbody>
</table>

<!-- Capability label audit table (if mismatches) -->
<h4>Capability Label Mismatches</h4>
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">ENG ID</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Current Label</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Correct PRD Capability</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Issue</th>
    </tr>
  </thead>
  <tbody>
    <!-- One row per mismatch. If none: replace entire table with <p>All capability labels match PRD.</p> -->
  </tbody>
</table>
```

### 3. Engineering ERD Comparison (Step 2 — if completed)

```html
<h3>Engineering ERD Comparison</h3>
<p><strong>Overall structure match:</strong> {Aligned / Partial / Not aligned}</p>

<!-- Headline metrics table -->
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Metric</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">Result</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">How Obtained</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">Confidence</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px;">Structural alignment</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{how}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{confidence}</td>
    </tr>
    <tr style="background-color:#F9F9F9;">
      <td style="border:1px solid #ccc; padding:6px 12px;">Content alignment</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{how}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{confidence}</td>
    </tr>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px;">Combined engineering alignment</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{%}</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{how}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">{confidence}</td>
    </tr>
  </tbody>
</table>

<!-- Per-section scores table -->
<h4>Per-Section Scores</h4>
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Section</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">Structure %</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Structure Basis</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">Content %</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Content Basis</th>
    </tr>
  </thead>
  <tbody>
    <!-- One row per canonical section: Document Metadata, Monthly Delivery Summary, 
         Functional Deliverables, Non-Functional Deliverables, Engineering Epics, 
         E2E Testing Plan, System Context. Use N/A when a section does not apply. -->
  </tbody>
</table>
<p><strong>Overall confidence:</strong> {Low/Medium/High}</p>
```

### 4. Scoped Alignment Review (Step 3 — if completed)

```html
<h3>Scoped Alignment Review</h3>

<!-- Scope declaration table -->
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px; width:200px;"><strong>Engineering ERD scope</strong></td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{inferred scope, e.g. "Review-status enhancement — April 2026"}</td>
    </tr>
    <tr style="background-color:#F9F9F9;">
      <td style="border:1px solid #ccc; padding:6px 12px;"><strong>Generated ERD slice</strong></td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{which generated ERD parts were compared}</td>
    </tr>
  </tbody>
</table>

<!-- Scoped headline metrics table (same structure as Step 2 headline table, 
     but metric names include "(scoped)") -->

<!-- Scoped per-section scores table (same structure as Step 2 per-section table) -->

<!-- Scope fairness note as a single-row table -->
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <tbody>
    <tr style="background-color:#F9F9F9;">
      <td style="border:1px solid #ccc; padding:6px 12px;"><strong>Step 2 vs Step 3:</strong> {e.g. "Step 2 combined 45.0% → Step 3 scoped 80.0%. Delta driven by excluding out-of-scope generated rows."}</td>
    </tr>
  </tbody>
</table>
<p><strong>Overall confidence:</strong> {Low/Medium/High}</p>
```

### 5. System Context Comparison (Step 4 — if completed)

```html
<h3>System Context Comparison</h3>

<!-- Table A: per-ERD metrics -->
<h4>Per-ERD Metrics</h4>
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Metric</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">ERD A (%)</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">ERD B (%)</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">Confidence</th>
    </tr>
  </thead>
  <tbody>
    <!-- Rows: Context–PRD alignment, Context relevance, Structural completeness, Unsupported claims rate -->
  </tbody>
</table>

<!-- Table B: cross-ERD pair metrics -->
<h4>Cross-ERD Metrics</h4>
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Metric</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">Result</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center;">Confidence</th>
    </tr>
  </thead>
  <tbody>
    <!-- Rows: Thematic consistency, Inter-ERD consistency, Mention overlap, Overall Step 4 score -->
  </tbody>
</table>
<p><strong>Overall confidence:</strong> {Low/Medium/High}</p>
```

### 6. Recommendations (always last)

```html
<h3>Recommendations</h3>
<table style="border-collapse:collapse; width:100%; font-family:Segoe UI,sans-serif; font-size:14px;">
  <thead>
    <tr style="background-color:#E7E6E6;">
      <th style="border:1px solid #999; padding:8px 12px; text-align:center; width:40px;">#</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:left;">Recommendation</th>
      <th style="border:1px solid #999; padding:8px 12px; text-align:center; width:80px;">Priority</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">1</td>
      <td style="border:1px solid #ccc; padding:6px 12px;">{recommendation text}</td>
      <td style="border:1px solid #ccc; padding:6px 12px; text-align:center;">High</td>
    </tr>
    <!-- Continue for all recommendations. Merge recommendations from all completed steps
         into one consolidated, de-duplicated, priority-ordered table. -->
  </tbody>
</table>
```

---

## Building the HTML from a completed Markdown report

1. **Map every Markdown table** to an HTML `<table>` with the inline styles above. Do not skip tables.
2. **Map Markdown headers** (`##` to `<h2>`, `###` to `<h3>`, `####` to `<h4>`).
3. **Do not add any content that is not in the Markdown report.** No greetings, no commentary, no session references.
4. **Do not remove any scored content from the Markdown report.** If a table exists in the report, it must appear in the Teams message (unless hitting the 28 KB limit — see Message size limits).
5. **Apply alternating row colors** on `<tbody>` rows: odd rows default background, even rows `background-color:#F9F9F9`.
6. **Consolidate recommendations** from all completed steps into one table at the end, de-duplicated and ordered by priority.

## After a successful post

Confirm to the user in chat that the message was sent to Teams. Then show the **What would you like to do next?** options from `workflow_operations.md` if still relevant.

If the action fails (auth, network): state the error briefly and offer to **retry** after the user checks the connector or token.

## OAuth / setup (human operator)

Configure the Custom GPT **Action** with Microsoft identity and **Graph permissions** that allow posting channel messages (delegated or application permissions per your tenant policy — consult current Microsoft Graph docs for `ChannelMessage.Send` or equivalent). Store **no** tokens in Knowledge files; use the GPT's **Authentication** UI only.
