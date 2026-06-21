# Security & Trust Center 🛡️

Trust is everything when granting an AI access to your workspace. This document explains exactly how Assist Pro handles your data, ensures absolute safety, and prevents runaway automation.

---

## 🔒 1. "Human-in-the-Loop" & Approval Rules

"AI that acts inside my workspace" shouldn't be scary. Assist Pro operates on a strict **Draft → Review → Execute** model for anything sensitive.

### Can it take actions without approval?
**Yes, but only for "Safe Actions."**
- Reading your schedule
- Searching your email (if connected)
- Drafting content
- Summarizing files

### What actions ALWAYS require confirmation?
**"High-Risk Actions" are hard-coded to require a human click.**
- Sending any email (`GMAIL_SEND`)
- Modifying or deleting CRM contacts (`HUBSPOT_UPDATE`, `PIPEDRIVE_DELETE`)
- Posting messages to public Slack channels (`SLACK_POST_PUBLIC`)
- Executing code or web-agent form submissions

### What happens when an automation fails halfway?
Assist Pro uses transactional task chaining. If Step 2 of a 4-step workflow fails (e.g., an API goes down), the entire workflow halts. The error is logged to your Audit Feed with the exact failure reason, and the remaining steps are aborted. It will never send an email referencing a Notion document that failed to create.

---

## 🧠 2. Model Providers & Data Privacy

### What data is sent to model providers?
We send the minimum context required to complete a task. If you ask the AI to summarize an email, the text of that email is sent to the LLM (e.g., OpenAI or Anthropic) for processing. 

### Does my data train the models?
**Absolutely NOT.** 
We use enterprise API agreements with OpenAI, Anthropic, and Groq. Under these agreements, our users' prompt data, uploaded files, and contextual information are **strictly excluded from model training**. 

### How "Digital Twin" Learning Works
If you opt-in to the Digital Twin feature, Assist Pro analyzes your sent emails to learn your tone, syntax, and phrasing.
- The analysis extracts abstract style markers ("Uses short sentences", "Prefers formal greetings").
- The raw emails are **not** stored indefinitely. Only the abstract mathematical `UserStyleProfile` is saved.

### How long does memory persist?
RAG (Retrieval-Augmented Generation) memory persists as long as your account is active. However, you have full control:
- You can view all memory vectors in your dashboard.
- You can delete specific memories.
- You can purge your entire history at any time with one click.

---

## 🔐 3. Encryption & Infrastructure

- **Encryption at Rest**: All databases are encrypted using AES-256.
- **OAuth Token Management**: API keys and OAuth tokens are encrypted at the application level before being stored in the database.
- **Data Retention**: If you delete your account, a cascading delete immediately removes all OAuth tokens, memory vectors, user data, and activity logs. There are no "soft deletes" for sensitive credentials.

---

## 👨‍💻 4. Admin Controls & Audit Logs

### Audit Logs
Every single action taken by the AI is logged in an immutable `Task` and `ActivityFeed` table. Users can view exactly:
- What prompt triggered the action.
- What LLM planned it.
- The exact JSON payload sent to the third-party API.
- The exact JSON response received.

### Admin Toggles
Admins can globally disable specific integrations or actions at the workspace level. If an admin disables the `GMAIL_SEND` action, the orchestrator will instantly block any AI attempt to use that tool across the entire workspace.
