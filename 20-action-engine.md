# Action Engine

QueChat’s Universal Action Engine lets agents perform real-world tasks **only** through connectors. Chat and the SDK never call external APIs directly.

## Concepts

- **Actions** — metadata catalog (input/output JSON Schema, retries, approval flags)
- **Connectors** — how actions reach external systems (MVP: Generic REST)
- **Installations + vault** — per-workspace config and AES-encrypted credentials
- **Executor** — validate → load connector/credentials → run → audit
- **Workflows** — sequential / conditional / parallel / delay / approval steps
- **Scheduler** — cron / one-shot jobs polled by `/api/cron/actions`
- **Approvals** — pause sensitive actions / workflow gates until an admin decides

## Admin

In `/app`:

1. **Connectors** — install Generic REST (base URL + optional bearer token), Test / Disable / Rotate
2. **Actions** — enable/disable catalog tools for chat; run a one-off action
3. **Workflows** — step-list builder, Run now, Schedule (cron)
4. **Approvals** — approve/reject pending requests
5. **Executions** — history + monitor cards

## Public API / SDK

API keys need the `actions` scope for `/api/engine`.

```ts
const client = new QueChatClient({
  apiKey: "qc_live_…",
  baseUrl: "https://YOUR_QUECHAT_HOST",
});

await client.executeAction("rest_get", { path: "/status" });
await client.runWorkflow("demo-rest-flow", { ticketId: "T-1" });
await client.chat({
  messages: [{ role: "user", content: "Call the status endpoint." }],
});
```

Chat tool-calling uses the same Action Engine when tools are enabled for the workspace.

### Timezone

Chat injects an authoritative clock into the system prompt:

1. Visitor `timeZone` from the widget/SDK (`Intl` IANA id), when present
2. Else workspace **Default timezone** (Actions admin; default `Asia/Kolkata`)
3. Else `Asia/Kolkata`

The model must not invent “today”; calendar tools should pass that IANA zone with start/end times.

## Environment

```
CREDENTIALS_ENCRYPTION_KEY=…   # credential AES key material
CRON_SECRET=…                  # Authorization: Bearer … for /api/cron/actions
```

Schedule a cron (e.g. Vercel Cron or docker) to `POST /api/cron/actions` every minute.

## Seed

`npm run db:seed` upserts categories, the `generic-rest` connector, and `rest_request|get|post|put|delete` actions.
