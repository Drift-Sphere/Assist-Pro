# Workflow: CRM Update via Voice 🗣️

This workflow highlights the Voice-to-Task capability and intent extraction.

## 1. User Prompt / Trigger
*Trigger:* User clicks the microphone icon in the dashboard or mobile view.
*Voice Input:* "Hey, I just got off the phone with David Smith. Change his deal stage to 'Negotiation' and add a note that he wants a 10% discount."

## 2. Tool Selection Plan
1. `VOICE_STT`: Transcribe the audio via Whisper/Groq.
2. `HUBSPOT_SEARCH_CONTACT`: Find "David Smith" in the CRM.
3. `HUBSPOT_UPDATE_DEAL`: Modify the deal stage for the associated contact.
4. `HUBSPOT_ADD_NOTE`: Append the discount request to the contact timeline.

## 3. Actions Executed
- **Action 1 (Success)**: Audio converted to text perfectly.
- **Action 2 (Success)**: Found David Smith (ID: 9482).
- **Action 3 (Pending Approval)**: Prepared payload to change deal stage to `contract_negotiation`.
- **Action 4 (Pending Approval)**: Prepared note payload.

## 4. Output / Audit Log
The dashboard prompts the user:
*"Ready to update HubSpot for David Smith: Move deal to 'Negotiation' and add note about 10% discount?"*
User clicks "Approve". Both actions fire simultaneously.

## 5. Failure Handling
**Scenario:** There are three "David Smith"s in the CRM.
**Result:** Action 2 returns an array of 3 contacts.
**Resolution:** The orchestrator halts execution and asks the user for clarification: *"I found 3 contacts named David Smith (David at Acme, David at Globex, David at Initech). Which one should I update?"*
