# Local development and Docker

## Docker (app only)

Docker runs the QueChat web container. Postgres is external (e.g. Supabase).

1. Copy env:

```bash
cp .env.example .env.docker
# set DATABASE_URL (transaction pooler :6543?pgbouncer=true&sslmode=require)
# set DIRECT_URL (session pooler :5432?sslmode=require)
# set DEEPSEEK_API_KEY (optional) and SUPER_ADMIN_PASSWORD
```

2. Start:

```bash
docker compose up --build -d
```

3. Open:

- App: http://localhost:3000
- Signup: http://localhost:3000/signup
- Login: http://localhost:3000/login → portal `/app`

Stop:

```bash
docker compose down
```

## Local app (no Docker)

1. Configure `.env` / `.env.local` from `.env.example` with an external `DATABASE_URL`.

2. Install and run:

```bash
npm install
npx prisma migrate deploy
npx prisma db seed
npm run dev
```

## Important environment variables

| Variable | Purpose |
|----------|---------|
| `DATABASE_URL` | App Postgres URL (Supabase transaction pooler `:6543?pgbouncer=true&sslmode=require`) |
| `DIRECT_URL` | Prisma migrate URL (Supabase session pooler `:5432?sslmode=require`) |
| `APP_URL` | Public app URL |
| `SESSION_SECRET` | Session signing secret |
| `SUPER_ADMIN_EMAIL` | Platform admin email |
| `SUPER_ADMIN_PASSWORD` | Platform admin password |
| `DEMO_PASSWORD` | Seeded demo user password |
| `DEEPSEEK_API_KEY` | Optional; seeds an active DeepSeek provider |
| `RESEND_API_KEY` | Optional invite email delivery |
| `CREDENTIALS_ENCRYPTION_KEY` | Encrypts connector secrets |
| `CRON_SECRET` | Protects `POST /api/cron/actions` (workflows + crawl follow-ups) |
| `GOOGLE_AUTH_CLIENT_ID` | Google login/register OAuth client (falls back to `GOOGLE_OAUTH_CLIENT_ID`) |
| `GOOGLE_AUTH_CLIENT_SECRET` | Google login/register OAuth secret |

## Seeded demo users

Default password is typically `quechat-admin`:

| Role | Email |
|------|-------|
| Super Admin | `admin@quechat.dev` |
| Owner | `owner@quechat.dev` |
| Admin | `org-admin@quechat.dev` |
| Member | `member@quechat.dev` |
| Read-only | `readonly@quechat.dev` |

Seed also creates a default organization and workspace (`publicKey=default`).

## npm scripts (common)

- `npm run dev` — local Next.js
- `npm run build` / `npm start` — production build uses **Webpack** (`next build --webpack`) because Turbopack needs native SWC binaries that slim Docker images often lack
- `vercel-build` — Vercel: `prisma generate`, `prisma migrate deploy` (needs `DATABASE_URL`; set `DIRECT_URL` for reliable migrate), then webpack build
- `npm run db:migrate` / `db:seed` / `db:studio` / `db:deploy` — Prisma workflows
- `npm run docker:up` / `docker:down` — compose helpers
- `npm run sdk:publish` — publish `@quechat/*` versions that are not yet on npm (needs `NPM_TOKEN`)

Docker **entrypoint** runs `prisma migrate deploy` (retries on pooler P1001) then starts the Next standalone server.

## Vercel

Set both `DATABASE_URL` (transaction pooler) and `DIRECT_URL` (session `:5432`). Missing `DIRECT_URL` used to fail `vercel-build`; the script now falls back to `DATABASE_URL` and continues the Next build if migrate fails. Agents/Languages features still need a successful migrate.
