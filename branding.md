# Branding and theming

## Content & branding

```js
{
  title: "Acme Assistant",
  imageUrl: "https://example.com/logo.png",
  imageWidth: "120px",
  greeting: "Hi! 👋 I'm the Acme assistant. How can I help?",
  greetingOutside: true
}
```

- **`title`** — the name in the header (e.g. your company or product name).
- **`imageUrl` / `imageWidth`** — your logo in the header. If `imageUrl` is omitted, a default bot icon is shown.
- **`greeting`** — the opening message. It appears as the first bot message when the widget opens.
- **`greetingOutside`** — when `true`, the greeting also floats next to the launcher **before** the visitor opens the widget, as a teaser. When `false` (default), the greeting only appears inside the opened window.

---

## Theming

### Presets

Pass a preset name to `theme`:

```js
{ theme: "futuristic" }
```

Available presets: **`boring`** (default), **`futuristic`**, **`lighty`**, **`o Canada`**.

### Preset + overrides

Pass an object to start from a preset and override specific tokens:

```js
{
  theme: {
    preset: "boring",
    tokens: {
      headerBackground: "linear-gradient(90deg, #0f172a, #1e293b)",
      openButtonBackground: "#0f172a",
      openButtonColor: "#ffffff",
      badgeBackground: "#f97316"
    }
  }
}
```

### `themeTokens`

`themeTokens` overrides tokens on top of whatever `theme` resolves to. Use it when you want a preset plus a few tweaks without nesting:

```js
{
  theme: "lighty",
  themeTokens: {
    promptBackground: "rgba(17, 24, 39, 0.92)",
    promptTextColor: "#ffffff"
  }
}
```

### Available theme tokens

Every token is a CSS value (color, gradient, shadow, or font family).

| Token | Controls |
| --- | --- |
| `fontFamily` | Base font family for the widget. |
| `surfaceBackground` | Chat window background. |
| `surfaceTextColor` | Default text color inside the window. |
| `surfaceShadow` | Window shadow (input-at-bottom layout). |
| `surfaceShadowTop` | Window shadow (input-at-top layout). |
| `headerBackground` | Header background. |
| `headerTitleColor` | Header title color. |
| `headerSubtitleColor` | Header subtitle ("Online") color. |
| `headerLogoBackground` | Header logo background. |
| `headerLogoColor` | Header logo/icon color. |
| `closeButtonColor` | Header close/expand button color. |
| `closeButtonHoverBackground` | Close button hover background. |
| `botMessageBackground` | Bot message bubble background. |
| `botMessageTextColor` | Bot message text color. |
| `botMessageIconColor` | Bot message icon color. |
| `userMessageBackground` | User message bubble background. |
| `userMessageTextColor` | User message text color. |
| `userMessageIconColor` | User message icon color. |
| `inputContainerBorderColor` | Border between messages and the input area. |
| `inputBackground` | Input field background. |
| `inputBorderColor` | Input field border. |
| `inputTextColor` | Input text color. |
| `inputPlaceholderColor` | Input placeholder color. |
| `sendButtonColor` | Send button color. |
| `promptBackground` | Quick-reply prompt background. |
| `promptTextColor` | Quick-reply prompt text color. |
| `openButtonBackground` | Launcher button background. |
| `openButtonColor` | Launcher button icon color. |
| `openButtonShadow` | Launcher button shadow. |
| `badgeBackground` | Notification badge background. |
| `badgeTextColor` | Notification badge text color. |
| `typingBackground` | "Typing…" indicator background. |
| `typingIconColor` | "Typing…" indicator icon color. |
| `typingDotColor` | "Typing…" animated dots color. |
| `scrollbarThumbColor` | Message list scrollbar color. |

---

## Positioning

The `position` object controls where the launcher and window appear. It has three **modes**.

```ts
position?: {
  mode?: 'preset' | 'coordinates' | 'trigger';

  // preset mode
  preset?:
    | 'bottom-right' | 'bottom-left'
    | 'top-right'    | 'top-left'
    | 'bottom-center'| 'top-center'
    | 'center-right' | 'center-left'
    | 'center';

  // coordinates mode
  x?: number | string; // e.g. 24 or '24px'
  y?: number | string; // e.g. 120 or '120px'

  // trigger mode (open near your own element)
  gap?: number;     // distance from trigger to window, px
  offsetX?: number; // extra horizontal offset, px
  offsetY?: number; // extra vertical offset, px
}
```

### `mode: 'preset'` (default)

Anchor the widget to a screen region. Default preset is `bottom-right`.

```js
{ position: { mode: "preset", preset: "top-left" } }
```

### `mode: 'coordinates'`

Pin the widget to fixed coordinates. Requires both `x` and `y` (otherwise it falls back to preset mode).

```js
{ position: { mode: "coordinates", x: 40, y: 140 } }
```

### `mode: 'trigger'`

