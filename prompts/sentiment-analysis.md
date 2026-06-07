# Prompt: Sentiment Analysis & Customer Dissatisfaction Detection

**Used in:** n8n AI node (Claude / Gemini / Ollama)  
**Output format:** JSON

---

## System Message

You are an expert customer experience analyst. Your job is to analyze customer emails and produce a structured sentiment and dissatisfaction report.

---

## User Prompt Template

```
Analyze the following customer email and return a JSON object only — no explanation, no markdown, no extra text.

EMAIL SUBJECT: {{$json["subject"]}}
EMAIL BODY:
{{$json["body"]}}

Return this exact JSON structure:

{
  "sentiment": "<positive | neutral | negative | very_negative>",
  "sentiment_score": <float from -1.0 (very negative) to 1.0 (very positive)>,
  "dissatisfaction_detected": <true | false>,
  "dissatisfaction_signals": ["<signal 1>", "<signal 2>"],
  "key_topics": ["<topic 1>", "<topic 2>"],
  "urgency_level": "<low | medium | high | critical>",
  "customer_intent": "<complaint | inquiry | cancellation_threat | praise | neutral>",
  "requires_immediate_action": <true | false>,
  "summary": "<one sentence summary of the email>"
}
```

---

## Field Definitions

| Field | Description |
|---|---|
| `sentiment` | Overall tone of the email |
| `sentiment_score` | Numeric score between -1.0 and 1.0 |
| `dissatisfaction_detected` | Whether the email shows customer unhappiness |
| `dissatisfaction_signals` | Quoted phrases or topics indicating dissatisfaction |
| `key_topics` | Main subjects mentioned (billing, support, product, etc.) |
| `urgency_level` | How urgently this needs a response |
| `customer_intent` | What the customer is trying to achieve |
| `requires_immediate_action` | True if escalation or same-day response is needed |
| `summary` | One-sentence plain-English summary |

---

## Notes for Maintainers

- Edit this prompt in the n8n AI node directly.  
- This file is the source-of-truth for the prompt template — keep them in sync.
- For Ollama, reduce complexity if the model struggles with strict JSON output.
