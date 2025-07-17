# Model Backend Configuration

The `model_backend.yaml` file allows node operators to configure a specific model and point it to a different URL (backend).

## Template

```yaml
backendProvider: "custom" # "fxn", "custom", "vllm" or "ollama"
url: "http://your-backend:8082" # required for "custom"
fxnId: "2" # can be be found here https://www.function.network/models
apiKey: "your-api-key" # optional
bearerToken: "your-bearer-token" # optional
modelNameAlias: "your-model-name-alias" # optional
```

## Parameters

### `backendProvider`

*   **Description**: Specifies the type of backend provider.
*   **Options**: `"fxn"`, `"custom"`, `"vllm"`, `"ollama"`
*   **Type**: `string`

While `"custom"` is the primary supported provider for now, we are actively developing a native Function Network inference engine. See "The Future: Custom Inference Engine" below for more details.

### `url`

*   **Description**: The URL of your custom backend. This is required when `backendProvider` is set to `"custom"`.
*   **Type**: `string`

### `fxnId`

*   **Description**: The Function Network model ID to participate in
*   **Type**: `string`

The `fxnId` for the model you want to participate in can be found [here](https://www.function.network/models)

### `apiKey`

*   **Description**: Your API key for the backend service (optional).
*   **Type**: `string`

### `bearerToken`

*   **Description**: Your bearer token for authentication (optional).
*   **Type**: `string`

### `modelNameAlias`

*   **Description**: Your backend may reference the model name differently in OpenAI requests in comparsion to what's onchain. So this allows you to specify the mapping between Function's model name and your backend model name via an alias.
*   **Type**: `string`

## The Future: Custom Inference Engine

While the current `"custom"` backend provider allows the node to act as a proxy to any existing model endpoint, this is an interim solution. We are actively developing a native, high-performance inference engine that will become the standard for the Function Network.

This upcoming engine is being built from the ground up for true, distributed inference. It will feature advanced optimizations such as:

*   **Pipeline Parallelism**: Processing different stages of inference simultaneously across multiple nodes.
*   **Sharding**: Splitting a model into smaller pieces (shards or layers) that are distributed across the network.
*   **Custom Network Transport**: A highly optimized network layer to reduce unnecessary data transfer between nodes, ensuring the most efficient communication for distributed inference.

Eventually, this means a single node will only need to hold and compute one shard of a model, rather than the entire model. This distributed approach will significantly lower the hardware barrier for node operators and enable the network to run much larger and more powerful models collectively.

This custom engine is a core part of our roadmap and will become the default inference backend as it becomes available. In the meantime, the `"custom"` provider offers the flexibility to connect to any inference engine you choose to run.
