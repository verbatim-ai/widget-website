
# Staging mode

When you use your widget in a staging environment, take care to specify the `apiBaseUrl` to **https://staging-api.verbatim-ai.com**
Your setup should look like this,

```html
<!-- 1. A container element for the widget -->
<div id="verbatim-chatbot"></div>

<!-- 2. Load the widget from the Verbatim CDN -->
<script src="https://cdn.verbatim-ai.com/widget/chatbot/v1/chatbot-widget.iife.js"></script>

<!-- 3. Mount it -->
<script>
  ChatbotWidget.mountChatbotWidget("#verbatim-chatbot", {
    apiBaseUrl: "https://staging-api.verbatim-ai.com", // You use the staging backend
    accessToken: "YOUR_ACCESS_TOKEN",
    corpusIds: ["YOUR_CORPUS_ID"]
  });
</script>
```
