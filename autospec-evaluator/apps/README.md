# Connected apps (optional)

No required integrations. Add OAuth or API details in `connected-apps.yaml` when you connect external systems; reference env vars only, not secrets.

## Microsoft Teams (Graph channel message)

- **Spec:** `teams-channel-message.openapi.yaml` — `POST /teams/{teamId}/channels/{channelId}/messages`, operation **`sendChannelMessage`**.
- **GPT setup:** Import the YAML in **Actions**, configure Microsoft Graph **Bearer** auth, adjust path-parameter **defaults** for your team/channel.
- **Agent behavior:** See **`knowledge/teams_channel_message.md`** (upload as Knowledge with the evaluator).

## Power Automate (HTTP trigger — evaluation report as JSON)

- **Spec:** `power-automate-report.openapi.json` — `POST /invoke`, operation **`submitEvaluationReport`**.
- **GPT setup:** Import the JSON in **Actions**, replace the **server URL** with your flow's HTTP trigger base URL, update the **`sig` default** with your SAS signature, set authentication to **None**.
- **Agent behavior:** See **`knowledge/power_automate_report.md`** (upload as Knowledge with the evaluator). The agent sends one POST per Excel sheet — common workbook fields every time; the flow switches on **`sheetName`**; only the matching content key is populated.
