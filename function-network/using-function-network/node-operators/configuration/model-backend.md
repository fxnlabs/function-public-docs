# Model Backend Configuration

The `model_backend.yaml` file allows node operators to configure a specific model and point it to a different URL (backend).

## Template

```yaml
backend_provider: "custom" # "fxn", "custom", "vllm" or "ollama"
url: "http://your-backend:8082" # required for "custom"
fxn_id: "2" # can be be found here https://www.function.network/models
api_key: "your-api-key" # optional
bearer_token: "your-bearer-token" # optional
```

## Parameters

### `backend_provider`

*   **Description**: Specifies the type of backend provider.
*   **Options**: `"fxn"`, `"custom"`, `"vllm"`, `"ollama"`
*   **Type**: `string`

The only avaliable provider currently supported is "custom".

### `url`

*   **Description**: The URL of your custom backend. This is required when `backend_provider` is set to `"custom"`.
*   **Type**: `string`

### `fxn_id`

*   **Description**: The Function Network model ID to participate in
*   **Type**: `string`

The `fxn_id` for the model you want to participate in can be found [here](https://www.function.network/models)

### `api_key`

*   **Description**: Your API key for the backend service (optional).
*   **Type**: `string`

### `bearer_token`

*   **Description**: Your bearer token for authentication (optional).
*   **Type**: `string`
