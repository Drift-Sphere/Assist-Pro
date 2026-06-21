# Integration: Slack 💬

Assist Pro connects to your Slack workspace to provide notifications, summaries, and automated team updates.

## 🔒 Scopes & Permissions
- `chat:write`: Send messages as the Assist Pro bot.
- `channels:read`: List public channels to map automation targets.
- `im:write`: Send direct messages.

## ⚡ Supported Actions
- `SLACK_POST_CHANNEL`: Send a formatted message to a specific channel.
- `SLACK_POST_DM`: Send a direct message to a user.
- `SLACK_LIST_CHANNELS`: Retrieve available channels for workflow planning.

## ⚠️ Approval-Sensitive Actions
- `SLACK_POST_CHANNEL` (to public/team channels) requires human approval by default to prevent AI hallucinations from broadcasting to the entire company.
- `SLACK_POST_DM` (to yourself) executes instantly.

## 🎣 Webhook Events (Incoming)
Assist Pro can listen for Slack events (e.g., app mentions) if configured via the Universal Webhook Connector, allowing users to trigger AI tasks directly from Slack chat.
