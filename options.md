# Options reference

## Connecting to the API

The widget talks to two Verbatim API domains: **Session** (`/v1/session/*`) to create a chat session, and **Post**
(`/v1/post/*`) to load history, send messages, and fetch source attachments. Every request carries your
`X-Access-Token`.

```js
ChatbotWidget.mountChatbotWidget("#verbatim-chatbot", {
    accessToken: "YOUR_ACCESS_TOKEN",
    apiBaseUrl: "https://api.verbatim-ai.com", // optional; this is the default
    model: "gemma4",                           // optional
    corpusIds: ["YOUR_CORPUS_ID"],              // the documents to answer from
    lang: "en"                                  // optional
});
```

- **`accessToken`** — required. Without a valid token the API returns `401` and the widget cannot load history or send
  messages.
- **`corpusIds`** — required (at least one). The assistant answers from these corpora. Provide the IDs configured in
  your Verbatim account. An empty array is rejected at mount time.
- **`model`** — pick a different model if your organization has more than one available.
- **`lang`** — hints the answer language for each message.

---

## The mount function

```js
ChatbotWidget.mountChatbotWidget(target, options, mountOptions ?)
```

| Argument       | Type                    | Description                                                                                                                  |
|----------------|-------------------------|------------------------------------------------------------------------------------------------------------------------------|
| `target`       | `string \| HTMLElement` | A CSS selector (e.g. `"#verbatim-chatbot"`) or a DOM element to mount into.                                                  |
| `options`      | `object`                | The widget configuration — see [Options](options).                                                                           |
| `mountOptions` | `object` *(optional)*   | Mount-level settings. Currently: `{ isolateStyles?: boolean }` (default `true`). See [Style isolation](#14-style-isolation). |

Calling `mountChatbotWidget` again on the **same** target updates the widget in place with the new options (it does not
create a second instance).

---

All options live in the second argument to `mountChatbotWidget`.

### Connection

| Option        | Type       | Default                       | Purpose                                                                                                    |
|---------------|------------|-------------------------------|------------------------------------------------------------------------------------------------------------|
| `accessToken` | `string`   | — **(required)**              | Access token sent as the `X-Access-Token` header on every request.                                         |
| `corpusIds`   | `string[]` | — **(required)**              | The corpora (document collections) the assistant may answer from. Must contain **at least one** corpus id. |
| `apiBaseUrl`  | `string`   | `https://api.verbatim-ai.com` | Base URL of the Verbatim API. Override only for a staging or self-hosted deployment.                       |
| `model`       | `string`   | `gemma4`                      | The language model bound to a new chat session.                                                            |
| `lang`        | `string`   | `en`                          | ISO-639 language code sent with each message.                                                              |

### Content & branding

| Option            | Type      | Default        | Purpose                                                                                      |
|-------------------|-----------|----------------|----------------------------------------------------------------------------------------------|
| `title`           | `string`  | `AI Assistant` | Title shown in the widget header.                                                            |
| `imageUrl`        | `string`  | —              | Logo image URL for the header. Falls back to a default bot icon if omitted.                  |
| `imageWidth`      | `string`  | —              | CSS width for the logo image (e.g. `"120px"`).                                               |
| `greeting`        | `string`  | —              | A welcome message shown as the first bot bubble.                                             |
| `greetingOutside` | `boolean` | `false`        | Show the greeting as a floating bubble next to the launcher **before** the widget is opened. |

### Appearance

| Option        | Type               | Default    | Purpose                                                                     |
|---------------|--------------------|------------|-----------------------------------------------------------------------------|
| `theme`       | `string \| object` | `'boring'` | A theme preset, or a `{ preset, tokens }` object. See [Branding](branding). |
| `themeTokens` | `object`           | —          | Fine-grained color/style overrides applied on top of `theme`.               |

### Behavior & layout

| Option                 | Type                | Default                                      | Purpose                                                                                  |
|------------------------|---------------------|----------------------------------------------|------------------------------------------------------------------------------------------|
| `notificationBadge`    | `boolean`           | `true`                                       | Show a small badge on the launcher to draw attention until the widget is opened.         |
| `position`             | `object`            | `{ mode: 'preset', preset: 'bottom-right' }` | Where the launcher and window appear. See [Branding](branding).                          |
| `messageInputPosition` | `'bottom' \| 'top'` | `'bottom'`                                   | Whether the input sits at the bottom or the top of the window. See [Branding](branding). |
| `openTriggerId`        | `string`            | —                                            | The `id` of your own element that should open the widget. See [Branding](branding).      |
| `chatPrompts`          | `string[]`          | `[]`                                         | Suggested quick-reply prompts shown before the first message.                            |
| `pageContext`          | `object`            | —                                            | Run custom logic based on the visitor's URL. See See [Branding](branding).               |
