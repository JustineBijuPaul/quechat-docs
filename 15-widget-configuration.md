# Widget configuration

Each workspace has its own chat UI theme. Edit it in **Admin → Widget**.

## Templates

Start from a style template, then tune any field:

| Template | Look |
|----------|------|
| Classic circle | Round solid launcher with a short label |
| Brand pill | Pill with icon + label and gradient fill |
| Speech bubble | Teardrop shape, icon-only, outer ring |
| Minimal square | Soft square launcher |
| Dark launcher | Dark fill with light label |

Templates only patch appearance fields. Save to persist on the hosted embed.

## What you can customize

### Brand / copy

- Bot name, subtitle, welcome message
- Logo and avatar: drag-and-drop images (resized to 256×256 / 192×192, compressed WebP, stored on Cloudflare R2). **Delete** removes the file from R2 as well. You can still paste a URL if needed.
- Placeholder and send label
- Quick replies

### Colors & panel

- Theme mode (`light` / `dark` / `auto`)
- Primary, secondary, panel, text, muted, bubble colors
- Header style (`gradient` / `solid` / `minimal`). Gradient mode supports any number of color stops with position and opacity
- Font, border radius, panel width/height

### Launcher (freeform)

- Shape: `circle`, `pill`, `square`, `teardrop` (speech bubble)
- Size: `sm` / `md` / `lg`
- Label and optional icon (drag-and-drop, or falls back to logo)
- Show icon / show label toggles
- Icon size (12–48px), icon border width, icon border color (closed launcher)
- Fill: solid or gradient. Gradient mode uses a multi-stop editor (add/remove colors, angle, position %, opacity)
- Text color, button border width, button border color (outer ring)
- Position and offsets

### Advanced

- **Chatbot active / inactive** — turn the public widget and API off without deleting settings (Admin → Chatbot UI / UX → Public availability)
- `customCss` (injected into the hosted widget; max ~8000 chars)
- Toggles: branding, clear, header, auto-open
- **Language code** (`html lang`) plus workspace **Languages** settings for auto-detect, RTL, and the in-widget language menu (native names)

## How embeds pick up theme

Embeds load widget config for the workspace tied to `apiKey` (or legacy `workspaceKey`) via `GET /api/widget`.

Changing theme in Admin updates live embeds — no app republish.

## Export for custom client UIs

Admin → Widget → **Export for custom UIs** provides:

1. **Theme JSON** — full workspace theme object
2. **CSS variables** — `:root { --qc-… }` from the same tokens the hosted widget uses
3. **Launcher HTML** — standalone button markup matching the configured launcher

Use exports when you build a custom chat shell (or native UI) that should match brand. Hosted CDN/iframe/React SDKs still load `/embed` and do not require these snippets.

## CSS variables

The widget uses QueChat CSS custom properties including:

- Panel: `--qc-accent`, `--qc-accent-deep`, `--qc-paper`, `--qc-ink`, `--qc-muted`, `--qc-radius`, `--qc-panel-w/h`, `--qc-offset-x/y`
- Launcher: `--qc-launcher-bg`, `--qc-launcher-ink`, `--qc-launcher-border-w`, `--qc-launcher-border`, `--qc-launcher-icon-size`, `--qc-launcher-icon-border-w`, `--qc-launcher-icon-border`
- Header: `--qc-header-bg`

## Permissions

Super Admin, Owner, Admin, and Member can edit widget settings. Read-only users can view but not change them.

## Image storage (Cloudflare R2)

Uploads go to the `quechat` R2 bucket. Set:

```
R2_ENDPOINT=https://ad0a006fd2a310e97b785b5280f85ce5.r2.cloudflarestorage.com
R2_BUCKET=quechat
R2_ACCESS_KEY_ID=
R2_SECRET_ACCESS_KEY=
R2_PUBLIC_BASE_URL=https://pub-37b2a8f1d2ae45ffab5ce1344b551d8b.r2.dev
```

`R2_PUBLIC_BASE_URL` is optional (custom domain or `https://pub-….r2.dev`). If it is empty, images are served from `/api/media/widget/…`.

## SDKs tab

Admin → **SDKs** provides copy-ready install snippets that already target the hosted widget, so appearance remains Admin-controlled after install.
