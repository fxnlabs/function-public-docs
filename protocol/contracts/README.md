## Contract Core Components

### **FUNC Token**

FUNC is the native utility token of the Function Network. It powers the ecosystem by:

- **Staking**: Required for providers and gateways to participate.
- **Rewards**: Distributed to contributors based on performance.
- **Governance (Roadmap)**: FUNC holders influence decisions about the network's evolution.

### **Providers**

Providers supply computational resources for running AI models. They:

- Stake FUNC to participate.
- Execute AI inference tasks and earn FUNC rewards.
- Support specific models by aligning with their staking and performance requirements.

### **Gateways**

Gateways enable users to interact with the network. They:

- Stake FUNC to operate.
- Route user requests to providers and ensure task execution.
- Earn FUNC for their role in connecting users to the network.

### **Models**

AI models define the computational workloads to be executed on the network. Each model:

- Specifies computational requirements like shards and compute units.
- Requires providers to stake FUNC to support them.

### **Router**

The Router acts as the central management hub, connecting providers, gateways, models, and other core components.

### **Treasury**

The Treasury manages FUNC tokens and ensures accurate reward distribution to providers and gateways.

### **Checkpoints**

Checkpoint contracts track contributions and performance over time:

- **Provider Checkpoints**: Monitor provider activity and shards contributed.
- **Gateway Checkpoints**: Track gateway usage and compute units consumed.
