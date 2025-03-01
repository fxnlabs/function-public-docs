# Provider Staking Contract Documentation

The `ProviderStaking` contract enables staking FUNC tokens to earn rewards for providers. It integrates with other system components to handle staking, unstaking, reward claiming, and tracking provider states.

---

## Key Features

1. **Staking and Unstaking**:

    - Providers can stake FUNC tokens to participate in the system and earn rewards.
    - Unstaking is permitted after a lock period.

2. **Reward Management**:

    - Tracks and distributes rewards to providers based on their contributions during specific epochs.

3. **Boost Mechanism**:

    - Allows additional rewards to be distributed across a fixed number of epochs.

4. **Epoch Tracking**:

    - Maintains balances and rewards at the epoch level for accurate distribution and historical tracking.

5. **Integration with Router**:

    - Interacts with system components such as the `Router`, `ProviderCheckpoint`, and `ProviderRegistry`.

6. **Access Control**:
    - Securely restricts function access using roles and modifiers.

---

## Roles and Access Control

-   **`DEFAULT_ADMIN_ROLE`**:

    -   Full administrative access to the contract.

-   **Modifiers**:
    -   `onlyInitialized`: Ensures the contract is initialized.
    -   `onlyRegisteredProvider`: Ensures the provider is registered.
    -   `onlyWhitelistedProvider`: Ensures the provider is whitelisted.
    -   `onlyProviderCheckpoint`: Restricts access to the `ProviderCheckpoint` contract.

---

## State Variables

-   **Core Contracts**:

    -   `FUNC`: The FUNC token used for staking.
    -   `router`: The Router contract for system integration.
    -   `modelId`: The unique ID of the model associated with this staking pool.

-   **Staking and Rewards**:

    -   `effectiveStakedBalance`: Total effective staked balance in the pool.
    -   `providerStakeEpochs`: Tracks the most recent epoch a provider staked in.
    -   `providerStakes`: Tracks the amount staked by each provider.
    -   `providerRewardsClaimed`: Tracks claimed rewards for providers by epoch.

-   **Epoch Management**:

    -   `epochRewardHardCap`: Maximum rewards distributed in an epoch.
    -   `lastUpdatedEpoch`: The last epoch when balances were updated.
    -   `rewardsToBeDistributedAtEpoch`: Rewards distributed in each epoch.
    -   `effectiveStakedBalanceAtEpoch`: Effective staked balance at each epoch.

-   **Boosts**:
    -   `boosts`: Array of additional rewards distributed over epochs.
    -   `boostIndex`: Current index of boosts yet to be applied.

---

## Key Functions

### Initialization

#### `initialize`

Initializes the contract with the FUNC token, router, model ID, and admin.

-   **Parameters**:
    -   `_token`: The FUNC token address.
    -   `_router`: The Router contract address.
    -   `_modelId`: The unique model ID for this pool.
    -   `_admin`: The address granted `DEFAULT_ADMIN_ROLE`.

---

### Staking and Unstaking

#### `stakeForProvider`

Allows providers to stake FUNC tokens for their participation.

-   **Requirements**:
    -   Provider must be registered and whitelisted.
    -   Model associated with the provider must exist and be enabled.
    -   Staking amount must not exceed the required stake amount.

---

#### `unstakeProvider`

Enables providers to unstake FUNC tokens after the lock period.

-   **Requirements**:
    -   Provider must be registered and whitelisted.
    -   Lock period must have elapsed.
    -   Staking amount must be greater than zero.

---

### Reward Management

#### `claimRewards`

Allows providers to claim rewards for a specified range of epochs.

-   **Parameters**:

    -   `_id`: The provider ID.
    -   `_fromEpoch`: The starting epoch for claiming rewards.
    -   `_toEpoch`: The ending epoch for claiming rewards.

-   **Requirements**:
    -   Provider must have valid rewards for the specified epochs.
    -   Rewards cannot be claimed for future epochs or epochs within the lock period.

---

#### `boostRewardsForNEpochs`

Boosts rewards for the next N epochs.

-   **Parameters**:

    -   `_epochs`: Number of epochs to boost.
    -   `_amount`: Total FUNC amount to boost.

-   **Requirements**:
    -   `_epochs` and `_amount` must be greater than zero.
    -   `_amount` must be evenly divisible by `_epochs`.

---

#### `setTotalRewardForEpoch`

Sets the total rewards for a specific epoch.

-   **Parameters**:

    -   `_epoch`: The epoch to set the reward for.
    -   `_rewardAmount`: The reward amount.
    -   `_utilized`: Whether to apply boosts.

-   **Access Control**:
    -   Callable only by the `ProviderCheckpoint` contract.

---

### View Functions

#### `viewRewards`

Returns the total rewards available for a provider within a specified epoch range.

---

#### `totalRewardForEpoch`

Returns the total rewards for a given epoch.

---

#### `isProviderLive`

Checks if a provider is live based on its stake and eligibility.

---

#### `getProviderStakedAmount`

Returns the staked amount for a specific provider.

---

#### `getEffectiveStakedBalance`

Returns the current effective staked balance of the pool.

---

## Events

-   **`ProviderStaked(bytes id, uint256 modelId, uint256 epoch, uint256 amount)`**:

    -   Emitted when a provider stakes FUNC.

-   **`ProviderUnstaked(bytes id, uint256 modelId, uint256 epoch, uint256 amount)`**:

    -   Emitted when a provider unstakes FUNC.

-   **`ProviderClaimedRewards(bytes id, uint256 modelId, uint256 fromEpoch, uint256 toEpoch, uint256 amount)`**:

    -   Emitted when a provider claims rewards.

-   **`BoostForNEpochs(address indexed sender, uint256 epochs, uint256 amount)`**:

    -   Emitted when boosts are added for epochs.

-   **`EpochHardCapUpdated(uint256 indexed epoch, uint256 hardCap)`**:

    -   Emitted when the reward hard cap is updated.

-   **`RewardSetForEpoch(uint256 indexed epoch, uint256 rewardAmount)`**:

    -   Emitted when rewards are set for an epoch.

-   **`EpochBalancesUpdated(uint256 lastUpdatedEpoch, uint256 currentEpoch)`**:
    -   Emitted when epoch balances are updated.
