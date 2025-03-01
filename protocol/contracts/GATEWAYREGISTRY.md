# GatewayRegistry Contract Documentation

The `GatewayRegistry` contract serves as a management system for gateways, facilitating their registration, staking, metadata updates, and lifecycle operations. It ensures secure, role-based access and maintains an organized registry of gateways.

---

## Key Features

1. **Gateway Registration**:

    - Allows users to register gateways with unique identifiers and metadata.
    - Supports optional FUNC token staking during registration.

2. **Metadata Management**:

    - Provides functions to update metadata associated with gateways.

3. **Lifecycle Operations**:

    - Supports activation, deactivation, and wind-down of gateways.

4. **Access Control**:

    - Enforces role-based access for administrative functions.
    - Restricts gateway-specific actions to their respective owners.

5. **Event Logging**:
    - Logs key events such as gateway registration, updates, and lifecycle changes.

---

## Contract Roles

### Access Control

The contract uses OpenZeppelin's `AccessControl` to implement role-based permissions.

-   **`DEFAULT_ADMIN_ROLE`**:
    -   Grants access to administrative functions like toggling auto-whitelist.

### Modifiers

-   **`onlyRegisteredGatewayOwner`**:
    -   Restricts certain actions to the owner of a registered gateway.

---

## Data Structures

### Gateway

Represents a registered gateway.

-   **`owner`**: Address of the gateway's owner.
-   **`id`**: Unique identifier for the gateway.
-   **`registeredAt`**: Block number when the gateway was registered.
-   **`metadata`**: Metadata string associated with the gateway.
-   **`active`**: Boolean indicating whether the gateway is active.

---

## Key Functions

### Constructor

Initializes the `GatewayRegistry` contract with references to the FUNC token and Router contract.

#### Parameters:

-   **`_FUNC`**: Address of the FUNC ERC20 token contract.
-   **`_router`**: Address of the Router contract.

---

### Registration and Staking

#### `register(bytes calldata _id, string memory metadata)`

Registers a new gateway with optional metadata.

-   **Parameters**:
    -   `_id`: Unique identifier for the gateway.
    -   `metadata`: Metadata string (max length: `1024`).
-   **Emits**:
    -   `GatewayRegistered(_id, metadata)`

---

#### `registerAndStake(bytes calldata _id, string memory metadata, uint256 amount)`

Registers a new gateway and stakes FUNC tokens.

-   **Parameters**:
    -   `_id`: Unique identifier for the gateway.
    -   `metadata`: Metadata string (max length: `1024`).
    -   `amount`: Amount of FUNC tokens to stake.
-   **Emits**:
    -   `GatewayRegisteredAndStaked(_id, amount, metadata)`

---

### Lifecycle Operations

#### `windDownCluster()`

Winds down all gateways owned by the caller.

-   **Emits**:
    -   `GatewayClusterWindDown(msg.sender)`

---

#### `unstakeCluster()`

Unstakes all gateways owned by the caller.

-   **Emits**:
    -   `GatewayClusterUnstake(msg.sender)`
    -   Logs errors for failed unstaking attempts.

---

### Metadata Management

#### `updateMetadata(bytes calldata _id, string memory metadata)`

Updates the metadata of a registered gateway.

-   **Parameters**:
    -   `_id`: Unique identifier for the gateway.
    -   `metadata`: New metadata string (max length: `1024`).
-   **Emits**:
    -   `GatewayMetadataUpdated(_id)`

---

### Activation and Deactivation

#### `deactivate(bytes calldata _id)`

Deactivates a registered gateway.

-   **Parameters**:
    -   `_id`: Unique identifier for the gateway.
-   **Emits**:
    -   `GatewayDeactivated(_id)`

---

#### `reactivate(bytes calldata _id)`

Reactivates a registered gateway.

-   **Parameters**:
    -   `_id`: Unique identifier for the gateway.
-   **Emits**:
    -   `GatewayReactivated(_id)`

---

### Query Functions

#### `getGatewayIds()`

Returns the list of all registered gateway IDs.

-   **Returns**:
    -   Array of all gateway IDs.

---

#### `getGatewayIdsByOwner(address _owner)`

Returns the list of gateway IDs owned by a specific address.

-   **Parameters**:
    -   `_owner`: Address of the gateway owner.
-   **Returns**:
    -   Array of gateway IDs owned by the specified address.

---

#### `getActiveGatewayIds()`

Returns the IDs of all currently active gateways.

-   **Returns**:
    -   Array of active gateway IDs.

---

#### `getActiveGateways()`

Returns a list of all currently active gateways.

-   **Returns**:
    -   Array of `Gateway` structs representing active gateways.

---

#### `getActiveGatewayCount()`

Returns the count of currently active gateways.

-   **Returns**:
    -   Number of active gateways.

---

#### `getGateway(bytes calldata _id)`

Returns the `Gateway` struct associated with a given `_id`.

-   **Parameters**:
    -   `_id`: Unique identifier of the gateway.
-   **Returns**:
    -   A `Gateway` struct containing the gateway's details.

---

#### `registered(bytes calldata _id)`

Checks if a gateway is registered.

-   **Parameters**:
    -   `_id`: Unique identifier of the gateway.
-   **Returns**:
    -   Boolean indicating whether the gateway is registered.

---

## Events

-   **`GatewayRegistered(bytes id, string metadata)`**  
    Emitted when a gateway is registered.

-   **`GatewayRegisteredAndStaked(bytes id, uint256 amount, string metadata)`**  
    Emitted when a gateway is registered and staked.

-   **`GatewayClusterWindDown(address indexed owner)`**  
    Emitted when a gateway cluster is wound down.

-   **`GatewayClusterUnstake(address indexed owner)`**  
    Emitted when a gateway cluster is unstaked.

-   **`GatewayMetadataUpdated(bytes id)`**  
    Emitted when a gateway's metadata is updated.

-   **`GatewayDeactivated(bytes id)`**  
    Emitted when a gateway is deactivated.

-   **`GatewayReactivated(bytes id)`**  
    Emitted when a gateway is reactivated.

-   **`LogError(string message)`**  
    Emitted when an error occurs during gateway unstaking operations.
