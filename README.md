# User Guide

The[ Verbatim AI](https://www.verbatim-ai.com) Chatbot Widget is a drop-in AI assistant you can embed on any website with a single `<script>` tag. It connects to the Verbatim AI API, answers questions from your document corpora, and can show the source documents behind each answer (with page previews).
This guide covers installation, configuration, and every available option. 

[Verbatim AI](https://www.verbatim-ai.com/)  /  [Docs](https://verbatim-ai.gitbook.io/docs)

> 🔥 You can see the widget in action in our [Demo Website](https://chemcorp.verbatim-ai.com/) https://chemcorp.verbatim-ai.com/

---

## 1. Before you begin

You need:

- **An access token** *(required)*. Every request the widget makes is authenticated with a Verbatim **access token**, sent as the `X-Access-Token` header. See section [3. How to get an Access Token](#3-how-to-get-an-access-token-)
- **One or more corpus ready with theirs IDs** *(required)*. A *corpus* is a collection of documents the assistant is allowed to answer from. Your corpus must be init and ready before setup your widget. You must provide at least one corpus ID for the session to use. See section [4 where to get your Corpus ID?](#4-where-to-get-your-corpus-id-)
- **Access to your site's HTML**, so you can add a container element and a script tag. 

> **Both `accessToken` and a non-empty `corpusIds` array are mandatory.** If either is missing, `mountChatbotWidget` logs an error to the console and throws instead of mounting — see [Troubleshooting](troubleshooting.md).

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

**Styles are included automatically.** By default the widget renders inside a Shadow DOM and injects its own CSS, so you do **not** need to add a stylesheet. (If you turn style isolation off, you must add the stylesheet yourself — see [Options](options.md).)

---

## 3. How to get an Access Token ?

**IMPORTANT**
> Your access token required 4 scopes :  `session:read`, `session:create`,  `post:read` and `post:create`

Without these scopes, the widget will log an error in the console and display an error.

### By API

use `POST /v1/auth/access-token` 
Your query should look like for a 1-hour Access Token  

```shell
curl -X 'POST' \
  'https://api.verbatim-ai.com/v1/auth/access-token' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer MY_JWT_TOKEN' \
  -H 'Content-Type: application/json' \
  -d '{
  "ttl": 3600,
  "issuer": "widget-frontend",
  "scope": [
    "session:read",
    "session:create",
    "post:read",
    "post:create" 
  ]
}'
```

### Using your back office

_not ready yet, coming soon_

## 4. Where to get your Corpus ID ?

### By API

use `GET /v1/corpus/` to list your corpus. 

Your query should look like,

```shell
curl -X 'GET' \
  'https://api.verbatim-ai.com/v1/corpus/?pageSize=25&pageIndex=0' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer MY_JWT_TOKEN'
```

The ID is the body of each item

```json
{
  "orgId": "YOUR_ORG_ID",
  "pageIndex": 0,
  "items": [
    {
      "id": "YOUR_CORPUS_ID",
      "createdAt": "2026-04-23T04:06:51Z",
      "updatedAt": "2026-04-23T04:06:51Z",
      "name": "Support knowledge base",
      "description": "Tickets, FAQs and runbooks used by the support team.",
      "metadata": {
        "owner": "support-team",
        "language": "fr"
      }
    }
  ]
}
```

### Using your back office

Open your Corpus page, select your corpus. On the top of the page

<img src="assets/corpus_header.png">

> Click on the chips under the title of your corpus.

> The corpus id is copied in your clipboard. You just have to paste it in your configuration file.

