# Integration: Slack

Assist Pro uses Slack as both an output channel for summaries and an input trigger for chat-based automation.

## Required OAuth Scopes
- `chat:write`: Send messages to channels and DMs.
- `channels:read`: Map public channel IDs.
- `im:write`: Direct message functionality.
- `app_mentions:read`: (Optional) Allows triggering Assist Pro by @-mentioning it in Slack.

## Supported Actions
- `SLACK_SEND_DM`: DM the authorizing user with alerts, digests, or pending approval requests.
- `SLACK_POST_CHANNEL_MESSAGE`: Broadcast summaries, lead updates, or alerts to a designated team channel.
- `SLACK_SUMMARIZE_THREAD`: Read a long thread and output a bulleted summary.
- `SLACK_CREATE_REMINDER`: Use Slack's native reminder system.

## What Actions Require Approval?
- **`SLACK_POST_CHANNEL_MESSAGE` requires human approval** to prevent AI from accidentally hallucinating into a public company channel.
- `SLACK_SEND_DM` (to yourself) is a "Safe Action."

## Known Limitations
- Assist Pro cannot read private channels it has not been explicitly invited to.
- It cannot read DMs between other users.

## Rate-Limit or Quota Considerations
- Slack API limits `chat.postMessage` to 1 message per second per channel. The Orchestrator will queue bursts of notifications to prevent `429` errors.
