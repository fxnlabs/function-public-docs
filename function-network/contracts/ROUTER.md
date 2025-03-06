# Router Contract Documentation

The `Router` contract serves as the central registry for key FUNC system contracts, managing their addresses and ensuring seamless integration and communication between components.

---

## Key Features

1. **Centralized Management**:

    - Stores and manages addresses of all core contracts within the FUNC ecosystem.

2. **Role-Based Access Control**:

    - Ensures only authorized entities can update the stored contract addresses.

3. **Initializable Design**:

    - Supports initialization to securely set up the contract with required addresses.

4. **Provider Staking Management**:
    - Links staking pools with models and facilitates updates.

---

## Roles and Access Control

-   **`DEFAULT_ADMIN_ROLE`**:
    -   Full administrative control over the contract.
    -   Can update the addresses of core system contracts and staking pools.

---

## State Variables

### Core Contracts

-   `providerStakingFactory`: Factory contract for creating provider staking pools.
-   `providerRegistry`: Registry for managing providers.
-   `providerCheckpoint`: Tracks provider state history and checkpoints.
-   `gatewayStaking`: Manages staking operations for gateways.
-   `gatewayRegistry`: Registry for managing gateways.
-   `gatewayCheckpoint`: Tracks gateway state history and checkpoints.
-   `epochController`: Handles epoch-based logic and scheduling.
-   `permissions`: Manages permissions for providers and gateways.
-   `treasury`: Treasury contract for managing funds.
-   `model`: Model contract for managing models.
-   `logic`: Logic contract for managing computation and reward functions.

### Provider Staking Pools

-   `providerStakingPools`: Mapping of model IDs to their associated staking pools.

---

## Key Functions

### Initialization

#### `initialize`

Initializes the `Router` contract with addresses of all core contracts.

-   **Parameters**:
    -   `_initializeAddresses`: A struct containing the addresses of all required contracts.

---

### Contract Address Management

#### `setProviderStakingFactory`

Updates the `ProviderStakingFactory` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_providerStakingFactory`: New address of the `ProviderStakingFactory`.

#### `setProviderCheckpoint`

Updates the `ProviderCheckpoint` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_providerCheckpoint`: New address of the `ProviderCheckpoint`.

#### `setProviderRegistry`

Updates the `ProviderRegistry` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_providerRegistry`: New address of the `ProviderRegistry`.

#### `setGatewayStaking`

Updates the `GatewayStaking` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_gatewayStaking`: New address of the `GatewayStaking`.

#### `setGatewayCheckpoint`

Updates the `GatewayCheckpoint` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_gatewayCheckpoint`: New address of the `GatewayCheckpoint`.

#### `setGatewayRegistry`

Updates the `GatewayRegistry` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_gatewayRegistry`: New address of the `GatewayRegistry`.

#### `setEpochController`

Updates the `EpochController` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_epochController`: New address of the `EpochController`.

#### `setTreasury`

Updates the `Treasury` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_treasury`: New address of the `Treasury`.

#### `setPermissions`

Updates the `Permissions` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_permissions`: New address of the `Permissions`.

#### `setLogic`

Updates the `Logic` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_logic`: New address of the `Logic`.

#### `setModel`

Updates the `Model` address.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_model`: New address of the `Model`.

---

### Provider Staking Pool Management

#### `providerStaking`

Retrieves the staking pool associated with a given model ID.

-   **Parameters**:
    -   `_modelId`: Model ID to retrieve the staking pool for.
-   **Returns**: Address of the associated `ProviderStaking` contract.

#### `setStakingPool`

Updates the staking pool associated with a given model ID.

-   **Access**: `DEFAULT_ADMIN_ROLE` or `Model` contract.
-   **Parameters**:
    -   `_modelId`: Model ID to update.
    -   `_stakingPool`: New staking pool address.

---

## Events

-   **`ProviderStakingFactoryUpdated(IProviderStakingFactory providerStakingFactory)`**:
    -   Emitted when the `ProviderStakingFactory` address is updated.
-   **`ProviderCheckpointUpdated(IProviderCheckpoint providerCheckpoint)`**:

    -   Emitted when the `ProviderCheckpoint` address is updated.

-   **`ProviderRegistryUpdated(IProviderRegistry providerRegistry)`**:

    -   Emitted when the `ProviderRegistry` address is updated.

-   **`GatewayStakingUpdated(IGatewayStaking gatewayStaking)`**:

    -   Emitted when the `GatewayStaking` address is updated.

-   **`GatewayCheckpointUpdated(IGatewayCheckpoint gatewayCheckpoint)`**:

    -   Emitted when the `GatewayCheckpoint` address is updated.

-   **`GatewayRegistryUpdated(IGatewayRegistry gatewayRegistry)`**:

    -   Emitted when the `GatewayRegistry` address is updated.

-   **`EpochControllerUpdated(IEpochController epochController)`**:

    -   Emitted when the `EpochController` address is updated.

-   **`TreasuryUpdated(ITreasury treasury)`**:

    -   Emitted when the `Treasury` address is updated.

-   **`PermissionsUpdated(IPermissions permissions)`**:

    -   Emitted when the `Permissions` address is updated.

-   **`LogicUpdated(ILogic logic)`**:

    -   Emitted when the `Logic` address is updated.

-   **`ModelUpdated(IModel model)`**:

    -   Emitted when the `Model` address is updated.

-   **`ProviderStakingUpdated(uint256 modelId, IProviderStaking stakingPool)`**:
    -   Emitted when the staking pool for a model is updated.

---

## Fallback Functions

-   **`receive()`**: Rejects Ether transfers.
-   **`fallback()`**: Rejects Ether transfers and invalid calls.
