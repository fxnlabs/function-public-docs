# OpenAI Compatible API

Function is backwards compatible with OpenAI (OAI), allowing you to seamlessly leverage our backend and existing SDKs without any new dependencies.

The following API services are OAI compliant:

- Chat Completion: `https://api.function.network/v1/chat/completions`
- Embeddings: `https://api.function.network/v1/embeddings`
- Image Generation: `https://api.function.network/v1/images/generation`

## Python Example

```python
import os
import openai

client = openai.OpenAI(
  api_key=os.environ.get("YOUR_API_KEY"),
  base_url="https://api.function.network/v1",
)
```