Open the window next to your own trigger element. Requires `openTriggerId` (otherwise it falls back to preset mode).

```js
{
  openTriggerId: "open-chat-btn",
  position: { mode: "trigger", gap: 10, offsetY: -20 }
}
```

- **`gap`** — space between your trigger and the window.
- **`offsetX` / `offsetY`** — nudge the window from its computed position.

---

## Input position

`messageInputPosition` controls the vertical layout:

```js
{ messageInputPosition: "top" }
```

- **`'bottom'`** (default) — header on top, input on the bottom, newest messages at the bottom.
- **`'top'`** — input on top, header on the bottom, newest messages at the top.

---

## Opening the widget (trigger & badge)

By default the widget renders its own floating launcher button (with an optional notification badge).

### Use your own trigger

Set `openTriggerId` to the `id` of any element on your page. Clicking it opens the widget:

```html
<button id="open-chat-btn">Chat with us</button>
```

```js
{ openTriggerId: "open-chat-btn" }
```

> When `openTriggerId` is set, the built-in floating launcher **and** the notification badge are not rendered — your element becomes the sole entry point.

### Notification badge

```js
{ notificationBadge: true } // default
```

Shows a small badge on the built-in launcher to draw attention. It disappears once the widget is opened. (Has no effect when you use your own `openTriggerId`.)

---

## Quick-reply prompts

`chatPrompts` are suggested messages shown as clickable chips before the visitor sends their first message. Clicking one sends it immediately.

```js
{
  chatPrompts: [
    "What can you help me with?",
    "Show me the documentation",
    "How do I get started?"
  ]
}
```

The prompts hide automatically once the visitor sends a message.

---

## Page context (advanced)

`pageContext` lets you run custom logic when a visitor is on a specific URL path — for example auto-opening the widget or injecting a contextual message.

It maps a URL **pathname** to `{ timer, exec }`:

- **`timer`** — how long (in milliseconds) the visitor must be on the path before `exec` runs.
- **`exec`** — a function that receives a **context object** for reading and controlling the widget.

```js
{
  pageContext: {
    "/pricing": {
      timer: 3000, // after 3 seconds on /pricing
      exec: ({ open, messageOptions }) => {
        messageOptions.setMessages(prev => [
          ...prev,
          { content: "Questions about pricing? I can help!", sender: "bot" }
        ]);
        open.setIsOpen(true);
      }
    }
  }
}
```

### The context object

`exec` receives:

| Property | Description |
| --- | --- |
| `open.isOpen` | Whether the widget is currently open. |
| `open.setIsOpen(boolean)` | Open or close the widget. |
| `messageOptions.messages` | The current list of messages (`{ content, sender }`). |
| `messageOptions.setMessages(fn or array)` | Replace or update the message list. |
| `input.inputValue` | The current text in the input field. |
| `input.setInputValue(string)` | Set the input field text. |
| `promptsOptions.prompts` | The current quick-reply prompts. |
| `promptsOptions.setPrompts(array)` | Replace the quick-reply prompts. |
| `scrollToBottom()` | Scroll the message list to the latest message. |

---

## Source attachments

When an answer is backed by documents, the bot message shows an **"N sources"** button (N = number of source documents). This is automatic — no configuration required.

Clicking it reveals, for each source document:

- the **filename** and **size**,
- a **summary** of the document,
- a **thumbnail per referenced page** (labeled `Page 1`, `Page 2`, …).

Clicking a thumbnail opens the full-size page preview in a lightbox (the widget switches to full-page mode so the preview fills the screen, then returns to its previous size when you close it). A **Hide** button collapses the sources again.

---

## Style isolation

By default the widget mounts inside a **Shadow DOM** and injects its own styles. This keeps your site's CSS from affecting the widget and vice-versa — and means **you don't need to add a stylesheet**.

```js
// default — nothing extra required
ChatbotWidget.mountChatbotWidget("#verbatim-chatbot", { accessToken: "…" });
```

If you intentionally want the widget to render in the normal DOM (for example, to apply global overrides from your own stylesheet), disable isolation **and** add the widget stylesheet yourself:

```html
<link
  rel="stylesheet"
  href="https://cdn.verbatim-ai.com/widget/chatbot/v1/index.css"
/>
```

```js
ChatbotWidget.mountChatbotWidget(
  "#verbatim-chatbot",
  { accessToken: "…" },
  { isolateStyles: false }
);
```

---

## Sessions, cookies & privacy

- On the first request, the widget looks for a **session cookie** holding the Verbatim session id. If found, it reuses that session; if not, it creates one (`POST /v1/session/`) and stores the id in a browser **session cookie** (cleared when the browser session ends).
- The cookie is scoped to the API URL, model, and corpora, so multiple widgets with different configurations keep independent sessions.
- Message history is restored from the session when the widget opens, so returning visitors continue where they left off (within the same browser session).

