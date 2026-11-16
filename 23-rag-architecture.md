# QueChat RAG Architecture

QueChat has **one** Knowledge/RAG system per workspace. Manual Markdown and website imports share the same library and retrieval path. There is no second RAG engine and no external vector database (no pgvector / Pinecone / Qdrant / Weaviate / Elasticsearch).

## Status legend

- **IMPLEMENTED** — present and verified in code
- **PARTIALLY IMPLEMENTED** — present with known limits
- **NOT IMPLEMENTED** — intentionally out of scope for now
- **FUTURE** — possible later without replacing the single RAG path

## Pipeline (IMPLEMENTED)

```text
                         QUECHAT
                            │
                ┌───────────┴───────────┐
                │                       │
             RAG RUNTIME           RAG EVALUATION
                │                       │
                ↓                       ↓
          Knowledge Sources       Evaluation Dataset
                │                       │
        ┌───────┴────────┐              ↓
        ↓                ↓          Evaluation Case
     Manual           Website            │
        │                │               ↓
        └───────┬────────┘          Same Production RAG
                ↓                        │
        Extract / Normalize              ↓
                ↓                 Retrieval Metrics
             Chunking                    │
                ↓                        ↓
           Embeddings             Deterministic gen metrics
                ↓                        │
        PostgreSQL Knowledge             ↓
                │                  Optional LLM Judge
                │                        │
                ↓                        ↓
          Workspace Scope          Evaluation Result
                │
                ↓
          RAG Gate (workspace + agent)
                │
                ↓
 Language / query normalization
                │
                ↓
 Query construction
        ┌───────┴───────┐
        ↓               ↓
    Keyword          Semantic
        └───────┬───────┘
                ↓
               RRF
                ↓
          Diversity/MMR
                ↓
          Neighbor expansion
                ↓
          Context assembly
                ↓
          Untrusted DATA prompt
                ↓
               LLM
                ↓
            Answer + citation gate + diagnostics
```

## Feature checklist

| # | Feature | Status |
|---|---------|--------|
| 1 | Single RAG system | IMPLEMENTED |
| 2 | Knowledge ingestion | IMPLEMENTED |
| 3 | Manual knowledge | IMPLEMENTED |
| 4 | Website knowledge | IMPLEMENTED |
| 5 | Heading-aware chunking | IMPLEMENTED |
| 6 | Embeddings (OpenAI-compatible) | IMPLEMENTED |
| 7 | PostgreSQL `KnowledgeDoc` / `KnowledgeChunk` | IMPLEMENTED |
| 8 | Workspace isolation | IMPLEMENTED (+ integration tests) |
| 9 | RAG gates (`KnowledgeConfig.enabled`, `agent.ragEnabled`) | IMPLEMENTED |
| 10 | Query construction | IMPLEMENTED |
| 11 | Multilingual query normalization | IMPLEMENTED (`normalizeRetrievalQuery`; no language filter) |
| 12 | Keyword / IDF / fuzzy retrieval | IMPLEMENTED |
| 13 | Semantic / cosine retrieval | IMPLEMENTED |
| 14 | RRF (k≈60) | IMPLEMENTED |
| 15 | Diversity / MMR (λ≈0.72) | IMPLEMENTED |
| 16 | Neighbor expansion | IMPLEMENTED |
| 17 | Context assembly / `maxContextChars` | IMPLEMENTED |
| 18 | Strict mode | IMPLEMENTED |
| 19 | Fallback reply | IMPLEMENTED |
| 20 | Prompt grounding (untrusted DATA) | IMPLEMENTED |
| 21 | Application-generated citations + `citationsEnabled` | IMPLEMENTED |
| 22 | Retrieval diagnostics (`ChatTurn.retrievalJson`) | IMPLEMENTED |
| 23 | Agent preview RAG | IMPLEMENTED (`POST /api/agents/preview`) |
| 24 | Workspace embedding model | IMPLEMENTED (precedence + Option C mismatch skip) |
| 25 | Retrieval scalability | PARTIALLY IMPLEMENTED (chunk cache, cheap keyword candidates, semantic caps; still app-side Postgres JSON) |
| 26 | RAG evaluation datasets / runs | IMPLEMENTED |
| 27 | Retrieval metrics (Hit@K, Recall@K, Precision@K, MRR, nDCG@K, source match) | IMPLEMENTED |
| 28 | Deterministic generation evaluation | IMPLEMENTED |
| 29 | Optional LLM-as-judge | IMPLEMENTED (eval-only; never customer chat) |
| 30 | Security (SSRF, robots, workspace scoping, judge injection resistance) | IMPLEMENTED |
| 31 | Testing | IMPLEMENTED (unit + DB isolation + scale soft budgets) |

## Embedding model precedence (IMPLEMENTED)

1. `Workspace.embeddingModel` (if non-empty)
2. `QUECHAT_EMBEDDING_MODEL`
3. Provider default (`text-embedding-3-small` for OpenAI-kind; else active provider model)

**Model change policy (Option C):** chunk rows store `embeddingModel`. Retrieval only compares embeddings compatible with the active model. Legacy empty `embeddingModel` remains usable. Changing the workspace model does **not** wipe vectors; reindex to refresh.

Judge model override (eval only): `QUECHAT_EVAL_JUDGE_MODEL` (falls back to active chat model).

## Multilingual RAG (IMPLEMENTED)

- Detection/romanization live in `lib/language/*`.
- Retrieval uses `normalizeRetrievalQuery` → original + normalized + safe gloss/alternate forms.
- Knowledge is **not** duplicated per language and **not** filtered by language.
- Answer language remains a separate chat/language-policy concern.

## Evaluation (IMPLEMENTED)

Production chat:

`User → RAG → Answer` (no judge)

Evaluation:

`Case → production RAG → metrics → optional LLM judge → result`

Deterministic and model-based scores are stored separately. Optional overall judge score = mean of available judge dimensions (documented in run summary); it is **not** blended with Hit@K.

## Scalability notes (PARTIALLY IMPLEMENTED)

- Per-workspace chunk cache with fingerprint + invalidation on knowledge/config/embedding-model changes.
- Cheap keyword candidate prefilter for large pools (falls back to full pool if empty).
- Semantic candidate capping on very large corpora while preferring keyword hits.
- Soft latency tests for 1k / 10k in-memory pools.
- **FUTURE:** ANN / pgvector-style indexes only if measured need exceeds app-side optimization.

## Key files

- `lib/knowledge.ts` — config, load/cache, context, prompt
- `lib/knowledge/retrieve.ts` — hybrid ranking
- `lib/knowledge/query-normalize.ts` — multilingual query normalization
- `lib/knowledge/embeddings.ts` / `embedding-model.ts` — embeddings
- `lib/knowledge/chunk-cache.ts` — retrieval cache
- `lib/knowledge/eval.ts` / `eval-metrics.ts` / `eval-judge.ts` — evaluation
- `app/api/chat/route.ts` — production gates
- `app/api/agents/preview/route.ts` — admin preview RAG
