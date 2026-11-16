# Embedding overview

QueChat embeds load a **hosted** widget. Your site only supplies an API key and host URL; appearance, agents, and knowledge stay controlled from the QueChat portal.

## Recommended flow

1. Create/configure a workspace (providers, agents, knowledge, languages, widget theme).
2. Create a workspace API key in Admin → API keys.
3. Allow your site origin(s) on that key.
4. Install with CDN script, iframe, npm package, or mobile WebView.
5. Iterate on theme/knowledge in Admin — visitors see updates without republishing.

## Auth for embeds

Prefer:

```
/embed?apiKey=qc_live_YOUR_KEY
```

or SDK options:

```
apiKey: "qc_live_YOUR_KEY"
baseUrl: "https://YOUR_QUECHAT_HOST"
```

Legacy still works:

```
/embed?workspaceKey=default
```

Prefer API keys for production.

## Integration choices

| Method | Best for |
|--------|----------|
| CDN script (`/sdk/v1/quechat.js`) | Any static or CMS site |
| iframe `/embed` | Quick drop-in, sandboxed frame |
| `@quechat/web` | JS/TS apps, headless client |
| `@quechat/react` | React / Next.js |
| `@quechat/react-native` | React Native WebView overlay |
| Native WebView | Android / iOS / Flutter |
| Headless `POST /api/chat` | Custom UI or backend bots |

See:

- `12-embed-cdn-iframe.md`
- `13-embed-web-react.md`
- `14-embed-mobile-headless.md`
- `16-public-api-reference.md`

## Copy-ready snippets

Admin → **SDKs** also generates install snippets for the current workspace and key context.
