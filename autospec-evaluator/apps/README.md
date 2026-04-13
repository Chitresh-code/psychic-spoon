# Connected apps (optional)

No required integrations. Add OAuth or API details in `connected-apps.yaml` when you connect external systems; reference env vars only, not secrets.

## Microsoft Teams (Graph channel message)

- **Spec:** `teams-channel-message.openapi.yaml` — `POST /teams/{teamId}/channels/{channelId}/messages`, operation **`sendChannelMessage`**.
- **GPT setup:** Import the YAML in **Actions**, configure Microsoft Graph **Bearer** auth, adjust path-parameter **defaults** for your team/channel.
- **Agent behavior:** See **`knowledge/teams_channel_message.md`** (upload as Knowledge with the evaluator).
