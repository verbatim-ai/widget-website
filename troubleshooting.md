
# Troubleshooting

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| Widget never mounts; console shows `[ChatbotWidget] Invalid configuration` | `accessToken` is missing/empty, or `corpusIds` is missing/empty | Both are required — pass a non-empty `accessToken` and a `corpusIds` array with at least one id. |
| Widget loads but no history / messages fail | Missing or invalid `accessToken` (API returns `401`) | Provide a valid, non-expired access token. |
| `ChatbotWidget is not defined` | The script tag hasn't loaded before your `mountChatbotWidget` call | Place your inline script **after** the CDN `<script>` tag, or wait for `DOMContentLoaded`. |
| Nothing appears | The target element doesn't exist yet | Ensure the container (e.g. `#verbatim-chatbot`) is in the DOM before mounting. |
| Widget looks unstyled | Style isolation is off and the stylesheet isn't loaded | Add the `index.css` link, or remove `isolateStyles: false` to use the default Shadow DOM styling. |
| Answers ignore your documents | Wrong or missing `corpusIds` | Pass the correct corpus IDs for your account. |
| Custom trigger doesn't open the widget | `openTriggerId` doesn't match an element `id` | Make sure the `id` exists and matches exactly. |

---

*Need an access token or corpus IDs? Contact your Verbatim administrator.*
