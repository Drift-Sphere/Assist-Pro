# Workflow: Intelligent Meeting Follow-Up 🤝

This workflow demonstrates calendar parsing, content generation, and multi-tool orchestration.

## 1. User Prompt
> "Send a follow-up email to everyone from the 2 PM design sync. Summarize that we agreed to update the nav bar, and create a tracking task in Notion for next Tuesday."

## 2. Tool Selection Plan
1. `CALENDAR_GET_EVENT`: Identify the "2 PM design sync" and extract attendee emails.
2. `MEMORY_RETRIEVE`: Fetch the user's standard sign-off and tone.
3. `GMAIL_DRAFT`: Draft the summary email to the extracted attendees.
4. `NOTION_CREATE_PAGE`: Initialize a task in the connected Notion roadmap database.

## 3. Actions Executed
- **Action 1 (Success)**: Found event "Design Sync" at 14:00. Extracted attendees: `sarah@company.com`, `mike@company.com`.
- **Action 2 (Success)**: Retrieved style context.
- **Action 3 (Pending Approval)**: Drafted email to Sarah and Mike summarizing the nav bar decision.
- **Action 4 (Success)**: Created page "Update Nav Bar" in Notion with a due date of next Tuesday.

## 4. Output / Audit Log
The user sees a dashboard card:
*"Notion task 'Update Nav Bar' created. Draft email to Sarah and Mike is ready for review."*
User clicks "Approve & Send" on the email.

## 5. Failure Handling
**Scenario:** The user has not connected Notion.
**Result:** The orchestrator catches the missing permission *during the planning phase*, before any actions execute.
**Resolution:** The AI responds immediately: *"I can draft the email, but I cannot create the task because your Notion account is not connected. Would you like me to just draft the email?"*
