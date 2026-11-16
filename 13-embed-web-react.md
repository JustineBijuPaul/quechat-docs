# Web and React embeds

Packages live under the repo `sdk/` directory (`@quechat/web`, `@quechat/react`).

## JavaScript / TypeScript (`@quechat/web`)

Mount the hosted widget:

```js
import { mountQueChat } from "@quechat/web";

const handle = mountQueChat({
  apiKey: "qc_live_YOUR_API_KEY",
  baseUrl: "https://YOUR_QUECHAT_HOST",
  position: "bottom-right",
});

// Later:
// handle.destroy();
```

Headless client (custom UI):

```js
import { QueChatClient } from "@quechat/web";

const client = new QueChatClient({
  apiKey: "qc_live_YOUR_API_KEY",
  baseUrl: "https://YOUR_QUECHAT_HOST",
});

const result = await client.chat({
  messages: [{ role: "user", content: "How do I embed QueChat?" }],
});

console.log(result.content ?? result.reply);
```

## React (`@quechat/react`)

```jsx
import { QueChatWidget } from "@quechat/react";

export function App() {
  return (
    <QueChatWidget
      apiKey={process.env.NEXT_PUBLIC_QUECHAT_API_KEY}
      baseUrl="https://YOUR_QUECHAT_HOST"
      position="bottom-right"
    />
  );
}
```

### Next.js tip

Render the widget from a client provider so it does not run unexpectedly on the server:

```tsx
"use client";

import { QueChatWidget } from "@quechat/react";

export function Providers({ children }: { children: React.ReactNode }) {
  return (
    <>
      {children}
      <QueChatWidget
        apiKey={process.env.NEXT_PUBLIC_QUECHAT_API_KEY!}
        baseUrl="https://YOUR_QUECHAT_HOST"
      />
    </>
  );
}
```

Use a public env var only for **publishable** workspace keys that are already origin-restricted. Never expose Super Admin passwords or provider secrets to the browser.
