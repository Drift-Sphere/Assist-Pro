# Assist Pro: Architecture 🏗️

This document details the layered architecture that makes Assist Pro intelligent, safe, and highly integrated.

---

## 1. Orchestration Layer (The "Linnect Engine")
The orchestrator is the central brain of Assist Pro. When a prompt, webhook, or cron job triggers the system, the orchestrator:
1. **Parses Intent**: Identifies the goal, required tools, and required constraints.
2. **Plans the Chain**: Breaks complex requests ("Find leads and email them") into a sequence of atomic actions.
3. **Validates Permissions**: Ensures the user has the required connected integrations and scopes to perform the planned actions.

## 2. Multi-Model Routing Layer
We do not rely on a single monolithic LLM. Task complexity dictates model selection:
- **Groq (Llama-3)**: Handles intent parsing, data extraction, and fast, deterministic classifications in milliseconds.
- **GPT-4o & Claude 3.5 Sonnet**: Used for complex reasoning, multi-step web browsing logic, and high-fidelity drafting (Digital Twin emulation).

## 3. Memory Layer (RAG & Knowledge Graph)
- **Vector Storage**: We use PostgreSQL with `pgvector`.
- **Embeddings**: Google Gemini Embeddings.
- **Context Injection**: Before any action is taken, the system queries the memory layer. If you refer to "that project proposal from last week", the memory layer pulls the relevant vector and injects it into the prompt context so the AI understands exactly what you mean.

## 4. Tool Execution & Integration Layer
Assist Pro communicates with external APIs via secure, typed executor functions. 
- **Atomic Actions**: Every integration has specific actions (e.g., `GMAIL_SEND`, `SLACK_POST`, `NOTION_CREATE_PAGE`).
- **Data Standardization**: The execution layer normalizes outputs from various APIs into a standard JSON format that the AI can easily read to inform its next step.

## 5. Security Boundaries & Authorization
- **Local Resource Tracking**: If the AI creates a Google Sheet or you manually pick one, it is logged in our `AuthorizedResource` table. **The AI cannot read or write to any resource ID not present in this local table**, even if the OAuth token technically has permission.
- **Scope Minimization**: We request `drive.file` instead of `drive.readonly` or full `drive`.

## 6. The "Human-in-the-Loop" Approval Model
Safety is paramount. The system categorizes actions into two buckets:
1. **Safe Actions** (e.g., Read my calendar, draft a note, summarize a thread). These execute instantly.
2. **High-Risk Actions** (e.g., Send an email, delete a contact, execute a financial transaction, post to a public Slack channel). 

**How Approval Works:**
When the orchestrator plans a high-risk action, it halts execution. It serializes the exact API payload it *intends* to send and places it in a "Pending" queue. The user reviews the drafted email or CRM payload in the dashboard. Upon clicking "Approve", the orchestrator simply fires the pre-calculated payload. The AI cannot "hallucinate" an approval bypass.
