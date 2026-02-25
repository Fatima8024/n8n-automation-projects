# 🤖 Intelligent Customer Feedback Processor
> Built with n8n + Google Gemini AI — Fully Automated Sentiment Analysis & Routing

![Workflow](screenshots/workflow.png)

## 🚀 What It Does
This automation workflow processes customer feedback in real time using AI — no manual work needed.

| Sentiment | Action |
|-----------|--------|
| 😡 Negative | Logs to Google Sheets (Urgent Follow-up tab) |
| 🎉 Positive | Sends celebration email via Gmail to the team |
| 😐 Neutral | Logs to Google Sheets (Review tab) |

## 🛠️ Tech Stack
- **n8n** — Workflow automation platform
- **Google Gemini 1.5 Flash** — AI sentiment analysis (free)
- **Google Sheets** — Storing negative & neutral feedback
- **Gmail** — Sending celebration emails for positive feedback

## 📌 Workflow Nodes
```
n8n Form Trigger
      ↓
Basic LLM Chain (Google Gemini)
      ↓
Code Node (parse AI response)
      ↓
Switch Node
  ├── [negative] → Google Sheets (Urgent)
  ├── [positive] → Gmail
  └── [neutral]  → Google Sheets (Review)
```

## ⚙️ Setup Instructions

### Prerequisites
- n8n instance (self-hosted or n8n.cloud)
- Google account (for Sheets + Gmail)
- Google Gemini API key (free at [aistudio.google.com](https://aistudio.google.com))

### Step 1: Import the Workflow
1. Download `workflow.json` from this repo
2. Open your n8n instance
3. Click **"Add Workflow"** → **"Import from File"**
4. Select the downloaded `workflow.json`

### Step 2: Set Up Google Gemini
1. Go to [aistudio.google.com](https://aistudio.google.com)
2. Click **"Get API Key"** → Create a free key
3. In n8n, open the **Basic LLM Chain** node
4. Click **"Add Credential"** → Select **Google Gemini**
5. Paste your API key

### Step 3: Set Up Google Sheets
1. Create a new Google Sheet named **"Customer Feedback"**
2. In **Sheet1**, add headers: `Timestamp | Name | Email | Feedback | Status`
3. Add a second tab named **"Neutral Review"** with: `Timestamp | Name | Email | Feedback`
4. In n8n, connect your Google account in both Google Sheets nodes
5. Select your spreadsheet and correct sheet tab in each node

### Step 4: Set Up Gmail
1. In the Gmail node, connect your Google account
2. Update the **"To"** field with your team email address

### Step 5: Test the Workflow
1. Click **"Test Workflow"** in n8n
2. Open the form URL that appears
3. Submit feedback with different sentiments:
   - Negative: *"terrible experience, very disappointed"*
   - Positive: *"absolutely amazing, loved it!"*
   - Neutral: *"it was okay, nothing special"*
4. Verify each path works correctly

## 📂 Repository Structure
```
├── README.md
├── workflow.json        # Import this into n8n
└── screenshots/         # Workflow screenshots
```

## 🤝 Hire Me
I build custom automation workflows for businesses using n8n.

💼 **Available for freelance projects**
📧 Reach out via [LinkedIn https://www.linkedin.com/in/fatima-javed-612338244/] or 
                  [email fatimajaved8024@gmail.com]

---
⭐ Star this repo if you found it useful!

