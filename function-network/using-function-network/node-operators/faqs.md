# FAQs

### Is the Function Node just a proxy?

No. While the current `"custom"` backend provider allows the node to act as a proxy to an existing model endpoint, this is just an interim solution to provide maximum flexibility. The long-term vision is a native, high-performance inference engine built for distributed inference.

This engine will use techniques like pipeline parallelism, sharding, and a custom network transport to distribute the load of a single model across many nodes. This will allow the network to run massive models that no single operator could run on their own. You can read more about this in the [Model Backend Configuration](./configuration/model-backend.md#the-future-custom-inference-engine) documentation.

### How are rewards calculated?

Rewards are calculated based on a combination of your node's uptime and its tokens-per-second performance.

### How is performance quantified?

The network has an on-chain entity known as the "scheduler" that enforces quality of service. The scheduler performs various checks to ensure your node is running healthily, including health checks, matrix multiplication challenges, and response quality checks. It also monitors for proper resource allocation. If a node's resources are shared or improperly allocated, the scheduler may jail the node and slash its stake.

### How many models can a single node support?

A single node should be dedicated to serving one model. Sharing a single node's resources across multiple models is not supported and can lead to poor performance. This prevents proper resource allocation, and the scheduler may jail and slash your stake as a result.

### Is the node software open source?

Yes, the Function Network node is fully open source. You can find the source code and contribute on our [GitHub repository](https://github.com/fxnlabs/function-node).
