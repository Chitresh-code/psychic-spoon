# AutoSpec Evaluator — Custom GPT package

## Layout

| Path | Purpose |
|------|---------|
| **`prompt.md`** | Paste into the GPT **Instructions** field |
| **`knowledge/`** | Upload **every** `.md` file as **Knowledge** |
| **`agent.yaml`** | Metadata (name, model, capabilities) for sync scripts / docs |
| **`apps/`** | Optional future integrations (see `connected-apps.yaml`) |

## Setup (ChatGPT)

1. Create a custom GPT.
2. Paste **`prompt.md`** into **Instructions**.
3. Add **Knowledge**: upload the five schema/ops `.md` files in **`knowledge/`** (`workflow_operations.md`, `evaluation_report_schema.md`, `structure_analysis_schema.md`, `scope_aligned_comparison_schema.md`, `xlsx_report_export.md`); `knowledge/README.md` is optional.
4. Enable **Code interpreter & data analysis** and **Canvas** (optional).

## Repo mirror

The same agent also lives under `agents/autospec-evaluator/` in the workspace for version control and automation.
