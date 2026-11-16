# Architecture

QueChat is a Next.js application with PostgreSQL (via Prisma). Public chat and embeds are separated from the authenticated workspace portal.

## Stack

- **App:** Next.js (App Router) + React
- **Database:** PostgreSQL + Prisma ORM
- **Auth:** Session cookies (`SESSION_SECRET`)
- **Chat:** OpenAI-compatible `/chat/completions` against the active workspace LLM provider
- **SDKs:** Packages under `sdk/` plus CDN loader at `/sdk/v1/quechat.js`

## Tenancy model

```
Platform (Super Admin)
  └── Organizations
        └── Workspaces
              ├── Agents
              ├── Languages (localization)
              ├── Knowledge docs + website crawl
              ├── LLM providers
              ├── Memory (sessions / turns)
              ├── Widget theme
              ├── API keys
              ├── Analytics
              └── Audit logs
```

Admin API calls that touch tenant resources send context headers:

- `x-organization-id`
- `x-workspace-id`

Public chat resolves the workspace from a **workspace API key** (preferred) or legacy `workspaceKey` / `publicKey` (seed default: `default`).

## Request paths

| Kind | Examples |
|------|----------|
| Public UI | `/`, `/docs`, `/login`, `/signup`, `/invite/[token]`, `/embed` |
| Portal | `/app` (query: `tab`, `org`, `ws`) |
| Public APIs | `POST /api/chat`, `GET /api/agents`, `GET /api/widget`, `GET /api/languages` |
| Operator APIs | `/api/auth/*`, `/api/organizations*`, `/api/workspaces*`, `/api/workspace/languages`, `/api/api-keys*`, `/api/providers*`, `/api/agents*`, `/api/knowledge*`, `/api/memory*`, `/api/analytics*`, `/api/audits*`, `/api/widget`, `/api/sdk` |

## Important code areas

- `prisma/schema.prisma` — data model
- `lib/permissions.ts` — RBAC
- `lib/knowledge.ts` — retrieval for chat grounding
- `lib/language/` — detection, script, resolver, locale, i18n catalogs
- `lib/widget-theme.ts` — widget appearance
- `lib/cors.ts` — CORS for public APIs
- `sdk/` — web, React, React Native packages
- `components/ChatWidget.tsx` — hosted widget UI
- `components/BrandLockup.tsx` — “QueChat powered by BluVise”

## Branding

Product branding is **QueChat powered by BluVise** ([bluvise.com](https://www.bluvise.com/)). The widget may optionally show product branding via widget settings (`showBranding`).
