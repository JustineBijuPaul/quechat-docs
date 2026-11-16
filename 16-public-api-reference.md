# Public API reference

These endpoints are for embeds and headless clients. Authenticate with a workspace API key (preferred) or legacy workspace key where supported.

## `POST /api/chat`

Send a chat turn.

### Auth

- `x-api-key: qc_live_…`
- `Authorization: Bearer qc_live_…`
- or body/query `apiKey`

### Typical JSON body

```json
{
  "messages": [
    { "role": "user", "content": "Hello" }
  ],
  "agentId": "optional-agent-id",
  "lockAgent": false,
  "sessionId": "optional-session-id",
  "source": "optional-source-label",
  "timeZone": "Asia/Kolkata",
  "preferredLanguage": "hi",
  "browserLanguage": "en-IN",
  "workspaceKey": "legacy-optional"
}
```

### Behavior

- Uses the workspace’s active LLM provider
- May retrieve knowledge chunks (manual + website) and answer in the customer’s language
- May auto-route or honor pinned agents
- May persist session/turns and language metadata per Memory settings
- Writes success/failure audits and analytics fields

CORS + `OPTIONS` preflight are supported for browser clients.

## `GET /api/agents`

Lists customer-visible agents for the workspace (picker / routing UI).

Requires an API key with the appropriate `agents` scope (when scopes are enforced).

## `GET /api/widget`

Returns appearance/config for the hosted widget. Public responses may include `languages` for the widget picker.

## `GET /api/languages`

Language registry for pickers (`?q=` searches English and native names).

## `POST /api/language/detect`

Detect language + script for a string (admin session or chat-scoped API key).

## Operator (session) APIs

Authenticated portal APIs (cookie session + `x-organization-id` / `x-workspace-id`) include families such as:

- `/api/auth/*`
- `/api/organizations*`, `/api/workspaces*`
- `/api/users*`, `/api/invites*`
- `/api/api-keys*`
- `/api/providers*`, `/api/agents*`, `/api/knowledge*`
- `/api/workspace/languages`, `/api/organization/languages`
- `/api/memory*`, `/api/analytics*`, `/api/audits*`
- `/api/dashboard*`
- `/api/widget` (admin write path)
- `/api/sdk`

These are for the QueChat portal operators, not for public website visitors.

## Local example

```bash
curl -X POST http://localhost:3000/api/chat \
  -H "Content-Type: application/json" \
  -H "x-api-key: qc_live_YOUR_KEY" \
  -d '{"messages":[{"role":"user","content":"Hello"}]}'
```

(Port may differ if your compose/dev setup maps another host port.)
