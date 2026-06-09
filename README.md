# MMC Email Classification & Customer Risk Detection

AI-powered n8n pipeline that monitors Gmail, scores customer risk, and fires Slack alerts.

---

## Architecture

```
Gmail (unread, last 24h)
        │
        ▼
  Schedule Trigger (hourly)
        │
        ▼
  Gmail — Fetch Emails
        │
        ▼
  Split + Normalize
        │
        ▼
  Sentiment Analysis  ◄── AI (Claude / Gemini / Ollama)
        │
        ▼
  Risk Detection      ◄── AI (Claude / Gemini / Ollama)
        │
        ▼
  Route by Risk Score
        │
        ├── score < 61   → skip
        ├── score 61–80  → Slack #general-alerts
        └── score > 80   → Alert Summary AI → Slack #red-alert
```

All logic lives in the n8n UI. Prompts are in `/prompts`.

---