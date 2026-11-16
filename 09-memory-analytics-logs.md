# Memory, analytics, and logs

## Memory (Admin → Memory)

QueChat can store conversation sessions and turns in PostgreSQL (`ChatSession` / `ChatTurn`).

### Settings

- **Store full chat transcripts** — when off, metadata/errors may remain while message text is redacted
- **Max history messages** — how much recent chat the LLM sees per request
- **Retention days** — default window used when purging older sessions

### Operator actions

- Browse / search sessions
- Open full transcripts
- Delete a session or individual turn
- Purge sessions older than N days, or wipe all memory

### Widget Clear

Visitors can press **Clear** in the chat widget to reset their live conversation and start a fresh session.

## Analytics (Admin → Analytics)

Every chat turn can contribute metrics such as:

- Volume over time
- Success vs failure rate
- Agent / provider / model breakdown
- Language usage (`byLanguage`) when detection has run
- Latency and token usage (when the provider returns it)
- Knowledge sources matched

Use Analytics to monitor health and usage for a workspace.

## Logs / audits (Admin → Logs)

Important events are written to `AuditLog`, including:

- Chat success and chat failures (error text, provider, model, latency)
- Provider / agent / knowledge create-update-delete
- Admin login success and failed attempts

Use **Failures only** to review problems quickly. Failed chat turns can be inspected with full error details.
