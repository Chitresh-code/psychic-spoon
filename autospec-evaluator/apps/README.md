# Connected apps (optional)

No required integrations. Add OAuth or API details in `connected-apps.yaml` when you connect external systems; reference env vars only, not secrets.

## Action snapshots (SharePoint upload)

| File | Purpose |
|------|---------|
| `snapshots/power_automate_sharepoint_create_file_action.png` | Screenshot of the Power Automate **Create file** step showing dynamic mappings (`siteUrl`, `folderPath`, `fileName`, `base64ToBinary` on `fileContentBase64`). Upload to Custom GPT **Knowledge** if you want the model to see the same mapping visually. |
| `openapi_power_automate_sharepoint_upload.yaml` | OpenAPI 3.1 snapshot for the **Upload file to SharePoint** action. Replace **`REPLACE_WITH_FLOW_SIGNATURE`** (and adjust `servers` / workflow path if your flow differs) before pasting into ChatGPT or sharing; do not commit real signatures to public repos. |
