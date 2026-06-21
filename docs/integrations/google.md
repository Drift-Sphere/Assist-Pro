# Integration: Google Workspace 📧

Assist Pro integrates deeply with Gmail, Calendar, and Sheets to automate your daily operations.

## 🔒 Scopes & Permissions
We practice strict data minimization.
- **Gmail**: `https://www.googleapis.com/auth/gmail.modify` (Allows reading, drafting, and sending).
- **Calendar**: `https://www.googleapis.com/auth/calendar.events` (Allows managing events, cannot delete calendars).
- **Sheets**: `https://www.googleapis.com/auth/drive.file` (**CRITICAL**: We do not use `drive.readonly`. We only have access to files the AI creates, or files you explicitly select via the Google Picker UI).

## ⚡ Supported Actions
- `GMAIL_READ`: Search and summarize threads.
- `GMAIL_DRAFT`: Create an email draft.
- `GMAIL_SEND`: Dispatch an email.
- `CALENDAR_GET`: Find free time or specific events.
- `CALENDAR_CREATE`: Schedule a new meeting.
- `SHEETS_CREATE`: Initialize a new spreadsheet.
- `SHEETS_APPEND`: Add rows to an authorized spreadsheet.

## ⚠️ Approval-Sensitive Actions
- `GMAIL_SEND` **always** requires explicit human approval via the dashboard. It cannot be fully automated by the AI.

## 🛑 Limitations
- The AI cannot delete emails (only archive).
- The AI cannot access Google Docs or Google Drive files unless specifically built into a future integration update.
