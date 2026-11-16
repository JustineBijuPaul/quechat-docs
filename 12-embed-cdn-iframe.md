# CDN script and iframe embed

## CDN script (recommended for most websites)

```html
<script
  src="https://YOUR_QUECHAT_HOST/sdk/v1/quechat.js"
  data-api-key="qc_live_YOUR_API_KEY"
  data-base-url="https://YOUR_QUECHAT_HOST"
  async
></script>
```

Optional attributes / init options commonly include:

- `data-api-key` / `apiKey`
- `data-base-url` / `baseUrl`
- `data-position` / `position` (for example `bottom-right`)
- `data-workspace-key` / `workspaceKey` (legacy)

Programmatic init:

```js
QueChat.init({
  apiKey: "qc_live_YOUR_API_KEY",
  baseUrl: "https://YOUR_QUECHAT_HOST",
  position: "bottom-right",
});
```

The script injects the hosted widget. Theme and knowledge come from the workspace tied to the API key.

## iframe

```html
<iframe
  src="https://YOUR_QUECHAT_HOST/embed?apiKey=qc_live_YOUR_API_KEY"
  style="position:fixed;bottom:0;right:0;width:420px;height:720px;border:0;z-index:9999;background:transparent"
  allow="clipboard-write"
  title="QueChat"
></iframe>
```

Legacy:

```
/embed?workspaceKey=default
```

## Checklist

1. Key has `chat` scope (and `agents` if the picker is used).
2. Browser origin is allowed on the key (or `*` during local testing).
3. `baseUrl` / iframe `src` host matches your QueChat deployment.
4. Widget theme looks correct in Admin → Widget live preview.

Public integration docs also live at `/docs` on the running app.
