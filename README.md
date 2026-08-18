# User Guide

The[ Verbatim AI](https://www.verbatim-ai.com) Chatbot Widget is a drop-in AI assistant you can embed on any website with a single `<script>` tag. It connects to the Verbatim AI API, answers questions from your document corpora, and can show the source documents behind each answer (with page previews).
This guide covers installation, configuration, and every available option. 

[Verbatim AI](https://www.verbatim-ai.com/)  /  [Docs](https://verbatim-ai.gitbook.io/docs)

---

## 1. Before you begin

You need:

- **An access token** *(required)*. Every request the widget makes is authenticated with a Verbatim **access token**, sent as the `X-Access-Token` header. Generate one from your backend using the Verbatim API (`POST /v1/auth/access-token`, [check documentation here](https://www.verbatim-ai.com/api-docs/)) or ask your Verbatim administrator. Tokens are short-lived and scoped to your organization.
- **One or more corpus IDs** *(required)*. A *corpus* is a collection of documents the assistant is allowed to answer from. You must provide at least one corpus ID for the session to use.
- **Access to your site's HTML**, so you can add a container element and a script tag.

> **Both `accessToken` and a non-empty `corpusIds` array are mandatory.** If either is missing, `mountChatbotWidget` logs an error to the console and throws instead of mounting — see [Troubleshooting](#17-troubleshooting).

> **Security note:** the access token is visible in the page. Use short-lived, org-scoped access tokens — never a long-lived master secret or a raw JWT.

---

## 2. Quick start

Add a container element where the widget should mount, load the script from the Verbatim CDN, then call `mountChatbotWidget`:

```html
<!-- 1. A container element for the widget -->
<div id="verbatim-chatbot"></div>

<!-- 2. Load the widget from the Verbatim CDN -->
<script src="https://cdn.verbatim-ai.com/widget/chatbot/v1/chatbot-widget.iife.js"></script>

<!-- 3. Mount it -->
<script>
  ChatbotWidget.mountChatbotWidget("#verbatim-chatbot", {
    accessToken: "YOUR_ACCESS_TOKEN",
    corpusIds: ["YOUR_CORPUS_ID"]
  });
</script>
```

That's the minimum. **`accessToken` and `corpusIds` (at least one id) are both required**; everything else has a sensible default. If either required option is missing, `mountChatbotWidget` logs an error to the console and throws — the widget does not mount.

**Styles are included automatically.** By default the widget renders inside a Shadow DOM and injects its own CSS, so you do **not** need to add a stylesheet. (If you turn style isolation off, you must add the stylesheet yourself — see [Style isolation](#14-style-isolation).)

---
