# Connected apps (optional)

No required integrations. Add OAuth or API details in `connected-apps.yaml` when you connect external systems; reference env vars only, not secrets.

## SharePoint (via Power Automate HTTP trigger)

The flow typically **writes evaluation data to SharePoint** (list, library, or metadata) using the JSON body the GPT sends. The **OpenAPI** file is maintained to match the working **Power Automate** trigger—do not replace it in-repo with ad-hoc URLs.

- **Spec:** `power-automate-report.openapi.json` — `POST /invoke`, operation **`submitEvaluationReport`**, with query parameters as in the file.
- **GPT setup:** Import the JSON in **Actions**; set **server URL** and **parameter defaults** in the UI so they match your live trigger (per Power Platform). Set authentication to **None** if the spec uses **SAS** in `sig` (as defined).
- **Agent behavior:** See **`knowledge/power_automate_report.md`**. The JSON **body** is **`WorkbookSheetPayload` only**; the agent does not invent `sig` or other query values and retries **`api-version`** when the error response provides valid options.
