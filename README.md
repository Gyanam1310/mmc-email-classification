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

## Deployment

**Prerequisites:** AWS EC2 (Ubuntu 22.04+), Docker, Docker Compose, a domain pointed at the instance.

```bash
# 1. Clone
git clone https://github.com/your-org/mmc-email-classification.git
cd mmc-email-classification

# 2. Configure environment
cp deployment/.env.example deployment/.env
nano deployment/.env          # fill in required values

# 3. Start
cd deployment
docker compose up -d

# 4. Open n8n
# https://your-domain.com
```

**Import workflow:** n8n UI → Workflows → Import from File → select JSON from `workflows/exported-workflows/`.

**After import:** re-attach all credentials to their nodes, then activate the workflow.

---

## Required Credentials

Configure all credentials in **n8n UI → Settings → Credentials**. Nothing is stored in this repo.

| Service | n8n Credential Type | Notes |
|---|---|---|
| Gmail | Gmail OAuth2 API | OAuth redirect: `https://your-domain.com/rest/oauth2-credential/callback` |
| Slack | Slack API | Bot scopes: `chat:write`, `chat:write.public`. Invite bot to `#red-alert` and `#general-alerts`. |
| Anthropic / Claude | Anthropic API | Recommended model: `claude-3-5-sonnet-20241022` |
| Google Gemini | Google Gemini API | Alternative to Claude |
| Ollama | Ollama API | Self-hosted. Set base URL: `http://your-ollama-host:11434` |

**Switching AI provider:** change the model node + credential inside each AI node in n8n. No prompt edits needed.

---

## Repository Structure

```
mmc-email-classification/
├── deployment/
│   ├── docker-compose.yml
│   └── .env.example
├── prompts/
│   ├── sentiment-analysis.md
│   ├── risk-detection.md
│   └── alert-summary.md
├── workflows/
│   └── exported-workflows/    ← commit workflow JSON backups here
├── .gitignore
└── README.md
```

---

## Workflow Backup

After any significant change in n8n: **⋮ menu → Download** → save to `workflows/exported-workflows/`.

Naming convention: `email-classification_v1.0_2026-06-07.json`
