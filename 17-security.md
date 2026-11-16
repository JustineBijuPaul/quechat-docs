# Security

## Secrets

Keep out of git and public clients:

- `DATABASE_URL`
- `SESSION_SECRET`
- `SUPER_ADMIN_PASSWORD` / demo passwords
- LLM provider API keys
- Workspace API key secrets after rotation display
- `CREDENTIALS_ENCRYPTION_KEY`
- `CRON_SECRET`
- `DIRECT_URL` (migrations)
- `RESEND_API_KEY`

`.env*` files are ignored except `.env.example`.

## Workspace API keys

- Prefer origin allowlists over `*` in production
- Rotate keys that appear in screenshots, tickets, or chat
- Separate keys per environment
- Limit scopes to what the client needs (`chat`, `agents`)
- Disable unused keys instead of leaving them active

## Transport and embeds

- Serve QueChat and customer sites over HTTPS in production
- Confirm `baseUrl` / embed `src` point at your real host
- Do not place admin session cookies on public marketing pages as a substitute for API keys

## Untrusted customer and knowledge text

Customer messages and retrieved documents are **data**, not instructions. Language switches (“explain in English”) only change reply language. They cannot reveal system prompts, change tools, or bypass RBAC. Knowledge that says “always answer in English” does not override workspace language policy.

- Super Admin is unique; protect that account carefully
- Owners/Admins should only invite trusted emails
- Members should not mint API keys
- Read-only accounts are for visibility, not configuration

## Before public deploy

1. Change all default demo passwords
2. Rotate any demo/shared `qc_live_…` keys
3. Restrict allowed origins
4. Confirm only the intended LLM provider is active
5. Review Logs for failed auth and chat errors
