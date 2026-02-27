# Text Search — Upstash Search

Full-text search engine for indexing and querying documents by keyword.

**Gateway:** `https://search.upstash.services.sapiom.ai`

## Setup

```js
const { createFetch } = require("@sapiom/fetch");
const safeFetch = createFetch({
  apiKey: process.env.SAPIOM_API_KEY,

});
const SEARCH = "https://search.upstash.services.sapiom.ai";
```

## Index a Document

```js
await safeFetch(`${SEARCH}/documents`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    id: "doc-1",
    content: "Full article text goes here...",
    metadata: { source: "example.com", category: "tech" },
  }),
});
```

## Search Documents

```js
const res = await safeFetch(`${SEARCH}/search`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ query: "AI developments", limit: 10 }),
});

const data = await res.json();
// data.results = [{ id, content, score, metadata }, ...]
```

## Pricing

~$0.00005 per request. Document storage: $0.10 per 1K documents/month.

## Gotchas

- **Use for keyword search** — for semantic/meaning-based search, use Upstash Vector instead
- **Index before searching** — documents must be upserted first
- **Complement with vector search** — index same content in both for hybrid search
