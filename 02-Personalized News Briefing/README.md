# 📰 Personalized News Briefing Service — n8n Automation

An automated workflow built with **n8n** that fetches daily news, filters it using **AI (Groq LLaMA)**, and delivers relevant articles to **Google Sheets** and **Gmail** every morning.

---

## 🚀 What It Does

- Runs automatically every morning via a **Schedule Trigger**
- Fetches latest articles from **2 news sources** (NewsAPI + GNews)
- Uses **Groq LLaMA AI** to classify each article as relevant or not
- Saves relevant articles to **Google Sheets**
- Sends a **daily email notification** when the sheet is updated

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **n8n** | Workflow automation platform |
| **NewsAPI** | First news source |
| **GNews API** | Second news source |
| **Groq (LLaMA 3.3 70B)** | AI filtering of articles |
| **Google Sheets** | Storage of relevant articles |
| **Gmail** | Daily email notification |

---

## 📋 Workflow Structure

```
Schedule Trigger (7 AM daily)
        ↓
HTTP Request (NewsAPI) + HTTP Request (GNews)
        ↓
      Merge Node
        ↓
  Code in JavaScript (extract articles)
        ↓
  Loop Over Items (batch size: 1)
     ↙          ↘
   done          loop
     ↓             ↓
  Aggregate      Wait (2 sec)
     ↓             ↓
   Gmail        Groq AI (YES/NO)
                   ↓
                 IF Node
                ↙      ↘
              true     false
                ↓         ↓
             Set Node   Loop input
                ↓
           Google Sheets → Loop input
```

---

## ⚙️ Setup Instructions

### 1. Prerequisites
- n8n instance (cloud or self-hosted)
- NewsAPI account → [newsapi.org](https://newsapi.org)
- GNews account → [gnews.io](https://gnews.io)
- Groq account → [console.groq.com](https://console.groq.com)
- Google account (for Sheets + Gmail)

### 2. API Keys Required
| Service | Where to Get |
|---|---|
| NewsAPI | [newsapi.org](https://newsapi.org) — free tier |
| GNews | [gnews.io](https://gnews.io) — free tier |
| Groq | [console.groq.com](https://console.groq.com) — free |

### 3. Google Sheet Setup
Create a Google Sheet named **"Daily News Briefing"** with these headers in Row 1:
```
| Title | URL | Source | Published At |
```

### 4. Node Configuration

#### HTTP Request (NewsAPI)
- **URL:** `https://newsapi.org/v2/everything`
- **Params:** `q=technology Pakistan`, `apiKey=YOUR_KEY`, `pageSize=10`

#### HTTP Request (GNews)
- **URL:** `https://gnews.io/api/v4/search`
- **Params:** `q=technology Pakistan`, `token=YOUR_KEY`, `max=10`

#### Groq HTTP Request
- **URL:** `https://api.groq.com/openai/v1/chat/completions`
- **Method:** POST
- **Header:** `Authorization: Bearer YOUR_GROQ_KEY`
- **Body:**
```json
{
  "model": "llama-3.3-70b-versatile",
  "messages": [{
    "role": "user",
    "content": "You are a news classifier. Reply YES if the article is about AI, Technology, Pakistan economy, startups, or digital innovation. Reply NO for sports, military, or celebrity news. Title: {{$json.title}} Description: {{$json.description}} Reply ONE word: YES or NO"
  }]
}
```

#### IF Node Condition
```
{{ $json.choices[0].message.content.trim().toUpperCase() }} contains YES
```

#### Google Sheets Mapping (via Set Node)
| Column | Expression |
|---|---|
| Title | `{{ $('Loop Over Items').item.json.title }}` |
| URL | `{{ $('Loop Over Items').item.json.url }}` |
| Source | `{{ $('Loop Over Items').item.json.source }}` |
| Published At | `{{ $('Loop Over Items').item.json.publishedAt }}` |

---

## 📊 Sample Output

| Title | URL | Source | Published At |
|---|---|---|---|
| Pakistan to provide AI services | https://... | The Nation | 2026-02-15 |
| iPhones to be made in Pakistan | https://... | Times of India | 2026-02-19 |
| Pakistan IT competency test | https://... | The Register | 2026-02-06 |

---

## 🔧 Troubleshooting

| Issue | Fix |
|---|---|
| Groq rate limit error | Increase Wait node to 5 seconds |
| All articles going to false | Check expression mode is ON in Groq body |
| Duplicate rows in sheet | Remove extra feedback connections to Loop |
| Multiple emails | Add Aggregate node before Gmail on done branch |

---

## 📄 License
MIT License — free to use and modify.
