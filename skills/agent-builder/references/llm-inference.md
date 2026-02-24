# LLM Inference — OpenRouter

Call any LLM (GPT-4o, Claude, Llama, Gemini, etc.) via OpenRouter through Sapiom.

**Gateway:** `https://openrouter.services.sapiom.ai`
**API:** OpenAI-compatible (`/v1/chat/completions`)

## Setup

```js
const { createFetch } = require("@sapiom/fetch");
const safeFetch = createFetch({
  apiKey: process.env.SAPIOM_API_KEY,

});
const OPENROUTER = "https://openrouter.services.sapiom.ai/v1";
```

## Chat Completion

```js
const res = await safeFetch(`${OPENROUTER}/chat/completions`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    model: "openai/gpt-4o-mini",
    messages: [
      { role: "system", content: "You are a helpful assistant." },
      { role: "user", content: "Summarize this article: ..." },
    ],
    max_tokens: 1000,
    temperature: 0.7,
  }),
});

const data = await res.json();
const reply = data.choices[0].message.content;
```

## Recommended Models

| Model | Cost (per 1M tokens) | Best For |
|-------|---------------------|----------|
| `openai/gpt-4o-mini` | $0.15 in / $0.60 out | Default — fast, cheap, capable |
| `openai/gpt-4o` | $5.00 in / $15.00 out | Complex reasoning |
| `google/gemini-2.0-flash-001` | $0.075 in / $0.30 out | Cheapest capable model |
| `anthropic/claude-3.5-sonnet` | $3.00 in / $15.00 out | Best writing quality |
| `meta-llama/llama-3.1-70b-instruct` | $0.88 in / $0.88 out | Open-weights, good balance |

**Default recommendation:** `openai/gpt-4o-mini` — best cost/quality for most agent tasks.

## JSON Mode

```js
body: JSON.stringify({
  model: "openai/gpt-4o-mini",
  messages: [
    { role: "system", content: "Respond with JSON only. Schema: { summary: string, sentiment: 'positive'|'negative'|'neutral' }" },
    { role: "user", content: articleText },
  ],
  response_format: { type: "json_object" },
})
```

## Pricing

Varies by model. Response includes `usage.cost` with exact USD. Budget ~$0.001-0.01 per call for `gpt-4o-mini` with typical prompts.

## Gotchas

- **No API key needed** — the gateway injects credentials; just use `safeFetch`
- **Set `max_tokens`** — avoid unexpectedly long (expensive) responses
- **Check `res.ok`** — gateway returns 402 for payment issues, OpenRouter returns standard errors for model issues
