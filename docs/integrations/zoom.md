# Integration: Zoom 📹

Assist Pro integrates with Zoom to manage your video conferencing seamlessly alongside your calendar.

## 🔒 Scopes & Permissions
- `meeting:write`: Create and delete meetings.
- `meeting:read`: Retrieve meeting details and recordings.

## ⚡ Supported Actions
- `ZOOM_CREATE_MEETING`: Generate a meeting link and password.
- `ZOOM_GET_RECORDING`: Fetch the cloud recording link of a past meeting for the AI to summarize.

## ⚠️ Approval-Sensitive Actions
- Generating a meeting link is a safe action and occurs instantly (usually chained with a `GMAIL_DRAFT` action to send the link to a client).

## 🛑 Limitations
- The AI cannot start or host a meeting for you. It only manages the scheduling and retrieval of artifacts.
