# Mobile embeds and headless API

## React Native (`@quechat/react-native`)

Uses a WebView pointed at the hosted `/embed` experience:

```jsx
import { QueChatView } from "@quechat/react-native";

export function SupportScreen() {
  return (
    <QueChatView
      apiKey="qc_live_YOUR_API_KEY"
      baseUrl="https://YOUR_QUECHAT_HOST"
      overlay
    />
  );
}
```

## Android, iOS, Flutter

Pattern:

1. Create a WebView.
2. Load `https://YOUR_QUECHAT_HOST/embed?apiKey=qc_live_…`.
3. Enable JavaScript.
4. Use a transparent background where you want the corner launcher look.
5. Allow clipboard if your WebView stack supports it.

On physical devices, `localhost` refers to the device itself. Use your machine’s LAN IP or a deployed host.

Mobile WebViews often need `allowedOrigins: ["*"]` on the API key during development.

## Headless HTTP API

Build your own UI and call QueChat directly:

```bash
curl -X POST https://YOUR_QUECHAT_HOST/api/chat \
  -H "Content-Type: application/json" \
  -H "x-api-key: qc_live_YOUR_API_KEY" \
  -d '{"messages":[{"role":"user","content":"Hello"}]}'
```

Also accepted:

- `Authorization: Bearer qc_live_…`
- Body field `apiKey`

Related public GETs:

- `/api/agents` — customer-visible agents
- `/api/widget` — appearance config for custom UIs

CORS is enabled on these public routes for browser clients. See `16-public-api-reference.md`.
