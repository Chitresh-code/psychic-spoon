# Sample Agent

Use this folder as a **template** when adding a new custom GPT to the repo.

## Layout

| Path | Purpose |
|------|---------|
| `prompt.md` | Paste into the GPT **Instructions** field |
| `knowledge/` | Add `.md` (or other) files and upload them as **Knowledge** in the builder |
| `agent.yaml` | Name, slug, model hint, capabilities, conversation starters |
| `apps/` | Optional integrations (`connected-apps.yaml`, notes in `README.md`) |

## Next steps

1. Copy this folder: `cp -R sample-agent ../your-agent-slug` (or duplicate in the UI).
2. Edit `agent.yaml` and `prompt.md` for your product.
3. Replace `knowledge/README.md` with real docs; add more files as needed.
4. Wire **Capabilities** in the GPT builder to match `agent.yaml`.
