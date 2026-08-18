## Examples

## Basic setup

## Full setup

Your code snippet

```html
<button id="open-chat-btn">Chat with us</button>
<div id="verbatim-chatbot"></div>

<script src="https://cdn.verbatim-ai.com/widget/chatbot/v1/chatbot-widget.iife.js"></script>
<script>
  ChatbotWidget.mountChatbotWidget("#verbatim-chatbot", {
    // Connection
    accessToken: "YOUR_ACCESS_TOKEN",
    corpusIds: ["YOUR_CORPUS_ID"],
    model: "gemma4",
    lang: "en",

    // Branding
    title: "Acme Assistant",
    imageUrl: "https://example.com/logo.png",
    imageWidth: "120px",
    greeting: "Hi! 👋 How can I help you today?",
    greetingOutside: true,

    // Suggested prompts
    chatPrompts: [
      "What can you help me with?",
      "How do I get started?",
      "Contact support"
    ],

    // Appearance
    theme: {
      preset: "boring",
      tokens: {
        headerBackground: "linear-gradient(90deg, #0f172a, #1e293b)",
        openButtonBackground: "#0f172a",
        badgeBackground: "#f97316"
      }
    },

    // Open near a custom button
    openTriggerId: "open-chat-btn",
    position: { mode: "trigger", gap: 10 },

    // Layout
    messageInputPosition: "bottom",

    // Contextual behavior
    pageContext: {
      "/pricing": {
        timer: 3000,
        exec: ({ open }) => open.setIsOpen(true)
      }
    }
  });
</script>
```