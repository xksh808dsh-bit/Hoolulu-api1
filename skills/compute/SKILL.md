---
name: compute
description: Deploy and run code on Sapiom — scheduled jobs, batch execution,
  sandboxes. Use when user wants to deploy, run code on a schedule, execute tasks
  in parallel, or spin up a persistent dev environment.
---

# Compute

Deploy and run code via Sapiom compute gateways.

## When to Use

- User wants to deploy code
- User wants to run something on a schedule
- User wants to execute N tasks in parallel
- User wants a persistent environment with ports/filesystem

## Compute Patterns (pick one)

| Pattern | What It Does | When to Use | Reference |
|---------|-------------|-------------|-----------|
| Scheduled job | Runs on cron | Periodic tasks (monitoring, scraping, reports) | `references/jobs.md` |
| Batch job | N parallel tasks on demand | Process a list of items in parallel | `references/jobs.md` |
| Orchestrator + workers | Scheduled job fans out to batch | Periodic task that processes N items | `references/patterns.md` |
| Sandbox | Persistent instance with ports + filesystem | Dev environments, servers, long-running processes | `references/sandboxes.md` |

## Quick Decision

- "Run X every hour" → Scheduled job
- "Process these 50 items" → Batch job
- "Every hour, check for new items, process each" → Orchestrator + workers
- "Spin up a server" / "I need a dev environment" → Sandbox

## Setup (same for all patterns)

```js
const { createFetch } = require("@sapiom/fetch");
const safeFetch = createFetch({
  apiKey: process.env.SAPIOM_API_KEY,

});
const GATEWAY = "https://blaxel.services.sapiom.ai";
```

## References

- `references/jobs.md` — Jobs API (create, deploy, execute, manage)
- `references/sandboxes.md` — Sandboxes API (create, filesystem, processes, ports)
- `references/patterns.md` — Orchestrator + workers pattern with concrete example
