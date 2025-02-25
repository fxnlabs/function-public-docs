## FAQ

#### **General Questions**

1. **What is the Function Network?**

   - A decentralized system for AI inference, connecting gateways to AI providers using FUNC tokens for staking and rewards.

2. **What are FUNC tokens used for?**

   - FUNC tokens are used for staking by providers and gateways, distributed as rewards, and may be used for governance in the future.

3. **What is an epoch, and how does it work?**

   - An epoch is a time period defined by a fixed number of blocks during which network activities like staking and rewards are synchronized.

4. **How are rewards calculated?**

   - Rewards are based on the shards (units of work) contributed by providers within an epoch, distributed proportionally from the epoch’s reward pool.

5. **What is the significance of `minEpochsLocked`?**
   - This is the minimum number of epochs for which FUNC tokens must remain staked before they can be unstaked.

#### **Provider Questions**

1. **What is required to register as a provider?**

   - Register with metadata (typically the URL for the node), stake the required FUNC tokens, and ensure the provider is whitelisted.

2. **What happens if I don’t claim my rewards?**

   - Rewards remain claimable indefinitely but are subject to the epoch reward delay before they become available.

3. **What is the penalty for being jailed?**

   - Jailed providers cannot participate in staking or claim rewards during the jail period.

4. **What does the metadata of a provider represent?**
   - The metadata is typically the URL where the provider’s node can be accessed for inference requests.

#### **Gateway Questions**

1. **How do gateways interact with providers?**

   - Gateways aggregate client requests and route them to the appropriate providers based on availability and compute requirements.

2. **Can gateways unstake immediately?**

   - No, gateways must initiate a wind-down process, followed by a staking delay before they can unstake.

3. **What metadata do gateways need?**
   - Gateways typically include routing-related metadata like endpoint URLs or regional availability.
