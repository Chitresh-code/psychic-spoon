# AutoSpec Evaluator — Custom GPT package

## Layout

| Path | Purpose |
|------|---------|
| **`prompt.md`** | Paste into the GPT **Instructions** field |
| **`knowledge/`** | Upload **every** `.md` file as **Knowledge** |
| **`agent.yaml`** | Metadata (name, model, capabilities) for sync scripts / docs |
| **`apps/`** | Optional integrations (`connected-apps.yaml`) only if you add external APIs later |

## Setup (ChatGPT)

1. Create a custom GPT.
2. Paste **`prompt.md`** into **Instructions**.
3. Add **Knowledge**: upload the schema/ops `.md` files in **`knowledge/`** (`workflow_operations.md`, `evaluation_report_schema.md`, `structure_analysis_schema.md`, `scope_aligned_comparison_schema.md`, `system_context_schema.md`, `xlsx_report_export.md`). If you connect **Microsoft Teams** (below), also upload **`teams_channel_message.md`**. `knowledge/README.md` is optional.
4. Enable **Code interpreter & data analysis** and **Canvas** (optional).
5. **Optional — Teams:** In **Actions**, import **`apps/teams-channel-message.openapi.yaml`** (or paste your equivalent OpenAPI). Set **Authentication** to OAuth 2.0 / Bearer for Microsoft Graph; grant permissions your tenant allows for posting to the target channel. Update **`teamId`** / **`channelId`** defaults in the schema if the channel changes. Then upload **`teams_channel_message.md`** as Knowledge so the model formats messages correctly.

## Repo mirror

The same agent also lives under `agents/autospec-evaluator/` in the workspace for version control and automation.
