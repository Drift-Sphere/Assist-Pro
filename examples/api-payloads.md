# Developer Examples: API Payloads 💻

While the core Linnect Engine is proprietary, we provide typed interfaces for developers integrating internal tools with the Assist Pro webhook system.

## 1. Triggering an AI Task via Webhook

You can trigger an Assist Pro workflow programmatically.

```json
POST /api/v1/webhook/trigger
Headers:
  Authorization: Bearer <sk_test_123>
Body:
{
  "prompt": "Analyze the attached error log and create a Jira ticket if it's a P1, otherwise summarize it in Slack.",
  "context": {
    "source_system": "AWS CloudWatch",
    "priority_score": 9,
    "log_snippet": "Error: Connection timeout at pg_connect() in worker.ts:42"
  }
}
```

## 2. Standardized Action Payload (From AI to Integrations)

When the Orchestrator executes an action, it normalizes the payload. If you build a custom integration, expect to receive JSON matching this structure:

```json
{
  "action_type": "CUSTOM_CRM_UPDATE",
  "confidence_score": 0.98,
  "requires_approval": true,
  "payload": {
    "entity_id": "cust_89231",
    "fields_to_update": {
      "status": "churned",
      "reason": "pricing"
    }
  },
  "execution_chain_id": "chain_8819_abc"
}
```

## 3. Webhook Response Example

When a task completes, Assist Pro can fire a webhook back to your systems.

```json
{
  "event": "task.completed",
  "task_id": "task_9912",
  "original_prompt": "Send welcome email to new user.",
  "status": "success",
  "actions_taken": [
    {
      "integration": "GMAIL",
      "action": "GMAIL_SEND",
      "timestamp": "2026-06-21T14:00:00Z"
    }
  ],
  "human_approved": true,
  "approver_id": "user_441"
}
```
