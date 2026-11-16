# Knowledge base

QueChat grounds chat answers in workspace knowledge. You can combine **manual documents** and **website imports** — the assistant searches both together.

## Admin → Knowledge

Open **Admin → Knowledge**. The **Library** lists All / Manual / Website. Add content two ways:

| Tab | Use when |
|-----|----------|
| **Manual documents** | You want to write or paste Markdown (policies, internal FAQs, release notes, corrections) |
| **Import from website** | You want QueChat to crawl a public site, keep pages in sync, and cite real URLs |

Typical workflow:

1. Import your marketing or docs site for broad coverage.
2. Add manual documents for anything missing, outdated on the site, or not public.
3. Tune retrieval settings once — they apply to both sources.

## Manual documents

Operators with upload permission can:

- Create, edit, and delete manual Markdown documents
- Use one topic per document when possible
- Fill gaps after a website import (pricing exceptions, beta features, support macros)

Manual documents are **not** mixed into the website page list. Website-imported pages are managed only under **Import from website**.

## Website knowledge

Operators with website permissions can:

- Enter a root URL and discover pages (sitemap + crawl)
- Choose which pages to include or exclude
- Run an import / sync job with progress, remaining-time estimate, and history
- After a successful import, review **captured docs** (preview Markdown). If every page is already indexed, the job may show **Skipped** and **0 search pieces** for that run — existing chunks stay in the library
- Schedule automatic re-sync (daily, weekly, etc.)
- Schedule automatic re-sync (daily, weekly, etc.)

QueChat:

- Respects `robots.txt` and blocks unsafe targets (SSRF protection)
- Extracts readable text from HTML
- Chunks and indexes content for retrieval
- Updates incrementally when page content changes
- Marks removed pages (404/410) as inactive

Website pages appear as indexed chunks with **source URLs** for citations in chat.

## Retrieval settings

| Setting | Purpose |
|---------|---------|
| Enabled | Turn retrieval on/off |
| Strict | Prefer answering only from retrieved context |
| topK | How many chunks to retrieve |
| Chunk size / overlap | How manual docs are split (website chunks use the crawl pipeline) |
| Max context chars | Cap on context injected into the model |
| Fallback reply | Message when nothing relevant is found |
| Bot name | Display name used with KB replies |

These settings apply to **manual and website content together**.

## How chat uses knowledge

On each chat turn, QueChat runs RAG only when **workspace retrieval is enabled** and the **agent has RAG enabled**:

1. Builds a retrieval query from the latest user message (and recent user questions)
2. Loads **active indexed chunks** (manual documents and website pages) from Postgres
3. Hybrid retrieval (keyword + optional embeddings → RRF → diversity → neighbor expansion)
4. Injects context into the model prompt (capped by max context chars)
5. Instructs the model to answer in the **customer’s language and script**, even if the docs are in another language
6. Returns **application-generated** citations when the agent has citations enabled

An import that sits at **100% complete** with **Cancel import** still showing is a stuck job (follow-up scheduler step never ran). After deploy, polling the crawl API **finalizes** idle jobs. You can also Cancel; already-indexed pages remain. See [21-troubleshooting.md](./21-troubleshooting.md).

If nothing useful is found and strict mode is on, the assistant should avoid inventing product facts and may use the configured fallback reply.

See `23-rag-architecture.md` for the full architecture, evaluation, and gates.

## Permissions (typical)

| Action | Permission |
|--------|------------|
| View knowledge | Knowledge read / admin |
| Create manual docs | `KNOWLEDGE_UPLOAD` |
| Edit manual docs | `KNOWLEDGE_EDIT` |
| Delete manual docs | `KNOWLEDGE_DELETE` |
| Add / sync websites | Website source permissions |

Members may edit existing manual docs but not create or delete them, depending on workspace role.

## Files in this repository

The repo includes seed-style files under `knowledge/` (for example `config.json`, `agents.json`, and markdown under `knowledge/docs/`).

**Production and demo workspaces load these files via seed** (`prisma db seed` upserts every `knowledge/docs/*.md` except `README.md` into the default workspace and indexes them for RAG). Operators can still edit or add more docs in Admin → Knowledge.

## Writing good knowledge

- One topic per manual document when possible
- Use clear headings and concrete steps
- Include exact URLs, header names, and env var names
- Prefer current product behavior over aspirational features
- After a website import, add manual docs for gaps (support-only info, unreleased features, legal text)
- Update docs when Admin UX or API contracts change
