# Integration: HubSpot

Turn Assist Pro into an autonomous Sales Development Representative with direct CRM access.

## Required OAuth Scopes
- `crm.objects.contacts.read` & `write`
- `crm.objects.deals.read` & `write`
- `crm.objects.companies.read` & `write`

## Supported Actions
- `HUBSPOT_CREATE_CONTACT`: Add scraped leads directly to your CRM.
- `HUBSPOT_UPDATE_LIFECYCLE_STAGE`: Move a contact from 'Lead' to 'MQL' or 'Customer'.
- `HUBSPOT_CREATE_NOTE`: Log automated web research or meeting summaries to a contact's timeline.
- `HUBSPOT_ATTACH_MEETING_SUMMARY`: Pin generated meeting notes to the relevant Deal record.
- `HUBSPOT_CREATE_TASK`: Assign follow-up tasks to human sales reps.

## What Actions Require Approval?
- **`HUBSPOT_UPDATE_LIFECYCLE_STAGE` requires human approval** by default, to protect reporting integrity.
- Creating notes, tasks, or initial contacts are considered "Safe Actions."

## Known Limitations
- Custom Object manipulation is not supported out-of-the-box and requires specific schema mapping via custom prompts.

## Rate-Limit or Quota Considerations
- Complies with HubSpot's 10-second and daily API request limits. Bulk contact imports via the AI agent are throttled to 50 contacts per batch.
