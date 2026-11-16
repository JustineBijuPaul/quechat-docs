# QueChat knowledge docs index

This folder is the product knowledge corpus for **QueChat** (powered by [BluVise](https://www.bluvise.com/)).

These markdown files document the project for humans and for knowledge-base use. On `prisma db seed`, every file here except this README is upserted into the **default** organization’s **default** workspace Knowledge base and indexed for RAG.

## Contents

| File | Topic |
|------|-------|
| [00-overview.md](./00-overview.md) | What QueChat is |
| [01-architecture.md](./01-architecture.md) | Stack and tenancy |
| [02-auth-signup-login.md](./02-auth-signup-login.md) | Signup and login |
| [03-invites-and-users.md](./03-invites-and-users.md) | Invites and user management |
| [04-organizations-workspaces.md](./04-organizations-workspaces.md) | Orgs and workspaces |
| [05-roles-permissions.md](./05-roles-permissions.md) | RBAC hierarchy |
| [06-agents.md](./06-agents.md) | Agents and routing |
| [07-knowledge-base.md](./07-knowledge-base.md) | Manual docs, website import, and retrieval |
| [08-llm-providers.md](./08-llm-providers.md) | LLM providers |
| [09-memory-analytics-logs.md](./09-memory-analytics-logs.md) | Memory, analytics, audits |
| [10-api-keys.md](./10-api-keys.md) | Workspace API keys |
| [11-embedding-overview.md](./11-embedding-overview.md) | How embedding works |
| [12-embed-cdn-iframe.md](./12-embed-cdn-iframe.md) | CDN script and iframe |
| [13-embed-web-react.md](./13-embed-web-react.md) | Web and React SDKs |
| [14-embed-mobile-headless.md](./14-embed-mobile-headless.md) | Mobile and headless API |
| [15-widget-configuration.md](./15-widget-configuration.md) | Widget theme |
| [16-public-api-reference.md](./16-public-api-reference.md) | Public API endpoints |
| [17-security.md](./17-security.md) | Security practices |
| [18-local-development.md](./18-local-development.md) | Docker and local setup |
| [19-branding-bluvise.md](./19-branding-bluvise.md) | Branding lockup |
| [20-faq.md](./20-faq.md) | Short FAQ |
| [21-troubleshooting.md](./21-troubleshooting.md) | Common fixes |
| [22-languages.md](./22-languages.md) | Multilingual chat, script matching, Languages tab |
| [23-rag-architecture.md](./23-rag-architecture.md) | RAG pipeline, multilingual query normalization, embedding model, evaluation + LLM judge, scalability |

## Related app surfaces

- Public integrator docs UI: `/docs`
- Workspace portal: `/app`
- Hosted embed: `/embed`
- CDN loader: `/sdk/v1/quechat.js`
