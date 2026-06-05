# 🔔 Contract Renewal Reminder

An n8n workflow that automatically detects expiring contracts in HubSpot and notifies the Account Manager — before the client churns. Zero manual follow-up required.

## 📋 What It Does

| Step | Action |
|------|--------|
| 1 | Runs every day at 08:00 and fetches all deals from HubSpot |
| 2 | Skips contracts with auto-renewal or in-negotiation status |
| 3 | Calculates days remaining until each contract's end date |
| 4 | Filters deals expiring in exactly 90, 60, or 30 days |
| 5 | Creates a pre-filled renewal draft email in Gmail |
| 6 | Sends a WhatsApp notification to the Account Manager |
| 7 | Logs a renewal reminder task inside the deal in HubSpot |

If no contracts fall within the 90/60/30-day window, the workflow exits silently — no noise, no spam.

---

## 🗂️ Workflow Overview

```
Schedule Trigger (daily 08:00)
  └─► Fetch All Deals from HubSpot
        └─► Filter: skip auto-renewal & in-negotiation
              └─► Calculate Days Until Expiry
                    └─► Filter: exactly 90, 60, or 30 days?
                          └─► Loop Per Deal
                                └─► Set Urgency Label & Format Data
                                      └─► Create Gmail Draft
                                            └─► Notify Account Manager via WhatsApp
                                                  └─► Log Renewal Task in HubSpot
```

---

## 🛠️ Tech Stack

- **[n8n](https://n8n.io)** — workflow automation
- **HubSpot** — CRM / contract data source + task logging
- **Gmail** — renewal draft email
- **WhatsApp Business Cloud** — Account Manager notification

---

## ✅ Prerequisites

- n8n instance (self-hosted or cloud)
- HubSpot account with OAuth2 API access
- Google account with Gmail OAuth2 access
- WhatsApp Business Cloud account (Meta Business Manager)
- OAuth2 credentials configured in n8n

---

## ⚙️ Setup

### 1. Import the Workflow

1. Open your n8n instance
2. Go to **Workflows → Import**
3. Upload `Contract_Renewal_Reminder.json`

### 2. Configure Credentials

Go to **Credentials** in n8n and create the following:

| Credential | Type | Used In |
|------------|------|---------|
| HubSpot OAuth2 | HubSpot OAuth2 API | Fetch Active Deals, Log Renewal Task |
| Gmail OAuth2 | Gmail OAuth2 API | Create Gmail Draft |
| WhatsApp Business Cloud | WhatsApp Business Cloud API | Notify Account Manager |

Then connect each credential to its respective node inside the workflow.

### 3. Configure n8n Variables

Go to **Settings → Variables** and create the following:

| Variable | Value |
|----------|-------|
| `WHATSAPP_PHONE_NUMBER_ID` | Your Phone Number ID from Meta Business Manager |
| `AM_WHATSAPP_NUMBER` | Account Manager WhatsApp number (format: `628xxx`) |

### 4. Add HubSpot Custom Properties

Make sure the following custom properties exist on your HubSpot deals:

| Property | Type | Description |
|----------|------|-------------|
| `auto_renewal` | Boolean | Set to `true` to skip the deal |
| `deal_status` | String | Set to `in_negotiation` to skip the deal |

### 5. Activate the Workflow

Click **Activate** in the top-right corner of the n8n editor.

---

## ⚠️ Edge Cases

| Scenario | Behavior |
|----------|----------|
| Contract has auto-renewal | Filtered out — no notification sent |
| Contract is being renegotiated | Filtered out — no notification sent |
| No contracts in 90/60/30-day window | Workflow exits silently |
| Multiple contacts on one account | Notification sent per deal, not per contact |
| `closedate` field is empty | Deal is filtered out and skipped |

---

## 📁 File Structure

```
├── Contract_Renewal_Reminder.json   # n8n workflow file
└── README.md                        # This file
```

---

## 🔧 Customization Tips

- **Change trigger time:** Edit the Schedule Trigger node (default: 08:00 daily)
- **Add more reminder windows:** Add conditions to the Filter 90/60/30 Day Window node
- **Notify per contact:** Add a HubSpot Get Contacts node after the loop to fan out notifications
- **Send email directly:** Change the Gmail node from `create draft` to `send` to skip AM review

---

## 📄 License

MIT — free to use, modify, and distribute.
