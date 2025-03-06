# GatewayCheckpoint Contract Documentation

The `GatewayCheckpoint` contract is a core component for managing gateway-related state and historical data. It allows administrators and schedulers to create and update checkpoints for gateways, enabling operations such as staking, unstaking, and compute unit tracking.

---

## Key Features

1. **Gateway Checkpoint Management**:

    - Supports the creation of scheduler and operation checkpoints for gateways.
    - Tracks gateway states by epoch for accurate reporting and auditing.

2. **Role-Based Access Control**:

    - Admins and schedulers can update scheduler checkpoints.
    - Gateway staking contracts can update operation checkpoints.

3. **Event-Driven Architecture**:
    - Emits events for every checkpoint update, facilitating off-chain integration and monitoring.

---

## Contract Roles

### Access Control

The contract uses OpenZeppelin's `AccessControl` to enforce role-based access.

-   **`DEFAULT_ADMIN_ROLE`**:
    -   Can manage scheduler checkpoints.
    -   Assigned to the deploying address by default.
-   **`SCHEDULER_ROLE`**:
    -   Can update scheduler checkpoints.

### Modifiers

-   **`onlyAdminOrScheduler`**: Restricts function calls to accounts with `DEFAULT_ADMIN_ROLE` or `SCHEDULER_ROLE`.
-   **`onlyStaking`**: Restricts function calls to the address of the `GatewayStaking` contract.

---

## Data Structures

### GatewaySchedulerCheckpoint

Represents a scheduler checkpoint for a gateway in a specific epoch.

-   **`epoch`**: The epoch of the checkpoint.
-   **`gatewayIds`**: Array of gateway IDs included in the checkpoint.
-   **`computeUnits`**: Array of compute units for each gateway.
-   **`isFinalized`**: Boolean indicating whether the checkpoint is set.

---

### GatewayOperationCheckpoint

Represents an operation checkpoint for a gateway in a specific epoch.

-   **`id`**: Gateway ID.
-   **`epoch`**: Epoch of the operation.
-   **`amount`**: Amount involved in the operation.
-   **`totalPoolBalanceAfterOperation`**: Pool balance after the operation.
-   **`operation`**: Operation code (e.g., stake, unstake, consume units).

---

## Key Functions

### Constructor

Initializes the `GatewayCheckpoint` contract and sets the router reference.

#### Parameters:

-   **`_router`**: Address of the Router contract (cannot be zero).

---

### Scheduler Checkpoints

#### `updateGatewaySchedulerCheckpoint(uint256 _epoch, bytes[] calldata _gatewayIds, uint256[] calldata _computeUnits)`

Creates or updates a scheduler checkpoint for a specified epoch.

-   **Parameters**:
    -   `_epoch`: The epoch of the checkpoint.
    -   `_gatewayIds`: List of gateway IDs.
    -   `_computeUnits`: List of compute units for each gateway.
-   **Requirements**:
    -   `_gatewayIds` and `_computeUnits` must have the same length.
    -   A checkpoint for the specified epoch must not already exist.
-   **Emits**:
    -   `GatewaySchedulerCheckpointUpdated(_epoch)`

---

#### `getGatewaySchedulerCheckpoint(uint256 _epoch)`

Retrieves the scheduler checkpoint for a given epoch.

-   **Parameters**:
    -   `_epoch`: The epoch of the checkpoint.
-   **Returns**:
    -   The `GatewaySchedulerCheckpoint` for the epoch.
-   **Reverts**:
    -   If the checkpoint for the epoch is not set.

---

### Operation Checkpoints

#### `createGatewayOperationCheckpoint(bytes calldata _id, uint256 _amount, uint256 _poolBalanceAfterOperation, uint8 _operation)`

Creates a new operation checkpoint for a gateway.

-   **Parameters**:
    -   `_id`: Gateway ID.
    -   `_amount`: Amount involved in the operation.
    -   `_poolBalanceAfterOperation`: Pool balance after the operation.
    -   `_operation`: Operation code:
        -   `0`: Unstake
        -   `1`: Stake
        -   `2`: Consume Compute Units
-   **Requirements**:
    -   Caller must be the `GatewayStaking` contract.
    -   `_operation` must be a valid operation code.
-   **Emits**:
    -   `GatewayOperationCheckpointUpdated(_id, _amount, _operation)`

---

#### `getGatewayOperationCheckpoints(uint256 _epoch)`

Retrieves all operation checkpoints for a given epoch.

-   **Parameters**:
    -   `_epoch`: The epoch of the checkpoints.
-   **Returns**:
    -   An array of `GatewayOperationCheckpoint` structs.

---

## Events

-   **`GatewaySchedulerCheckpointUpdated(uint256 epoch)`**  
    Emitted when a scheduler checkpoint is updated.

-   **`GatewayOperationCheckpointUpdated(bytes id, uint256 amount, uint8 operation)`**  
    Emitted when an operation checkpoint is updated.
    -   **`id`**: Gateway ID.
    -   **`amount`**: Amount involved in the operation.
    -   **`operation`**: Operation code (e.g., stake, unstake, consume units).

## Access and Security

### Role Management

-   Admins can assign and revoke roles (`DEFAULT_ADMIN_ROLE`, `SCHEDULER_ROLE`).
-   `SCHEDULER_ROLE` is intended for accounts managing scheduler checkpoints.

### Validations

-   Checkpoint updates validate input parameters (e.g., lengths, uniqueness).
-   Only authorized accounts or contracts can interact with checkpoint creation.

---

## Usage Examples

### Create Scheduler Checkpoint

```solidity
gatewayCheckpoint.updateGatewaySchedulerCheckpoint(
    1,                      // Epoch
    [gatewayId1, gatewayId2], // Gateway IDs
    [100, 200]               // Compute units
);
```

### Create Operation Checkpoint

```solidity
gatewayCheckpoint.createGatewayOperationCheckpoint(
    gatewayId,      // Gateway ID
    50,             // Amount
    1000,           // Pool balance after operation
    1               // Operation code (stake)
);
```
