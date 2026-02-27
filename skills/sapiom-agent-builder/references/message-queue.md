# Message Queue — QStash

Publish messages to HTTP endpoints with guaranteed delivery, retries, and scheduling.

**Gateway:** `https://qstash.upstash.services.sapiom.ai`

## Setup

```js
const { createFetch } = require("@sapiom/fetch");
const safeFetch = createFetch({
  apiKey: process.env.SAPIOM_API_KEY,

});
const QSTASH = "https://qstash.upstash.services.sapiom.ai";
```

## Publish a Message

```js
const res = await safeFetch(`${QSTASH}/v2/publish/https://my-webhook.example.com/handler`, {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Upstash-Retries": "3",
    "Upstash-Delay": "30s",
  },
  body: JSON.stringify({ event: "scrape_complete", data: { url: "https://example.com" } }),
});

const data = await res.json();
console.log(`Message ID: ${data.messageId}`);
```

## Useful Headers

| Header | Example | Description |
|--------|---------|-------------|
| `Upstash-Retries` | `"3"` | Number of retry attempts |
| `Upstash-Delay` | `"60s"` | Delay before first delivery |
| `Upstash-Timeout` | `"30s"` | Delivery timeout |

## Pricing

~$0.00001 per message. Retries are free.

## Gotchas

- **Destination is in the URL path** — `POST /v2/publish/{destination-url}`
- **Messages go to HTTP endpoints** — the target must be publicly reachable
- **Not needed for simple cron** — Blaxel handles cron scheduling directly; use QStash only for webhook fan-out or delayed tasks
