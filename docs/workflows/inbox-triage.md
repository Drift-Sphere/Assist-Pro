# Workflow: Proactive Inbox Triage 📥

This workflow highlights the background Cron Engine and text analysis capabilities.

## 1. User Prompt / Trigger
*Trigger:* The `DigestAgent` cron job runs automatically at 8:00 AM.
*System Instruction:* "Scan the user's inbox for high-priority emails from the last 24 hours. Summarize them and draft suggested replies for urgent items."

## 2. Tool Selection Plan
1. `GMAIL_LIST_MESSAGES`: Fetch unread emails from the last 24 hours.
2. `LLM_CLASSIFY`: Score emails based on urgency and sender importance.
3. `GMAIL_DRAFT`: Generate replies for emails scoring > 8/10 urgency.
4. `SLACK_POST`: Send the morning summary to the user's private Slack channel.

## 3. Actions Executed
- **Action 1 (Success)**: Retrieved 42 unread emails.
- **Action 2 (Success)**: Identified 1 urgent email from a VIP client asking for a project update.
- **Action 3 (Success)**: Drafted a status update reply to the VIP client.
- **Action 4 (Success)**: Posted message to Slack: *"Morning! You have 42 new emails. 1 requires urgent attention from [VIP Client]. I have placed a drafted reply in your inbox."*

## 4. Output / Audit Log
The user wakes up, reads the Slack message, opens Gmail, reviews the draft, makes a minor edit, and hits Send.

## 5. Failure Handling
**Scenario:** The user's Gmail OAuth token has expired.
**Result:** Action 1 fails with a `401 Unauthorized`.
**Resolution:** The cron job halts. A system notification is sent to the user via email/Slack: *"Assist Pro could not run your morning digest because your Google connection expired. Please click here to reconnect."*
