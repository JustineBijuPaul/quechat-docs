# FAQ

## What is QueChat?

QueChat is an embeddable AI chatbot. You manage agents, knowledge, LLM provider, and widget theme in the portal; customer sites embed it with a workspace API key. QueChat is powered by BluVise (https://www.bluvise.com/).

## Where do I manage the product?

After `/login` or `/signup`, use the workspace portal at `/app`.

## How do I put QueChat on my website?

Create an API key in Admin → API keys, allow your site origin, then add the CDN script or iframe. See `11-embedding-overview.md` and `12-embed-cdn-iframe.md`. Full public guides: `/docs`.

## Do I need to redeploy my app when I change the theme?

No. Theme and knowledge are loaded from QueChat for the workspace tied to the API key.

## What is the difference between workspaceKey and apiKey?

`apiKey` (`qc_live_…`) is the recommended production credential with scopes and origin allowlists. `workspaceKey` / `publicKey` is a legacy public identifier (seed default `default`).

## Who can create API keys?

Admin, Owner, and Super Admin. Members (Edit) and Read-only cannot manage API keys.

## Can Members create agents or knowledge docs?

Members can typically edit/view workspace resources, but create/delete of many resources is reserved for higher roles. See `05-roles-permissions.md`.

## Can I use both manual documents and a website import?

Yes. Admin → Knowledge is one **Library** (All / Manual / Website). Chat searches both together. Add manual docs when something is missing from your site. See `07-knowledge-base.md`.

## How do languages work?

Admin → **Languages** sets supported languages, detection, romanized (Hinglish/Manglish), and script matching. Chat replies in the customer’s language and **the same script** (Latin vs native). The admin UI stays English. See `22-languages.md`.

## How does agent routing work?

Workspaces can auto-route each question to the best agent. If customer pick is enabled, visitors can pin a personality. Requests may include `agentId` and `lockAgent`.

## Where is conversation history stored?

In PostgreSQL session/turn tables when Memory settings allow it. Operators manage this under Admin → Memory. Visitors can Clear the widget to start a new live session.

## Does QueChat support multiple LLM vendors?

Yes. Admin → Providers supports OpenAI-compatible endpoints (DeepSeek, OpenAI, Groq, OpenRouter, Together, Ollama, Custom). Exactly one provider is active per workspace.
