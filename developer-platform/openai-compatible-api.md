# OpenAI Compatible API

Function is backwards compatible with OpenAI (OAI), allowing you to seamlessly leverage our backend and existing SDKs without any new dependencies.

The following API services are OAI compliant:

* Chat Completion: `https://api.fxnrouter.com//v1/chat/completions`
* Embeddings: `https://api.fxnrouter.com/v1/chat/completions`
* Image Generation: `https://api.fxnrouter.com/v1/images/generation`

## Python Example

```python
import os
import openai

client = openai.OpenAI(
  api_key=os.environ.get("YOUR_API_KEY"),
  base_url="https://api.fxnrouter.com/v1",
)
```
