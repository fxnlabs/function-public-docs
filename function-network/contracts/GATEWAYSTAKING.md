# GatewayStaking Contract Documentation

The `GatewayStaking` contract facilitates staking of FUNC tokens for gateways, enabling operations like staking, unstaking, usage tracking, and balance management. It integrates with other ecosystem components for secure and efficient functionality.

---

## Key Features

1. **FUNC Token Staking**:

    - Users can stake FUNC tokens for specific gateways.
    - Supports secure and efficient staking with role-based access controls.

2. **Gateway Lifecycle Management**:

    - Includes operations to wind down gateways, restart them, and track their states.

3. **Overdraw Handling**:

    - Tracks overdrawn stakes for gateways and adjusts balances accordingly.

4. **Burn Functionality**:

    - Implements optional burning of FUNC tokens during gateway operations.

5. **Integration with Ecosystem**:
    - Interacts with Router, GatewayRegistry, and GatewayCheckpoint contracts.

---

## Contract Roles

### Access Control

The contract uses `AccessControlledPausable` for role-based access and pause functionality.

-   **`DEFAULT_ADMIN_ROLE`**:

    -   Grants access to administrative functions like toggling burn functionality.

-   **`onlyCheckpoint` Modifier**:
    -   Restricts certain functions to the GatewayCheckpoint contract.

---

## Data Structures

### Gateway State

-   **`gatewayShutdownEpochs`**: Tracks shutdown epochs for gateways.
-   **`gatewayMostRecentStakeEpochs`**: Tracks the most recent staked epoch for gateways.
-   **`gatewayStakeOverdrawn`**: Tracks overdrawn FUNC amounts for gateways.
-   **`gatewayStakes`**: Tracks staked FUNC balances for gateways.

---

## Key Functions

### Constructor

#### `constructor(IERC20 _token, IRouter _router)`

Initializes the `GatewayStaking` contract.

-   **Parameters**:
    -   `_token`: Address of the FUNC ERC20 token contract.
    -   `_router`: Address of the Router contract.

---

### Staking Operations

#### `stakeForGateway(bytes calldata _id, uint256 amount)`

Stakes FUNC tokens for a gateway.

-   **Parameters**:
    -   `_id`: ID of the gateway.
    -   `amount`: Amount of FUNC tokens to stake.
-   **Requirements**:
    -   Gateway must be registered and whitelisted.
    -   Caller must be the gateway owner or the registry.
-   **Emits**:
    -   `GatewayStaked`.

---

#### `unstakeForGateway(bytes calldata _id)`

Unstakes FUNC tokens for a gateway.

-   **Parameters**:
    -   `_id`: ID of the gateway.
-   **Requirements**:
    -   Gateway must meet conditions like being shut down and not overdrawn.
    -   Minimum epochs locked must be satisfied.
-   **Emits**:
    -   `GatewayUnstaked`.

---

#### `windDownGateways(bytes[] calldata _gatewayIdsToWindDown)`

Winds down specified gateways.

-   **Parameters**:
    -   `_gatewayIdsToWindDown`: List of gateway IDs to wind down.
-   **Emits**:
    -   `GatewayWoundDown`.

---

### Burn and Usage Tracking

#### `checkpointUpdateGatewayUsage(bytes[] calldata _gatewayIds, uint256[] calldata _computeUnits)`

Updates gateway usage and burns FUNC tokens if enabled.

-   **Parameters**:
    -   `_gatewayIds`: List of gateway IDs.
    -   `_computeUnits`: Compute units consumed by the gateways.

---

#### `toggleBurn()`

Toggles the burn functionality.

-   **Emits**:
    -   `BurnToggled`.

---

### Gateway State Management

#### `restartGateway(bytes calldata _id)`

Restarts a gateway by resetting its shutdown state.

-   **Parameters**:
    -   `_id`: ID of the gateway.
-   **Emits**:
    -   `GatewayRestarted`.

---

### Query Functions

#### `getAvailableComputeUnitsForGateways(bytes[] calldata _ids)`

Returns available compute units for specified gateways.

-   **Parameters**:
    -   `_ids`: List of gateway IDs.
-   **Returns**:
    -   Array of available compute units for each gateway.

---

#### `getGatewayShutdownEpoch(bytes calldata _id)`

Returns the shutdown epoch for a gateway.

-   **Parameters**:
    -   `_id`: ID of the gateway.
-   **Returns**:
    -   Shutdown epoch or `0` if not shut down.

---

#### `getGatewayMostRecentStakedEpoch(bytes calldata _id)`

Returns the most recent staked epoch for a gateway.

-   **Parameters**:
    -   `_id`: ID of the gateway.
-   **Returns**:
    -   Most recent staked epoch.

---

#### `getGatewayStakeOverdrawnAmount(bytes calldata _id)`

Returns the overdrawn FUNC amount for a gateway.

-   **Parameters**:
    -   `_id`: ID of the gateway.
-   **Returns**:
    -   Overdrawn FUNC amount.

---

#### `getGatewayStakedAmount(bytes calldata _id)`

Returns the staked FUNC amount for a gateway.

-   **Parameters**:
    -   `_id`: ID of the gateway.
-   **Returns**:
    -   Staked FUNC amount.

---

#### `getGatewayTotalBalance()`

Returns the total FUNC balance staked across all gateways.

-   **Returns**:
    -   Total staked FUNC balance.

---

## Events

-   **`GatewayStaked(bytes id, uint256 epoch, uint256 amount)`**  
    Emitted when FUNC is staked for a gateway.

-   **`GatewayUnstaked(bytes id, uint256 epoch, uint256 amount)`**  
    Emitted when FUNC is unstaked for a gateway.

-   **`GatewayWoundDown(bytes id, uint256 epoch)`**  
    Emitted when a gateway is wound down.

-   **`GatewayRestarted(bytes id, uint256 epoch)`**  
    Emitted when a gateway is restarted.

-   **`GatewayBurn(bytes id, uint256 epoch, uint256 amount)`**  
    Emitted when FUNC is burned during a gateway operation.

-   **`GatewayOverdrawn(bytes id, uint256 epoch, uint256 amount)`**  
    Emitted when a gateway becomes overdrawn.

-   **`BurnToggled()`**  
    Emitted when burn functionality is toggled.
