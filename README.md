# ☀️ AI Daily Digest — Morning Briefing on Autopilot

> One AI-powered email. Every morning at 7 AM. Your emails, news, and calendar — all in one place.

---

## 🤔 The Problem

Every morning, you open 4 different apps:
- Gmail → scroll through 20+ emails
- Google News → skim headlines
- Google Calendar → check meetings
- Maybe a to-do list too

**25 minutes gone. Coffee cold. Brain already fried.**

## ✅ The Solution

This n8n workflow does it all for you — automatically, every single day.

One email. 7 AM. Everything you need. Nothing you don't.

---

## 📧 What The Email Looks Like

The digest email includes:

- **📧 Unread Emails** — sender name, subject, preview with color-coded avatars
- **📰 Top News** — headlines from Google News & TechCrunch, AI-summarized in one line each
- **📅 Calendar** — today's meetings with times (or a "No meetings today 🎉" celebration)
- **📊 Stats Bar** — email, news & event counts at a glance
- **💡 Daily Tip** — rotating productivity tips to start your day right
- **Smart Greeting** — says Good Morning / Afternoon / Evening based on time

---

## 🧩 Workflow Architecture

```
⏰ Schedule Trigger (7 AM daily)
    ├── 📧 Gmail (fetch unread emails)
    ├── 📰 Google News (RSS)
    ├── 🔬 TechCrunch (RSS)
    └── 📅 Google Calendar (today's events)
            │
            ▼
    🔀 Merge All Data (4 inputs)
            │
            ▼
    📦 Process Combined Data (Code node - filter & structure)
            │
            ▼
    🤖 Mistral AI (summarize news headlines only)
            │
            ▼
    📦 Build HTML Email (Code node - full email template)
            │
            ├── 📤 Gmail (send digest email)
            └── 📊 Google Sheets (log delivery)
```

**Key design decision:** AI only handles one small task — summarizing headlines. The entire HTML email is built in a Code node. This prevents the AI from leaking markdown/backticks into your email.

---

## 🛠️ Setup (5 minutes)

### Step 1: Import
1. Download `AI_Daily_Digest_CLEAN_FOR_SALE.json`
2. Open n8n → Menu (☰) → Import from File
3. Select the JSON file

### Step 2: Connect Credentials
| Service | Where to get it |
|---------|----------------|
| Gmail (OAuth2) | n8n Credentials → Add → Gmail OAuth2 |
| Google Calendar (OAuth2) | n8n Credentials → Add → Google Calendar |
| Google Sheets (OAuth2) | n8n Credentials → Add → Google Sheets |
| Mistral AI API Key | Free at [console.mistral.ai](https://console.mistral.ai) |

### Step 3: Customize
1. **Send Briefing Email node** → change `YOUR_EMAIL@gmail.com` to your email
2. **Log to Google Sheets node** → replace `YOUR_GOOGLE_SHEET_ID` with your sheet ID
3. **Calendar node** → set to `primary` or your calendar email

### Step 4: Create Google Sheet
Create a sheet with these columns in Row 1:
```
Date | Emails_Count | Events_Count | News_Count | Status | Generated_At
```

### Step 5: Activate
Toggle the workflow **ON** → wake up to your AI briefing every morning.

---

## ⚙️ Requirements

- n8n (cloud or self-hosted)
- Google account (Gmail, Calendar, Sheets)
- Mistral AI API key (free tier works perfectly)

**Running cost:** < $0.01/day (₹1/day)

---

## 📝 What I Learned Building This

A few lessons that might save you time:

1. **Don't let AI generate HTML emails** — it leaks markdown backticks. Use a Code node for the template, AI for content only.
2. **Set `numberInputs` on the Merge node** — if you're combining 4+ sources, set this explicitly or data gets lost.
3. **Gmail needs `emailType: "html"`** — without this, raw HTML shows up as text in the email.
4. **Lower AI temperature to 0.3** — at 0.7, summaries were too creative and inconsistent. 0.3 gives reliable output daily.
5. **Constrain AI to one task** — "summarize this headline in 15 words" works. "Generate a full HTML email with sections" doesn't.

---

## 🎨 Customization Ideas

- **Change news sources** — swap RSS URLs to finance, sports, or industry-specific feeds
- **Add Slack/Telegram** — send the digest to a Slack channel or Telegram bot
- **Add weather** — use OpenWeatherMap API node for local weather
- **Add Todoist/Notion** — pull your to-do list into the digest
- **Change AI model** — swap Mistral for OpenAI, Claude, or Gemini
- **Change schedule** — edit the cron expression (`0 8 * * *` for 8 AM, `0 7 * * 1` for Monday only)

---

## 🤝 Contributing

Found a bug? Have an improvement? PRs are welcome!

---

## 📄 License

MIT — free to use, modify, and share.

---

**Built with ❤️ using [n8n](https://n8n.io)**

If this helped you, consider giving it a ⭐!
