# Assist Pro System Architecture

This document breaks down the end-to-end technical architecture of Assist Pro, detailing how user intent is securely translated into autonomous actions across the 6 core layers of our system.

## 1. Input Layer
Assist Pro can ingest commands through multiple vectors:
- **Text Input**: Direct command input via the dashboard chat UI.
- **Voice Input**: Audio streams processed through OpenAI Whisper or Groq Whisper for highly accurate STT (Speech-to-Text).
- **Uploaded Docs/Context**: Users can upload PDFs, CSVs, or text files to provide temporary context for a single workflow.
- **Webhooks/Events**: Incoming triggers from scheduled cron jobs (e.g., "Daily Digest") or external webhooks (e.g., "New HubSpot Lead").

## 2. Intent & Planner Layer
The Orchestrator acts as the "brain" of the system.
- **Command Classification**: We use ultra-fast, deterministic models (Llama-3 on Groq) to instantly classify the incoming prompt. Is this a query? A multi-step action? An API failure retry?
- **Tool Selection**: The planner maps the extracted intent to available integration tools (e.g., mapping "Email David" to `GMAIL_SEND`).
- **Execution Planning**: For complex commands, a DAG (Directed Acyclic Graph) of actions is generated.
- **Safety Rules / Approval Routing**: The orchestrator checks every planned action against hardcoded safety rules. If an action is high-risk (e.g., `GMAIL_SEND`), the planner tags it with `requires_approval: true`.

## 3. Memory Layer
Our RAG (Retrieval-Augmented Generation) infrastructure ensures the AI has deep context.
- **What gets stored**: User preferences, past tool execution results, and interaction summaries.
- **Vector Memory**: Contextual data is embedded (Google Gemini Embeddings) and stored in PostgreSQL using `pgvector`.
- **Short-term vs Long-term**:
  - *Short-term (Session)*: Stored in Redis. Handles active multi-step workflows and conversational history for the current login session.
  - *Long-term*: Stored in `pgvector`. Persists across sessions.
- **Digital Twin Training Data**: When a user opts-in to style learning, we extract linguistic markers (tone, syntax) into a mathematical profile (`UserStyleProfile`). Raw emails are **not** kept for training; only the abstract profile is stored and injected into drafting prompts.

## 4. Action Layer
The executor layer handles the actual API calls via secure, typed integrations.
- **Gmail Actions**: `GMAIL_READ`, `GMAIL_DRAFT`, `GMAIL_SEND`
- **Slack Actions**: `SLACK_POST_CHANNEL`, `SLACK_POST_DM`, `SLACK_LIST_CHANNELS`
- **HubSpot Actions**: `HUBSPOT_SEARCH`, `HUBSPOT_CREATE`, `HUBSPOT_UPDATE`
- **Notion Actions**: `NOTION_CREATE_PAGE`, `NOTION_UPDATE_PAGE`
- **Browser Agent Actions**: Utilizes `Stagehand` (Playwright-based) to navigate the DOM, extract unstructured data, and submit forms on platforms lacking standard APIs.

## 5. Audit & Notification Layer
Total visibility into AI actions is critical for trust.
- **Task Logs**: Every planned and executed action is logged immutably. The log contains the exact JSON payload sent to the third-party API and the exact response received.
- **Notifications**: Users receive real-time UI toasts, Slack pings, or SMS alerts (via Twilio) when a high-risk task is pending approval or when a long-running workflow completes.
- **Failure Handling & Retries**: If an API request fails (e.g., 503 Server Error), the executor logs the failure, attempts an exponential backoff retry (up to 3 times), and if it still fails, aborts the remaining workflow steps to prevent cascading errors. Rollback rules apply to local database state (e.g., removing a pending task record).

## 6. Security Layer
- **OAuth Token Handling**: We use Clerk for primary auth. Integration OAuth tokens are encrypted at the application level before being written to PostgreSQL.
- **Encryption**: AES-256 encryption at rest for all database volumes.
- **Tenant Isolation**: PostgreSQL Row-Level Security (RLS) ensures that one workspace can never query or execute tools using another workspace's tokens.
- **Permission Scopes**: We practice strict scope minimization. For example, Google Workspace integration requests `drive.file` (only files created by the app or explicitly selected) rather than full `drive` access.
