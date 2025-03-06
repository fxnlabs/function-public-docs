# 🚀 Provider Guide

Function Network allows **compute providers** to contribute AI inference power in a **decentralized, scalable, and incentive-driven** ecosystem. By **staking FUNC tokens** and providing **computational resources**, providers earn rewards while supporting **AI model execution**.

---

## 🔗 Provider Resources

🔗 **[Function Network Dapp](#)** – Register, stake, and manage your provider status. (Currently not live)

---

## 🛠️ How It Works

1. **Registration & Staking**

   - Providers **register on the network** and **stake FUNC tokens** to participate.
   - Staking ensures **accountability** and aligns incentives within the ecosystem.

2. **Compute & Task Execution**

   - Providers run **AI inference tasks**, contributing compute power to the network.
   - Gateways dynamically route requests to **active, high-performing providers**.

3. **Rewards & Unstaking**
   - Providers earn FUNC **based on contribution, availability, and performance**.
   - After the required lock period, **staked FUNC can be unstaked if desired**.

---

## 🎯 How to Participate

### **Become a Provider & Start Earning**

🔹 **Register** your provider ID and connect it to an AI model.  
🔹 **Stake FUNC** tokens to activate your provider status.

### **Operate & Optimize Your Compute Node**

🔹 **Process AI inference tasks**, ensuring high availability for requests.  
🔹 **Monitor performance**, maximize uptime, and earn FUNC rewards.

### **Claim & Manage Rewards**

🔹 **Track rewards via the Function dashboard** and claim based on epoch performance.  
🔹 **Unstake FUNC tokens** once the minimum lock period has been met.

---

## 💰 Staking & Rewards

🔹 [**Function Network Dapp**](#) (Currently not live) – Connect your wallet to provide compute resources. Follow the steps to contribute to the network - Note that the ID should be your public peerID, and the URL should be your node URL.  
🔹 **Minimum Staking Period** – FUNC tokens must remain staked for a set number of epochs before unstaking.  
🔹 **Epoch-Based Reward System** – Providers earn rewards at the end of each epoch based on contributions.  
🔹 **FUNC Token Approvals** – Always approve FUNC transfers before staking.

---

## 🔄 **Provider Staking Workflows**

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

### **Common Notes**

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

---

Function Network enables **compute providers** to monetize their **GPU resources**, power **decentralized AI inference**, and contribute to **an open AI economy**. 🚀
