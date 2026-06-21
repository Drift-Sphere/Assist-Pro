# Workflow: Autonomous Lead Generation 🔍

This workflow demonstrates how Assist Pro uses its web agent and CRM integrations to autonomously build a lead pipeline.

## 1. User Prompt
> "Use the web agent to find 3 B2B SaaS companies in London. Get their CEO's name, add them as contacts in HubSpot, and draft a tailored introductory email to each, using my standard sales style."

## 2. Tool Selection Plan
The Orchestrator parses the intent and plans the following execution chain:
1. `WEB_BROWSE`: Deploy browser agent to search for targets.
2. `HUBSPOT_SEARCH_CONTACT`: Check if CEOs already exist in CRM (prevent duplicates).
3. `HUBSPOT_CREATE_CONTACT`: Add new leads to CRM.
4. `MEMORY_RETRIEVE`: Fetch the user's `UserStyleProfile` (Digital Twin).
5. `GMAIL_DRAFT`: Generate and save personalized drafts.

## 3. Actions Executed
- **Action 1 (Success)**: Web agent returns JSON with 3 companies and CEOs.
- **Action 2 (Success)**: HubSpot query confirms 0 matches.
- **Action 3 (Success)**: 3 new contacts added to HubSpot.
- **Action 4 (Success)**: Style profile retrieved (Tone: "Friendly but concise").
- **Action 5 (Pending Approval)**: 3 emails drafted using the scraped context and user tone.

## 4. Output / Audit Log
The user receives a notification:
*"Lead Generation Complete. 3 contacts added to HubSpot. 3 emails await your approval in Gmail Drafts."*

An exact log of the `WEB_BROWSE` output and `HUBSPOT_CREATE` payload is saved to the Activity Feed.

## 5. Failure Handling
**Scenario:** HubSpot API is down.
**Result:** Action 1 succeeds. Action 2 fails with `503 Service Unavailable`. 
**Resolution:** The orchestrator aborts Actions 3, 4, and 5. The user is notified: *"Workflow paused. HubSpot API is currently unreachable. The 3 scraped leads have been saved to your temporary memory for later processing."*
