# Troubleshooting

## Invalid API key / unauthorized chat

- Confirm the key is enabled and not expired
- Use the full `qc_live_…` secret (shown only once at create/rotate)
- Send via `x-api-key`, `Authorization: Bearer`, or supported `apiKey` field
- Ensure the key belongs to the intended workspace

## Origin blocked / CORS issues

- Add the exact browser origin to the key’s allowed origins
- For local/mobile experiments, `*` may be required temporarily
- Confirm you are calling the QueChat host that issued the key
- Preflight `OPTIONS` must succeed on `/api/chat` and `/api/agents`

## Wrong theme or agents in the embed

- The embed follows the workspace of the API key
- Open Admin with the matching org/workspace selected
- Check Admin → Widget live preview
- Hard-refresh the customer page / clear CDN caches if a loader was cached

## Blank iframe

- Verify `/embed?apiKey=…` loads directly in a browser tab
- Check iframe width/height/z-index CSS
- Ensure `allow="clipboard-write"` if clipboard features matter
- Look for mixed-content blocks (HTTPS page loading HTTP embed)

## Mobile WebView shows nothing

- Do not use `localhost` on a physical device; use LAN IP or deployed host
- Enable JavaScript in the WebView
- Allow broader origins on the key while debugging
- Confirm network access from the device to the QueChat host

## Chat answers ignore my docs

- Knowledge must be enabled in Admin → Knowledge settings
- Documents must exist in the **workspace** knowledge store (Library: All / Manual / Website)
- Website import must have **indexed** pages (not only discovered). A re-import of unchanged pages can show Skipped / 0 new search pieces while old chunks remain
- Confirm the active provider is healthy (Logs / Analytics)

## Website import stuck at 100% with Cancel import

The crawl processed pages then waited for a scheduler follow-up that often never runs on Vercel. Current builds **complete the job** when nothing is left to fetch, and **GET /api/knowledge/crawls/:id** finalizes idle jobs. Cancel is safe; existing indexed pages stay. Ensure `CRON_SECRET` and cron hitting `/api/cron/actions` if you rely on scheduled re-sync.

## GET /api/agents 500 on production

Prisma Client expects Agent personality / language columns. Apply pending migrations:

```bash
npx prisma migrate deploy
```

Use production `DATABASE_URL` + `DIRECT_URL`. Then redeploy or wait for `vercel-build` migrate.

## Docker `next build` fails (Turbopack / SWC)

Slim Debian images may lack `@next/swc-linux-x64-gnu`. The repo build script uses `next build --webpack`. Rebuild the image after pulling that change.

## Language detection feels wrong

- Latin script is not English. Check Admin → Languages (romanized + mixed language on)
- Short messages do not switch language
- Explicit “in English” overrides auto-detect
- Admin dashboard stays English even if customers chat in Hindi

## Widget logo/avatar upload fails

- Set `R2_ACCESS_KEY_ID` and `R2_SECRET_ACCESS_KEY` for the `quechat` Cloudflare R2 bucket
- Optional `R2_PUBLIC_BASE_URL` (custom domain or r2.dev). Without it, images load from `/api/media/widget/…`
- After upload, click **Save** on the Widget tab so the live embed picks up the new URLs
- Use PNG, JPEG, WebP, or GIF; files are resized to a square and compressed before upload

## Provider errors / high failure rate

- Check Admin → Logs (Failures only)
- Verify provider base URL, model id, and API key
- Ensure exactly one provider is active
- Review rate limits on the upstream vendor

## Cannot create workspace / API key

- Workspace create/delete: Owner only
- API key manage: Admin / Owner / Super Admin
- Confirm your role in `/app` and the active organization

## Invite email not arriving

- Set `RESEND_API_KEY` (and optional `EMAIL_FROM`), or
- Copy the invite URL from Admin and share it manually

## Login works but portal looks empty

- Confirm org/workspace membership
- Switch organization/workspace in the portal
- Read-only users see data but cannot edit
