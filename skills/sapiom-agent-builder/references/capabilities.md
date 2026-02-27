# Gateway Capabilities

Available services your agent can use through Sapiom gateways. All requests use `@sapiom/fetch` — payment is handled automatically.

## Service Overview

| Service | Gateway URL | What It Does | Cost/Request | Best For |
|---------|------------|--------------|--------------|----------|
| **OpenRouter** | `https://openrouter.services.sapiom.ai` | LLM inference (GPT-4o, Claude, Llama, etc.) | Varies by model | Reasoning, summarization, classification |
| **Firecrawl** | `https://firecrawl.services.sapiom.ai` | Web scraping (clean markdown) | ~$0.009 | Extracting page content |
| **Linkup** | `https://linkup.services.sapiom.ai` | Web search | ~$0.02 | Finding URLs, real-time info |
| **Upstash Vector** | `https://upstash.services.sapiom.ai` (mgmt) + `https://{id}.vector.data.sapiom.ai` (instance) | Vector database (embeddings) | ~$0.000004 | Semantic search, RAG |
| **Upstash Search** | `https://upstash.services.sapiom.ai` (mgmt) + `https://{id}.search.data.sapiom.ai` (instance) | Full-text search | ~$0.00005 | Keyword search, document indexing |
| **Upstash Redis** | `https://upstash.services.sapiom.ai` (mgmt) + `https://{id}.redis.data.sapiom.ai` (instance) | Redis key-value store | ~$0.000002/cmd | Caching, rate limiting, ephemeral state |
| **QStash** | `https://upstash.services.sapiom.ai` | Message queue / scheduling | ~$0.00001 | Delayed tasks, webhooks |
| **Neon Postgres** | `https://neon.services.sapiom.ai` | On-demand PostgreSQL databases | ~$0.01 (24h) | Structured data, state across runs |
| **Deploy + Compute** | `https://blaxel.services.sapiom.ai` | Scheduled jobs, batch execution, sandboxes | ~$0.001 | Running your agent code |

## Common Patterns

| Agent Pattern | Services Needed |
|---------------|----------------|
| Monitor + Alert | Firecrawl (scrape) + OpenRouter (analyze) |
| Research + Report | Linkup (search) + Firecrawl (scrape) + OpenRouter (summarize) |
| Collect + Store | Firecrawl (scrape) + Upstash Vector (store) |
| Search + Answer | Upstash Vector (retrieve) + OpenRouter (generate) |
| Watch + Notify | Firecrawl (scrape) + QStash (webhook) |
| Collect + Persist | Firecrawl (scrape) + Neon Postgres (store) |
| Track + Report | Neon Postgres (state) + OpenRouter (analyze) |

## SDK Setup (same for all services)

```js
const { createFetch } = require("@sapiom/fetch");

const safeFetch = createFetch({
  apiKey: process.env.SAPIOM_API_KEY,

});

// Then use safeFetch() exactly like native fetch()
const res = await safeFetch("https://firecrawl.services.sapiom.ai/v1/scrape", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ url: "https://example.com" }),
});
```
