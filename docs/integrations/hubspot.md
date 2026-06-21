# Integration: HubSpot 🧡

Assist Pro acts as an autonomous sales development representative (SDR) inside your HubSpot CRM.

## 🔒 Scopes & Permissions
- `crm.objects.contacts.read` / `write`
- `crm.objects.deals.read` / `write`
- `crm.objects.companies.read` / `write`

## ⚡ Supported Actions
- `HUBSPOT_SEARCH_CONTACT`: Find leads by name, email, or company.
- `HUBSPOT_CREATE_CONTACT`: Insert scraped or inbound leads.
- `HUBSPOT_UPDATE_DEAL`: Move deals across pipeline stages.
- `HUBSPOT_ADD_NOTE`: Log call summaries or web-agent research to a contact timeline.

## ⚠️ Approval-Sensitive Actions
- `HUBSPOT_UPDATE_DEAL` and any deletion actions require explicit human approval to protect pipeline integrity.
- `HUBSPOT_CREATE_CONTACT` executes instantly.

## 🛑 Limitations
- Custom objects require explicit schema mapping and may not be fully supported by the default AI orchestrator without custom instructions.
