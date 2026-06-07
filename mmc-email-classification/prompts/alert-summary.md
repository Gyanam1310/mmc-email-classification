# Prompt: Alert Summary Generation

**Used in:** n8n AI node (Claude / Gemini / Ollama)  
**Output format:** JSON (Slack Block Kit compatible)  
**Depends on:** Output of risk-detection node

---

## System Message

You are a concise business communication assistant. Your job is to generate clear, actionable Slack alert messages for the customer risk management team.

---

## User Prompt Template

```
Generate a Slack alert message for the following high-risk customer email. Return a JSON object only — no explanation, no markdown wrapper, no extra text.

EMAIL SUBJECT: {{$json["subject"]}}
SENDER: {{$json["sender"]}}
RECEIVED: {{$json["date"]}}

RISK ANALYSIS:
{{$node["Risk Detection"].json | json}}

Return this exact JSON structure:

{
  "slack_message": {
    "text": "<fallback plain text for notifications>",
    "blocks": [
      {
        "type": "header",
        "text": {
          "type": "plain_text",
          "text": "🚨 RED ALERT — High Risk Customer Email"
        }
      },
      {
        "type": "section",
        "fields": [
          { "type": "mrkdwn", "text": "*From:*\n<sender email>" },
          { "type": "mrkdwn", "text": "*Risk Score:*\n<score>/100" },
          { "type": "mrkdwn", "text": "*Risk Level:*\n<level>" },
          { "type": "mrkdwn", "text": "*Priority:*\n<P1/P2/P3>" },
          { "type": "mrkdwn", "text": "*Churn Risk:*\n<low/medium/high>" },
          { "type": "mrkdwn", "text": "*Response SLA:*\n<time>" }
        ]
      },
      {
        "type": "section",
        "text": {
          "type": "mrkdwn",
          "text": "*Subject:* <email subject>\n*Summary:* <one sentence summary>"
        }
      },
      {
        "type": "section",
        "text": {
          "type": "mrkdwn",
          "text": "*Risk Factors:*\n• <factor 1>\n• <factor 2>"
        }
      },
      {
        "type": "section",
        "text": {
          "type": "mrkdwn",
          "text": "*Recommended Action:*\n<recommended action>"
        }
      },
      {
        "type": "divider"
      }
    ]
  }
}
```

---

## Notes for Maintainers

- The `blocks` array uses [Slack Block Kit](https://api.slack.com/block-kit).  
- Adjust emoji, header text, or fields to match your team's preferences directly in the n8n AI node.  
- For the standard (non-red-alert) Slack channel, use a simpler header: `⚠️ Risk Alert`.
- The `text` fallback field is shown in Slack push notifications and must always be populated.
