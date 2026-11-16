# Agents

Agents are workspace personalities that shape how QueChat replies (tone, focus, greeting, accent, visibility, language).

## Admin → Agents

Configure:

- Name, tagline, description, avatar, accent
- Personality engine (tone, empathy, formality) plus freeform instructions
- Greeting, fallback, unknown-answer behavior
- **Language:** `auto` (match the customer) or a fixed code (`en`, `hi`, `ml`, …). Latin script can still be Hindi/Malayalam — see [22-languages.md](./22-languages.md)
- Knowledge (RAG) on/off, citations, allowed tools, memory
- Visibility to customers (when picker is enabled)
- Default agent, **Allow customer pick**, **Auto-route agents**
- Version history, duplicate, preview

## How routing works

By default, QueChat can **auto-route** questions to the best agent for the turn (keywords + LLM classifier).

Customers may pin an agent when the workspace allows customer pick. Chat requests can send:

- `agentId` — preferred agent
- `lockAgent` — keep that agent for the session

## Relationship to knowledge, languages, and providers

- Agents guide style, tools, and whether RAG runs (`ragEnabled`).
- **Citations** in chat API responses follow `citationsEnabled` (application-generated from retrieved chunks).
- **Languages** (workspace tab) plus the agent language field control reply language and script.
- Knowledge retrieval grounds facts in the **one library** (manual + website).
- The active LLM provider generates the final reply.

Changing agents in Admin updates live chat for embeds that use that workspace’s API key — no client redeploy required.

If **GET /api/agents** returns 500 on Vercel, production Postgres is usually missing the latest Agent migration. Run `npx prisma migrate deploy` with `DIRECT_URL`. See [22-languages.md](./22-languages.md) and [21-troubleshooting.md](./21-troubleshooting.md).
