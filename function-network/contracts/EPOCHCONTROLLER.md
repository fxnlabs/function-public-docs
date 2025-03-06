# EpochController Contract Documentation

The `EpochController` contract is a critical component for managing and tracking epochs in a blockchain-based system. Epochs are defined as fixed intervals of blocks and are used for various time-based operations, such as staking, reward distribution, and lock periods. This contract provides administrators the ability to configure epoch parameters and query epoch-related information in a decentralized manner.

---

## Key Features

1. **Epoch Management**:

    - Tracks the current epoch and calculates the start of the next epoch.
    - Allows modification of epoch-related parameters like length, lock periods, and reward delays.

2. **Admin-Restricted Modifications**:

    - All configuration changes can only be performed by accounts with the `DEFAULT_ADMIN_ROLE`.

3. **Parameter Validations**:

    - Ensures epoch parameters fall within specified minimum and maximum bounds to maintain system stability.

4. **Event Emission**:
    - Emits events whenever epoch parameters are updated, enabling off-chain monitoring and integration.

---

## Key Parameters

### Public Constants

-   **`MIN_EPOCH_LENGTH` / `MAX_EPOCH_LENGTH`**: Minimum and maximum allowed blocks per epoch.
-   **`MIN_EPOCH_LOCK` / `MAX_EPOCH_LOCK`**: Bounds for the minimum number of epochs tokens can be locked.
-   **`MIN_REWARD_DELAY` / `MAX_REWARD_DELAY`**: Bounds for the delay in epochs before rewards can be claimed.

### State Variables

-   **`epochLength`**: The number of blocks in an epoch.
-   **`firstEpochBlock`**: The block number of the first epoch.
-   **`minEpochsLocked`**: Minimum number of epochs tokens are locked.
-   **`epochRewardDelay`**: Number of epochs before rewards are claimable.
-   **`epochCheckpoint`**: The current epoch checkpoint number.
-   **`epochCheckpointBlock`**: Block number when the last checkpoint was updated.

---

## Key Functions

### Constructor

Sets the initial values for the epoch parameters and validates them against the defined bounds.

#### Parameters:

-   **`_epochLength`**: Number of blocks in an epoch.
-   **`_firstEpochBlock`**: Block number of the first epoch. If zero, it defaults to the next epoch block.
-   **`_minEpochsLocked`**: Minimum number of epochs tokens are locked.
-   **`_epochRewardDelay`**: Number of epochs before rewards are claimable.

#### Emits:

-   `EpochRewardDelayUpdated`
-   `MinEpochsLockedUpdated`
-   `EpochLengthUpdated`

---

### Epoch Parameter Setters

#### `setEpochLength(uint256 _epochLength)`

-   Updates the number of blocks per epoch.
-   Reverts if the new epoch length is outside the allowed range or if reducing the epoch length in the current epoch.

#### Emits:

-   `EpochLengthUpdated`

---

#### `setMinEpochsLocked(uint256 _minEpochsLocked)`

-   Updates the minimum number of epochs tokens are locked.
-   Reverts if the new value is outside the allowed range.

#### Emits:

-   `MinEpochsLockedUpdated`

---

#### `setEpochRewardDelay(uint256 _epochRewardDelay)`

-   Updates the reward delay in epochs.
-   Reverts if the new value is outside the allowed range.

#### Emits:

-   `EpochRewardDelayUpdated`

---

### Epoch Queries

#### `nextEpochBlock()`

-   Returns the block number of the next epoch based on the current block and configured parameters.

---

#### `epochNumber()`

-   Returns the current epoch number based on the current block, `epochLength`, and `epochCheckpoint`.

---

## Access Control

The contract uses OpenZeppelin's `AccessControl` for role-based access management.

### Roles:

-   **`DEFAULT_ADMIN_ROLE`**:
    -   Has the authority to update epoch parameters.
    -   Assigned to the deploying account during contract initialization.

---

## Events

-   **`EpochLengthUpdated(uint256 epochLength)`**  
    Emitted when the epoch length is updated.

-   **`MinEpochsLockedUpdated(uint256 minEpochsLocked)`**  
    Emitted when the minimum epochs lock parameter is updated.

-   **`EpochRewardDelayUpdated(uint256 epochRewardDelay)`**  
    Emitted when the epoch reward delay parameter is updated.

## Usage Examples

### Initialization

```solidity
EpochController epochController = new EpochController(
    100,        // epochLength (blocks)
    1000000,    // firstEpochBlock
    10,         // minEpochsLocked
    5           // epochRewardDelay
);
```

### Querying Epoch Information

```solidity
uint256 currentEpoch = epochController.epochNumber();
uint256 nextEpochStartBlock = epochController.nextEpochBlock();
```

This contract is designed to be a robust and flexible controller for epoch-based systems, enabling precise control over time-sensitive operations in decentralized environments.
