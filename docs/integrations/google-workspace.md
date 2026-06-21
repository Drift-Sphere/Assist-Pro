# Integration: Google Workspace

Assist Pro deeply integrates with Gmail, Calendar, and Sheets, balancing powerful automation with strict privacy scopes.

## Required OAuth Scopes
- **Gmail**: `https://www.googleapis.com/auth/gmail.modify` (Read, draft, and send capability)
- **Calendar**: `https://www.googleapis.com/auth/calendar.events` (Manage events, but cannot delete the calendar itself)
- **Sheets**: `https://www.googleapis.com/auth/drive.file` (**Note**: We do not use `drive.readonly` or full `drive`. The AI can only access spreadsheets it created itself or ones you explicitly select via the Google Picker UI).

## Supported Actions
- `GMAIL_SEND_EMAIL`: Send a fully composed email.
- `GMAIL_DRAFT_EMAIL`: Place an email in the drafts folder.
- `GMAIL_LABEL_THREAD`: Organize inbox threads automatically.
- `CALENDAR_READ_AVAILABILITY`: Check schedule for free blocks.
- `CALENDAR_CREATE_EVENT`: Schedule meetings and invite attendees.
- `SHEETS_APPEND_ROW`: Add data to a specific, authorized spreadsheet.

## What Actions Require Approval?
- **`GMAIL_SEND_EMAIL` ALWAYS requires explicit human approval** via the dashboard before it actually fires.
- `CALENDAR_CREATE_EVENT` and `SHEETS_APPEND_ROW` are generally considered "Safe" but can be set to require approval via Admin settings.

## Known Limitations
- Assist Pro cannot delete your emails (it can only archive or trash them depending on labels).
- It cannot read arbitrary Google Drive files (Docs, Slides) without explicit future integrations.

## Rate-Limit or Quota Considerations
- Google API requests are subject to standard Google Workspace quotas. Assist Pro implements exponential backoff for `429 Too Many Requests`. Heavy batch operations (e.g., adding 500 rows to Sheets) are throttled internally to prevent quota exhaustion.
