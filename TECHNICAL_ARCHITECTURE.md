# Assist Pro: Technical Architecture Overview 🏗️

This document outlines the high-level technical architecture, stack, and design patterns that power the Assist Pro engine. It is designed to provide engineering teams and technical evaluators with a clear understanding of our scalable, secure, and intelligent infrastructure.

---

## 💻 Tech Stack

Assist Pro is built on a modern, high-performance web stack:

- **Framework**: Next.js 14 (App Router)
- **Language**: TypeScript (Strict Mode)
- **Database**: PostgreSQL (with `pgvector` for semantic search)
- **ORM**: Prisma
- **Authentication**: Clerk (with Enterprise SSO capabilities)
- **Billing & Subscriptions**: Paddle (Merchant of Record)
- **UI/Styling**: Tailwind CSS + Shadcn UI + Radix UI Primitives
- **Deployment**: Vercel (Edge Functions & Serverless API Routes)

---

## 🧠 AI Orchestration & The "Linnect Engine"

The core of Assist Pro is its proprietary orchestration engine, which routes intents, manages memory, and safely executes commands across third-party APIs.

### 1. Multi-Model Routing
Assist Pro does not rely on a single LLM. Instead, it dynamically routes tasks based on required latency, reasoning complexity, and cost:
- **Groq (Llama-3)**: Handles immediate, low-level intent parsing and simple data extraction where millisecond latency is critical.
- **GPT-4o & Claude 3.5 Sonnet**: Executed for complex logic, multi-step planning, and high-fidelity drafting (Digital Twin).

### 2. RAG Active Memory
- **Vector Database**: Utilizes `pgvector` directly within PostgreSQL to store interaction histories, user preferences, and uploaded contextual documents.
- **Embeddings**: Uses Google Gemini Embeddings to map semantic meaning.
- **Context Injection**: Before the AI acts, relevant memory vectors are retrieved and injected into the system prompt, giving the AI "long-term memory" of past user interactions.

### 3. Digital Twin Style-Learning
- **Profile Generation**: Analyzes historical user communications (emails, Slack messages, documents).
- **Tone Mapping**: Extracts linguistic markers, syntax habits, and formatting preferences into a `UserStyleProfile` schema.
- **Drafting**: Reapplies this style map during content generation, ensuring AI-drafted responses sound indistinguishable from the human user.

---

## 🌐 Autonomous Browser Web Agent

For tasks that lack a direct API (e.g., general web research, finding lead information on unstructured sites), Assist Pro deploys a headless browser agent.
- **Technology**: Built on [Stagehand](https://github.com/browserbase/stagehand), allowing the AI to navigate the DOM natively.
- **Capabilities**: Can click, type, extract data, and synthesize information from public websites autonomously based on a user-defined goal.

---

## 🔌 Integration Architecture & Security

Assist Pro integrates deeply with 20+ SaaS platforms (Google Workspace, Slack, Notion, HubSpot, Zoom, Calendly, Trello, ClickUp, Zoho, Pipedrive, Twilio/WhatsApp).

### Security-First OAuth & Scopes
- **Minimal Privilege**: We strictly adhere to the principle of least privilege. For example, our Google Sheets integration uses the highly restricted `drive.file` scope. We can only read and write to spreadsheets that the user explicitly selects via the Google Picker, or spreadsheets the AI creates itself. 
- **Local Resource Tracking**: Every authorized external resource (e.g., a specific spreadsheet or Notion database ID) is logged in our local `AuthorizedResource` table. If a resource isn't registered, the AI cannot access it.
- **AES-256 Encryption**: All OAuth tokens, API keys, and sensitive webhook data are encrypted at rest.

---

## ⚙️ Proactive Cron Engine

Assist Pro operates proactively in the background, not just reactively to user prompts.
- **Digest Agent**: Compiles daily/weekly summaries of inbox activity, CRM updates, and scheduled events.
- **Opportunity Spotter**: Scans linked accounts for dormant leads, high-priority unread messages, or scheduling conflicts, alerting the user and suggesting AI-drafted responses.

---

## 🛡️ "Human-in-the-Loop" Verification

To prevent AI hallucinations from causing business damage, the architecture enforces a strict boundary between *Drafting* and *Execution*.
- **Approval Gate**: High-risk actions (sending emails, modifying CRM records, posting public Slack messages) halt execution and generate a "Pending Task".
- **Execution Strategy**: The task is only executed when the user explicitly clicks "Approve" via the UI, converting the queued payload into an actual API POST request.

---

*This document is a high-level summary. Proprietary routing algorithms, specific prompt architectures, and internal encryption schemas are kept strictly confidential.*
