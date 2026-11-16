# QueChat overview

QueChat is an embeddable AI chatbot for websites and apps. It provides a sticky corner assistant whose appearance, agents, knowledge, and LLM provider are managed from a workspace portal — so product teams can ship chat without rebuilding UI for every site.

**QueChat is powered by [BluVise](https://www.bluvise.com/).**

## What you get

- Hosted chat widget (script, iframe, or SDK)
- Workspace-scoped agents, knowledge, and **Languages** (chat language vs admin UI)
- Pluggable LLM providers (OpenAI-compatible APIs)
- API keys with origin allowlists for secure embeds
- Conversation memory, analytics (including language usage), and audit logs
- Multi-tenant organizations and workspaces with role-based access

## Who it is for

- Product and engineering teams who need a branded in-product assistant
- Support and docs teams who want answers grounded in their knowledge base
- Agencies or platforms that run multiple sites under separate workspaces

## Key surfaces

| Surface | URL / path | Purpose |
|---------|------------|---------|
| Marketing home | `/` | Product landing |
| Integration docs | `/docs` | How to embed QueChat |
| Sign up | `/signup` | Create owner account + organization |
| Log in | `/login` | Email/password → workspace portal |
| Workspace portal | `/app` | Dashboard, agents, knowledge, keys, widget, etc. |
| Embed host | `/embed` | Hosted widget loaded by iframe / CDN SDK |
| CDN script | `/sdk/v1/quechat.js` | One-line website install |

## Core idea

1. Create an organization and workspace in QueChat.
2. Configure providers, agents, knowledge, languages, and widget theme.
3. Create a workspace API key (`qc_live_…`).
4. Embed once with that key — theme and knowledge stay live from Admin.

Appearance and knowledge updates do **not** require republishing your app.
