# API & Workflow Examples

This document outlines real JSON payloads and task logs representing how Assist Pro plans, executes, and audits complex workflows internally.

---

## 1. Single-Action Task
**Input Command:** "Create a new spreadsheet called Q3 Marketing Budget."
**Resolved Intent:** `CREATE_SHEET`
**Execution Plan:**
```json
{
  "steps": [
    { "tool": "SHEETS_CREATE", "params": { "title": "Q3 Marketing Budget" } }
  ]
}
```
**Final Output / Task Log:**
```json
{
  "status": "success",
  "action": "SHEETS_CREATE",
  "result": { "spreadsheetId": "1aB2c3D4...", "url": "https://docs.google.com/..." }
}
```

---

## 2. Multi-Step Orchestration
**Input Command:** "Find the latest email from Sarah, summarize it, and put the summary in my Slack DM."
**Resolved Intent:** `EMAIL_TRIAGE_AND_NOTIFY`
**Execution Plan:**
```json
{
  "steps": [
    { "step_id": 1, "tool": "GMAIL_SEARCH", "params": { "query": "from:sarah" } },
    { "step_id": 2, "tool": "LLM_SUMMARIZE", "depends_on": [1], "params": { "text_ref": "step_1.output.body" } },
    { "step_id": 3, "tool": "SLACK_POST_DM", "depends_on": [2], "params": { "message_ref": "step_2.output.summary" } }
  ]
}
```

---

## 3. Approval-Required Task
**Input Command:** "Email the engineering team that deployment is delayed by 2 hours."
**Resolved Intent:** `EMAIL_TEAM`
**Execution Plan:**
```json
{
  "steps": [
    { "tool": "GMAIL_DRAFT", "params": { "to": "eng@company.com", "subject": "Deployment Delay" } },
    { "tool": "GMAIL_SEND", "requires_approval": true, "params": { "draft_id_ref": "step_1.output.draft_id" } }
  ]
}
```
**Approval State:** Task halts at Step 2. Status set to `PENDING_APPROVAL`. Once human clicks "Approve", Step 2 executes.

---

## 4. Failed Task + Retry Behavior
**Input Command:** "Add 50 rows to the reporting sheet."
**Execution Execution:** Google Sheets API returns `429 Too Many Requests`.
**Failure Handling:**
```json
{
  "attempt": 1,
  "status": "failed",
  "error": "429 Rate Limit Exceeded",
  "retry_scheduled_in_ms": 2000
}
```
*(After 3 failed retries, workflow aborts, user notified via Dashboard Toast).*

---

## 5. Memory-Aware Drafting Task
**Input Command:** "Draft a proposal to Acme Corp using the standard pricing tiers we discussed last week."
**Resolved Intent:** `DRAFT_PROPOSAL`
**Execution Plan:**
```json
{
  "steps": [
    { "tool": "MEMORY_SEARCH", "params": { "query": "Acme Corp standard pricing tiers" } },
    { "tool": "NOTION_CREATE_PAGE", "params": { "content": "LLM_GENERATED_USING_MEMORY_CONTEXT" } }
  ]
}
```

---

## 6. Digital Twin Drafting Task
**Input Command:** "Reply to John and say no to his feature request."
**Resolved Intent:** `DRAFT_REPLY`
**Execution Plan:**
```json
{
  "steps": [
    { "tool": "FETCH_USER_STYLE_PROFILE", "params": { "user_id": "usr_123" } },
    { "tool": "GMAIL_DRAFT", "params": { 
        "to": "john@example.com", 
        "instructions": "Polite rejection. Tone modifiers: [Professional, Concise, Empathetic]" 
    }}
  ]
}
```

---

## 7. Scheduled/Cron Proactive Task
**Background Trigger:** Daily 8 AM Digest
**Resolved Intent:** `MORNING_DIGEST`
**Execution Plan:**
```json
{
  "steps": [
    { "tool": "CALENDAR_GET_TODAY", "params": {} },
    { "tool": "GMAIL_LIST_UNREAD_HIGH_PRIORITY", "params": {} },
    { "tool": "SLACK_POST_DM", "params": { "compiled_digest_ref": "LLM_SUMMARY_OF_1_AND_2" } }
  ]
}
```
