# Integration: Notion 📓

Assist Pro connects to Notion to help you build and maintain a self-organizing knowledge base.

## 🔒 Scopes & Permissions
Assist Pro requires standard Notion OAuth scopes. 
- You must explicitly grant the Assist Pro integration access to specific Pages or Databases during the OAuth flow.
- **The AI cannot read or write to any Notion page you do not explicitly share with it.**

## ⚡ Supported Actions
- `NOTION_CREATE_PAGE`: Initialize a new document or database entry.
- `NOTION_UPDATE_PAGE`: Add blocks (text, checklists, headings) to an existing page.
- `NOTION_SEARCH`: Query your shared Notion databases for context via the RAG memory layer.

## ⚠️ Approval-Sensitive Actions
- Modifying highly structured databases may require approval depending on your workspace settings, but general page creation is considered a "Safe Action."
