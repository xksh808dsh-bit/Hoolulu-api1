# Vector Storage — Upstash Vector

Store and search embeddings for semantic similarity. Useful for RAG, content deduplication, and recommendation.

**Gateway:** `https://vector.upstash.services.sapiom.ai`

## Setup

```js
const { createFetch } = require("@sapiom/fetch");
const safeFetch = createFetch({
  apiKey: process.env.SAPIOM_API_KEY,

});
const VECTOR = "https://vector.upstash.services.sapiom.ai";
```

## Upsert Vectors

```js
await safeFetch(`${VECTOR}/upsert`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify([
    {
      id: "article-1",
      data: "The quick brown fox jumps over the lazy dog",
      metadata: { source: "example.com", date: "2026-02-23" },
    },
  ]),
});
```

Using `data` (string) lets Upstash auto-embed the text. Alternatively, pass `vector` (float array) for pre-computed embeddings.

## Query (Semantic Search)

```js
const res = await safeFetch(`${VECTOR}/query`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    data: "latest AI news",
    topK: 5,
    includeMetadata: true,
    includeData: true,
  }),
});

const results = await res.json();
// [{ id, score, metadata, data }, ...]
```

## Pricing

~$0.000004 per request. Storage: $0.25/GB/month.

## Gotchas

- **Use `data` for auto-embedding** — no need to generate embeddings yourself
- **`topK` max is 1000** — paginate for larger result sets
- **Metadata is filterable** — use it to scope searches by source, date, etc.
