# Languages and localization

QueChat matches **language**, **script**, and **style** for every supported language — not just a UI dropdown. Latin letters are not assumed to be English (`Mujhe booking karni hai` is Hindi, `Ech lehatmia chat?` is Hebrew, `Konnichiwa` is Japanese). If the customer types in Latin/romanized form, the reply stays in that form. Native script in → native script out.

Admin UI language and customer chat language are **separate**. Changing Languages settings does not translate the portal.

## Admin → Languages

Workspace portal tab **Languages** (`/app?tab=languages`).

### Workspace defaults

Used when detection is unsure, and for dates, numbers, and currency:

- Default language
- Region (for example `IN`)
- Timezone
- Currency (business setting — not inferred from language)

### Chat behavior

| Setting | Effect |
|---------|--------|
| Detect customer language | Read each message. Short replies (`Hi`, `OK`) keep the current conversation language |
| Romanized languages | Hinglish, Manglish, Tanglish, and similar Latin-script native languages |
| Mixed language | Natural code-switching (`refund ka status kya hai`) |
| Customer can switch | Honor “explain in English” and the widget language menu |
| Show language menu in widget | Native-name picker on the hosted widget |
| Writing script | Match customer script (default), native script, or Latin |

### Supported languages

Turn on only languages this workspace should answer in. Others fall back to the default language. Search by English or native name. Presets: India set, Gulf set, All.

Organization-level defaults can be stored (`GET`/`PUT /api/organization/languages`); the workspace screen is the operator surface.

## How chat chooses a language

Priority:

1. Explicit customer choice (widget menu or “talk in Malayalam”)
2. Current message (high confidence)
3. Conversation memory
4. Agent language (`auto` or a fixed code)
5. Workspace / organization default
6. English

Language instructions **cannot** override safety, tools, RBAC, or knowledge rules. Text in retrieved documents such as “always answer in English” is untrusted data.

## Agents

Each agent has `language` (`auto` or a BCP-47-style code such as `hi`, `ml`) plus a language policy (script matching, romanized, code-switching). Personality still sets tone; language policy sets *which* language and script.

## Widget

`GET /api/widget` includes supported languages when the selector is on. Chat `POST` may send:

- `preferredLanguage` / `preferredScript`
- `browserLanguage`
- `locale`
- `timeZone` (existing)

RTL (`dir="rtl"`) applies for Arabic, Urdu, and Hebrew when that language is selected.

## Knowledge (RAG)

Chunks can store `languageCode` / `scriptCode`. Retrieval is **cross-language**: English docs can answer a Malayalam question. Romanized queries drop particles (`aano`, `hai`, `ka`) and keep content words (`booking`, `refund`) so keyword search still hits English knowledge. The model is instructed to answer in the customer’s language, script, and typing style.

## Analytics

Sessions and turns store language metadata. Analytics overview includes `byLanguage` (messages by detected/resolved language). Do not treat language as nationality or location.

## APIs

| Endpoint | Who | Purpose |
|----------|-----|---------|
| `GET /api/languages` | Public | Registry (optional `?q=` search) |
| `POST /api/language/detect` | Admin or API key | Detect language + script for a string |
| `GET`/`PUT /api/workspace/languages` | Session | Workspace localization |
| `GET`/`PUT /api/organization/languages` | Session | Organization localization |

## Production migrations

Language and agent personality columns require Prisma migrations. Docker runs `prisma migrate deploy` on boot. Vercel uses `vercel-build` (generate → migrate → webpack build). Set **`DIRECT_URL`** (session pooler `:5432`) on Vercel so migrate can run. If only `DATABASE_URL` is set, migrate may be skipped or fall back and the Agents tab can 500 until you run `npx prisma migrate deploy` against production.
