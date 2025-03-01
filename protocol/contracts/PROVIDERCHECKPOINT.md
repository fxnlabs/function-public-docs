# Provider Checkpoint Contract Documentation

The `ProviderCheckpoint` contract manages state tracking, checkpoint creation, and reward distribution for Providers. It integrates with other system components, including the Router, to facilitate operations like staking, unstaking, and rewards management.

---

## Key Features

1. **Provider Checkpoints**:

    - Tracks provider-related operations (e.g., staking, unstaking, rewards) using operation checkpoints.

2. **Scheduler Integration**:

    - Creates scheduler checkpoints for models to aggregate and finalize shard contributions and rewards.

3. **Reward Management**:

    - Tracks and finalizes provider reward checkpoints for specific epochs.

4. **Access Control**:

    - Securely restricts functions to admin or scheduler roles.

5. **Router Integration**:
    - Facilitates interactions with other system components via the Router.

---

## Roles

### Access Control

The contract uses `AccessControl` to manage roles and permissions.

-   **`DEFAULT_ADMIN_ROLE`**:

    -   Grants full administrative access to the contract.

-   **`SCHEDULER_ROLE`**:
    -   Allows schedulers to perform operations like creating checkpoints.

---

## State Variables

-   **Router Reference**:

    -   `router`: The Router contract used to interact with other system components.

-   **Checkpoint Mappings**:

    -   `providerSchedulerCheckpoints`: Maps model IDs and epochs to scheduler checkpoints.
    -   `providerOperationCheckpoints`: Maps model IDs to an array of operation checkpoints.
    -   `providerOperationCheckpointsByEpoch`: Maps model IDs and epochs to an array of operation checkpoints.
    -   `providerRewardCheckpoints`: Maps provider IDs and epochs to reward checkpoints.
    -   `shardSumsForEpoch`: Tracks the sum of shards for each model ID and epoch.
    -   `providerShardsInEpoch`: Tracks provider shards for each model ID and epoch.
    -   `rewardCheckpointBatch`: Tracks the reward batch ID for each model ID and epoch.

-   **Last Updated Epoch**:
    -   `lastSchedulerUpdatedEpochForModel`: Tracks the last epoch where a scheduler checkpoint was updated for each model.

---

## Key Functions

### Constructor

#### `constructor(IRouter _router)`

Initializes the contract and assigns the deployer the `DEFAULT_ADMIN_ROLE`.

-   **Parameters**:
    -   `_router`: Address of the `Router` contract.

---

### Checkpoint Creation

#### `createProviderRewardsCheckpoints`

Creates provider reward checkpoints for a specified model ID and epoch.

-   **Parameters**:
    -   `_batchId`: The reward batch ID.
    -   `_modelId`: The ID of the model.
    -   `_epoch`: The epoch for the rewards.
    -   `_providerShards`: Array of provider shards contributing to the rewards.
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE` or `SCHEDULER_ROLE`.

---

#### `createProviderSchedulerCheckpoint`

Creates a scheduler checkpoint for a specified model ID and epoch.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
    -   `_epoch`: The epoch for the checkpoint.
    -   `_totalShards`: Total shards in the epoch.
    -   `_totalRewardsForEpoch`: Total rewards for the epoch.
    -   `_expectedRewardBatchId`: The expected batch ID for rewards.
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE` or `SCHEDULER_ROLE`.

---

#### `createProviderOperationCheckpoint`

Creates an operation checkpoint for a specific provider.

-   **Parameters**:
    -   `_id`: The ID of the provider.
    -   `_amount`: The amount involved in the operation.
    -   `_poolBalanceAfterOperation`: The balance after the operation.
    -   `_operation`: The type of operation (unstake, stake, or claim rewards).
-   **Access Control**:
    -   Callable only by authorized staking addresses.

---

### Data Retrieval

#### `getProviderSchedulerCheckpoint`

Retrieves the scheduler checkpoint for a specified model ID and epoch.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
    -   `_epoch`: The epoch for which the checkpoint is requested.
-   **Returns**:
    -   A `ProviderSchedulerCheckpoint` struct.

---

#### `getProviderOperationCheckpoints`

Retrieves all operation checkpoints for a specified model ID.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
-   **Returns**:
    -   An array of `ProviderOperationCheckpoint` structs.

---

#### `getProviderOperationCheckpointsByEpoch`

Retrieves operation checkpoints for a specific model ID and epoch.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
    -   `_epoch`: The epoch for which the checkpoints are requested.
-   **Returns**:
    -   An array of `ProviderOperationCheckpoint` structs.

---

#### `getProviderRewardCheckpoint`

Retrieves the reward checkpoint for a specific provider ID and epoch.

-   **Parameters**:
    -   `_id`: The ID of the provider.
    -   `_epoch`: The epoch for which the reward checkpoint is requested.
-   **Returns**:
    -   A `ProviderRewardCheckpoint` struct.

---

## Events

-   **`ProviderSchedulerCheckpointUpdated(uint256 indexed modelId, uint256 indexed epoch)`**:

    -   Emitted when a scheduler checkpoint is updated.

-   **`ProviderOperationCheckpointUpdated(bytes id, uint256 amount, uint8 operation)`**:
    -   Emitted when an operation checkpoint is created.
