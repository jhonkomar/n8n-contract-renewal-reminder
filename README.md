# Contract Renewal Reminder — n8n Workflow

An automated n8n workflow that detects expiring contracts in HubSpot
and notifies the Account Manager before it's too late.

## Flow
1. Runs automatically every day at 08:00
2. Fetches all deals from HubSpot
3. Skips contracts with auto-renewal or in-negotiation status
4. Filters deals expiring in exactly 90, 60, or 30 days
5. For each deal: creates a Gmail draft → sends WhatsApp notification to AM → logs a task in HubSpot

## Stack
- n8n
- HubSpot (CRM)
- Gmail (draft email)
- WhatsApp Business Cloud (AM notification)

## Setup

### Credentials
- HubSpot OAuth2
- Gmail OAuth2
- WhatsApp Business Cloud

### HubSpot Custom Properties
- `auto_renewal` (boolean)
- `deal_status` — set to `in_negotiation` to skip deals being renegotiated

### n8n Variables
- `WHATSAPP_PHONE_NUMBER_ID` — Phone Number ID from Meta Business Manager
- `AM_WHATSAPP_NUMBER` — Account Manager WhatsApp number (format: `628xxx`)

## Import
1. Open n8n → Workflows → Import
2. Upload `Contract_Renewal_Reminder.json`
3. Configure credentials and variables above
4. Activate the workflow
