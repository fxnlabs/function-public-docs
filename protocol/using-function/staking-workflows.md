## Staking Workflows for Function Network

Below are detailed workflows for registering, staking, unstaking, claiming rewards, etc. for both **Gateways** and **Providers** within the Function Network.

### **Provider Staking via Dapp**

🔗 [Function Network](https://devnet.function.network/provide)

Simply access the Function Network Dapp, and connect your wallet to provide compute resources.

Follow the steps to contribute to the network. Note that the ID should be your public peerID, and the URL should be your node URL.

### **Provider Staking Workflows**

#### **1. Registering & Staking a Provider**

Providers must be linked to a specific model and registered before staking. If auto-whitelist is enabled, follow steps 1B.

##### **Steps 1A**

1. Call `register` in `ProviderRegistry`:
   - Provide a unique `id` (max 64 bytes), `modelId`, and metadata.
   - **Metadata**: The metadata field for a provider is intended to be the URL to access the provider's node, enabling connection to its services. The metadata must be in JSON format and must contain a 'url' field of the provider URL, and optionally a 'name' field.
2. Emit event: `ProviderRegistered`.

```solidity
register("unique-provider-id", 1, "{}")
```

1. Call `stakeForProvider` in `ProviderStaking` (use the router address to locate the correct staking pool for a given model):

   - Provide the registered unique `id` (max 64 bytes)

2. Emit event: `ProviderStaked`.

```solidity
stakeForProvider("unique-provider-id")
```

Providers must meet the required stake amount defined by the model.

#### **Steps 1B**

1. Call `registerAndStake` in `ProviderRegistry`:
   - Provide a unique `id` (max 64 bytes), `modelId`, and metadata.
   - **Metadata**: The metadata field for a provider is intended to be the URL to access the provider's node, enabling connection to its services. The metadata must be in JSON format and must contain a 'url' field of the provider URL, and optionally a 'name' field.
2. Emit event: `ProviderRegistered`.

```solidity
registerAndStake("unique-provider-id", 1, "{}")
```

#### **3. Unstaking a Provider**

To retrieve staked FUNC tokens, providers must ensure all lock periods and requirements are met.

##### **Steps**

1. Ensure Minimum Lock Period Elapsed:
   - Providers must meet the `minEpochsLocked` requirement.
2. Call `unstakeProvider` in `ProviderStaking`:
   - Provide the `id` of the provider to unstake FUNC.
3. FUNC Transfer:
   - The FUNC tokens are transferred back to the provider owner.
4. Emit event: `ProviderUnstaked`.

Example:

```solidity
unstakeProvider("unique-provider-id")
```

#### **4. Claiming Rewards**

Providers earn FUNC rewards based on shard contributions and participation metrics. To claim rewards for all providers owned, check out 1A, to claim rewards for a specific provider, check 1B.

##### **Steps 1A**

1. Ensure Rewards Are Delayed:
   - Rewards can only be claimed for epochs up to `currentEpoch - epochRewardDelay` as defined in `EpochController`.
2. Call `claimRewardsCluster` in `ProviderRegistry`:
   - Provide the range of epochs to claim rewards for all providers owned by the caller.
3. Emit event: `RewardClaimedForCluster`.

Example:

```solidity
claimRewardsCluster(10, 20)
```

This will attempt to claim rewards for the given epoch range for all providers, failed providers can be individually claimed with steps in step 1B.

##### **Steps 1B**

1. Ensure Rewards Are Delayed:
   - Rewards can only be claimed for epochs up to `currentEpoch - epochRewardDelay` as defined in `EpochController`.
2. Call `claimRewards` in `ProviderStaking`:
   - Provide the range of epochs to claim rewards for all providers owned by the caller.
3. Emit event: `ProviderClaimedRewards`.

Example:

```solidity
claimRewards("id", 10, 20)
```

### **Gateway Staking Workflows**

Currently Third Party Gateways are not currently enabled.

<!--
#### **1. Registering & Staking a Gateway**

Gateways must be registered before staking. If auto-whitelist is enabled, follow steps 1B.

##### **Steps 1A**

1. Call `register` in `GatewayRegistry`:
   - Provide a unique `id` (max 64 bytes), and metadata.
   - **Metadata**: The metadata field for a provider is intended to be the URL to access the provider's node, enabling connection to its services. The metadata must be in JSON format and must contain a 'url' field of the provider URL, and optionally a 'name' field.
2. Emit event: `ProviderRegistered`.

```solidity
register("unique-provider-id", 1, "{}")
```

1. Call `stakeForProvider` in `ProviderStaking` (use the router address to locate the correct staking pool for a given model):

   - Provide the registered unique `id` (max 64 bytes)

2. Emit event: `ProviderStaked`.

```solidity
stakeForProvider("unique-provider-id")
```

Providers must meet the required stake amount defined by the model.

#### **Steps 1B**

1. Call `registerAndStake` in `ProviderRegistry`:
   - Provide a unique `id` (max 64 bytes), `modelId`, and metadata.
   - **Metadata**: The metadata field for a provider is intended to be the URL to access the provider's node, enabling connection to its services. The metadata must be in JSON format and must contain a 'url' field of the provider URL, and optionally a 'name' field.
2. Emit event: `ProviderRegistered`.

```solidity
registerAndStake("unique-provider-id", 1, "{}")
```

#### **1. Registering a Gateway**

Gateways must be registered before staking FUNC tokens. Registration associates metadata and allows the Gateway to participate in the network.

##### **Steps**

1. Call `register` in `GatewayRegistry`:
   - Provide a unique `id` (max 64 bytes) and optional metadata (max 1024 bytes).
   - Metadata is intended for descriptive information about the Gateway, such as its role or identifier.
2. Auto-whitelisting (if enabled):
   - The gateway will automatically be whitelisted for participation.
3. Emit event: `GatewayRegistered`.

Example:

```solidity
register("unique-gateway-id", "My Gateway Metadata")
```

#### **2. Staking FUNC for a Gateway**

Once registered, the Gateway owner can stake FUNC tokens to participate in the network.

##### **Steps**

1. Determine Stake Amount:
   - Ensure you have enough FUNC tokens to stake.
   - Transfer FUNC tokens to the `GatewayRegistry` contract.
2. Call `registerAndStake`:
   - Provide the `id`, metadata, and amount of FUNC tokens to stake.
3. Auto-update Staking Contract:
   - The FUNC tokens are approved and forwarded to the `GatewayStaking` contract.
4. Emit event: `GatewayRegisteredAndStaked`.

Example:

```solidity
registerAndStake("unique-gateway-id", "My Gateway Metadata", 1000 * 10**18)
```

#### **3. Wind Down a Gateway**

Gateways must be wound down before they can be unstaked. This marks the gateway for eventual deactivation.

##### **Steps**

1. Call `windDownGateways` in `GatewayStaking`:
   - Provide a list of gateway IDs to wind down.
2. Wait for the wind-down delay:
   - A specified delay period (defined in `EpochController`) must pass before the gateway can be unstaked.
3. Emit event: `GatewayWoundDown`.

Example:

```solidity
windDownGateways(["unique-gateway-id"])
```

#### **4. Unstaking a Gateway**

To retrieve staked FUNC tokens, the gateway must complete the wind-down and lock periods defined by the system.

##### **Steps**

1. Ensure Wind-Down Delay is Complete:
   - Gateways must meet the wind-down delay requirement before unstaking.
2. Ensure Minimum Lock Period Elapsed:
   - Gateways must also meet the `minEpochsLocked` requirement.
3. Call `unstakeForGateway` in `GatewayStaking`:
   - Provide the `id` of the gateway to unstake FUNC.
4. FUNC Transfer:
   - The FUNC tokens are transferred back to the gateway owner.
5. Emit event: `GatewayUnstaked`.

Example:

```solidity
unstakeForGateway("unique-gateway-id")
```
 -->

### **Common Notes**

<!-- - **Wind-Down Delay:** Gateways must wait a specified number of epochs after calling `windDownGateways` before unstaking. -->

- **Minimum Epochs Locked:** Applies to both Gateways and Providers. Check this value using `minEpochsLocked` in `EpochController`.
- **Reward Delay:** Rewards can only be claimed for epochs up to `currentEpoch - epochRewardDelay`.
- **Provider Metadata:** The metadata for a provider is typically the URL to access its node, enabling external systems to connect and send inference requests.
- **FUNC Token Approvals:** Always approve FUNC transfers before staking.

## Provider and Gateway Active Status

#### **Provider Active Status**

A **Provider** is considered **Active** if the following conditions are met:

1. **Registered:** The provider has been registered in the Function Network.
2. **Not Deactivated:** The provider has not been explicitly deactivated.
3. **Sufficiently Staked:** The provider has staked an amount of FUNC tokens equal to or greater than the `modelStakeAmount` for the associated model.
4. **Whitelisted:** The provider is whitelisted.
5. **Model is Active:** The model associated with the provider is enabled and functional.
<!--

#### **Gateway Active Status**

A **Gateway** is considered **Active** if the following conditions are met:

1. **Registered:** The gateway has been registered in the Function Network.
2. **Not Deactivated:** The gateway has not been explicitly deactivated.
3. **Effective Stake Amount:** The gateway has an effective stake balance that is not overdrawn.
4. **Whitelisted:** The gateway is whitelisted. -->
