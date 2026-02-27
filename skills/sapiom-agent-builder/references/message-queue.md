# Message Queue — QStash

Publish messages to HTTP endpoints with guaranteed delivery, retries, and scheduling.

**Gateway:** `https://upstash.services.sapiom.ai`

## Setup

```js
const { createFetch } = require("@sapiom/fetch");
const safeFetch = createFetch({
  apiKey: process.env.SAPIOM_API_KEY,
});
const QSTASH = "https://upstash.services.sapiom.ai";
```

## Publish a Message

```js
const res = await safeFetch(`${QSTASH}/v1/qstash/publish/https://my-webhook.example.com/handler`, {
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

## Enqueue to a Queue

```js
// Enqueue — ordered delivery with a named queue
const res = await safeFetch(`${QSTASH}/v1/qstash/enqueue/my-queue/https://my-webhook.example.com/handler`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ event: "task", payload: { id: 42 } }),
});
```

## Batch Publish

```js
const res = await safeFetch(`${QSTASH}/v1/qstash/batch`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify([
    { destination: "https://my-webhook.example.com/handler-a", body: JSON.stringify({ id: 1 }) },
    { destination: "https://my-webhook.example.com/handler-b", body: JSON.stringify({ id: 2 }) },
  ]),
});
```

## Useful Headers

| Header | Example | Description |
|--------|---------|-------------|
| `Upstash-Retries` | `"3"` | Number of retry attempts |
| `Upstash-Delay` | `"60s"` | Delay before first delivery |
| `Upstash-Timeout` | `"30s"` | Delivery timeout |

## Pricing

~$0.000010 per message published. Retries are free.

## Gotchas

- **Destination is in the URL path** — `POST /v1/qstash/publish/{destination-url}`
- **Messages go to HTTP endpoints** — the target must be publicly reachable
- **Not needed for simple cron** — Blaxel handles cron scheduling directly; use QStash only for webhook fan-out or delayed tasks
- **Enqueue vs publish** — use `enqueue` for ordered processing via a named queue; use `publish` for fire-and-forget fan-out
