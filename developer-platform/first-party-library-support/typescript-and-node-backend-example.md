# Typescript & Node Backend Example

See our full reference implementation [here](https://github.com/fxnlabs/function-nodets-api-demo).

### Step 1: Add the required registry

```sh
npm config set @buf:registry https://buf.build/gen/npm/v1/
```

### Step 2: Add required dependencies

<pre class="language-sh"><code class="lang-sh"><strong>npm i @connectrpc/connect
</strong><strong>npm i @connectrpc/connect-node
</strong><strong>npm i @buf/fxnlabs_api-gateway.connectrpc_es
</strong></code></pre>

### Step 3: Create the API Client

```typescript
import {createPromiseClient, Interceptor} from "@connectrpc/connect";
import { createConnectTransport } from "@connectrpc/connect-node";
import {
    APIGatewayService
} from "@buf/fxnlabs_api-gateway.connectrpc_es/apigateway/v1/apigateway_connect.js";

function createAPIClient() {
    // Adds API Key to each request
    const apiKeyInterceptor: Interceptor = (next) => async (req) => {
        req.header.set("x-api-key", process.env.FXN_API_KEY!)
        return await next(req);
    };

    // Creates the transport
    const transport = createConnectTransport({
        httpVersion: "1.1",
        baseUrl: "api.function.network",
        interceptors: [apiKeyInterceptor]
    });

    // Creates the client
    return createPromiseClient(APIGatewayService, transport);
}
```

### Step 4: Call our API services

<pre class="language-typescript"><code class="lang-typescript">// Init the API Client
const apiClient = createAPIClient()

// Use API Client
<strong>const response = await apiClient.embed({
</strong>    model: "bge-small-en-v1.5",
    input: "Embed me so I can use it for RAG Pipelines",
})

// Log responses
console.log(response.object)
console.log(response.data)
</code></pre>
