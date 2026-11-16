# Workspace API keys

API keys authenticate public embeds and headless API calls for a specific workspace.

## Admin → API keys

Capabilities:

- Generate keys (`qc_live_…`)
- Rotate (new secret; old invalidated)
- Enable / disable
- Delete
- Configure scopes, allowed origins, optional rate limits / expiry

The raw secret is shown **once** on create or rotate. QueChat stores a hash, not the plaintext secret.

## Scopes

Common scopes:

- `chat` — call `POST /api/chat`
- `agents` — call `GET /api/agents`

(Additional scopes may exist in types/UI as the product evolves.)

## Allowed origins

- Exact browser origins (recommended for production), or
- `*` for open development / some mobile WebView cases

Origin allowlists protect browser installs. Headless server calls still require a valid key.

## How to send the key

Accepted on public APIs:

- Header `x-api-key: qc_live_…`
- Header `Authorization: Bearer qc_live_…`
- Body or query `apiKey` (depending on endpoint)

## Who can manage keys

Create / rotate / delete is limited to **Admin**, **Owner**, and **Super Admin**. Members and Read-only cannot manage API keys.

## Security checklist

1. One key per environment (dev / staging / prod) when possible
2. Restrict `allowedOrigins` in production
3. Rotate any key pasted into chat, tickets, or screenshots
4. Prefer HTTPS hosts for `baseUrl` / embed URLs
5. Never commit keys to git
