# Permissions Contract Documentation

The `Permissions` contract manages the whitelisting and jailing of Gateways and Providers within the system. It ensures secure access control through role-based mechanisms and integrates with other components for entity registration and epoch management.

---

## Key Features

1. **Whitelisting**:

    - Enables adding or removing Gateways and Providers to/from the whitelist.
    - Entities must be registered before they can be whitelisted.

2. **Jailing**:

    - Allows Gateways or Providers to be temporarily jailed for a specified number of epochs.
    - Supports unjailing entities manually.

3. **Access Control**:

    - Role-based access for admins and whitelisters to manage entities securely.

4. **Batch Processing**:
    - Supports batch operations with a defined maximum batch size to optimize gas usage.

---

## Roles

### Access Control

The contract employs `AccessControl` for secure operations.

-   **`DEFAULT_ADMIN_ROLE`**:

    -   Grants full administrative privileges for managing the contract.

-   **`WHITELISTER_ROLE`**:
    -   Allows managing the whitelist and jailing entities.

---

## State Variables

-   **`router`**:

    -   Reference to the `Router` contract for system-wide integrations.

-   **`PROVIDER` / `GATEWAY`**:

    -   Constants representing the entity types:
        -   `PROVIDER = 0`
        -   `GATEWAY = 1`

-   **`providerJailedTilEpoch` / `gatewayJailedTilEpoch`**:

    -   Mappings of entity IDs to the epoch until which they are jailed.

-   **`providerWhitelist` / `gatewayWhitelist`**:

    -   Mappings of entity IDs to their whitelist status.

-   **`MAX_BATCH_SIZE`**:
    -   The maximum number of entities that can be processed in a single batch operation (default: 100).

---

## Key Functions

### Constructor

#### `constructor(IRouter _router)`

Initializes the contract and assigns the deployer the `DEFAULT_ADMIN_ROLE`.

-   **Parameters**:
    -   `_router`: Address of the `Router` contract.

---

### Whitelisting Functions

#### `whitelisted(bytes calldata _id, uint8 _type) -> bool`

Checks if a Gateway or Provider is whitelisted and not jailed.

-   **Parameters**:
    -   `_id`: The ID of the entity.
    -   `_type`: The entity type (`0` for Provider, `1` for Gateway).
-   **Returns**:
    -   `true` if the entity is whitelisted and not jailed, otherwise `false`.

---

#### `whitelist(bytes[] calldata _ids, uint8 _type)`

Adds a batch of IDs to the whitelist.

-   **Parameters**:
    -   `_ids`: Array of IDs to whitelist.
    -   `_type`: The entity type (`0` for Provider, `1` for Gateway).
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE` or `WHITELISTER_ROLE`.

---

#### `removeFromWhitelist(bytes[] calldata _ids, uint8 _type)`

Removes a batch of IDs from the whitelist.

-   **Parameters**:
    -   `_ids`: Array of IDs to remove.
    -   `_type`: The entity type (`0` for Provider, `1` for Gateway).
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE` or `WHITELISTER_ROLE`.

---

### Jailing Functions

#### `jailForEpochs(bytes calldata _id, uint8 _type, uint256 _numEpochs)`

Temporarily jails a Gateway or Provider for a specified number of epochs.

-   **Parameters**:
    -   `_id`: The ID of the entity.
    -   `_type`: The entity type (`0` for Provider, `1` for Gateway).
    -   `_numEpochs`: The number of epochs to jail the entity.
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE` or `WHITELISTER_ROLE`.

---

#### `unjail(bytes calldata _id, uint8 _type)`

Manually unjails a Gateway or Provider.

-   **Parameters**:
    -   `_id`: The ID of the entity.
    -   `_type`: The entity type (`0` for Provider, `1` for Gateway).
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE` or `WHITELISTER_ROLE`.

---

## Events

-   **`Whitelisted(bytes id, uint8 indexed type)`**:

    -   Emitted when an entity is added to the whitelist.

-   **`RemovedFromWhitelist(bytes id, uint8 indexed type)`**:

    -   Emitted when an entity is removed from the whitelist.

-   **`Jailed(bytes id, uint8 indexed type, uint256 numEpochs, uint256 jailUntil)`**:

    -   Emitted when an entity is jailed.

-   **`Unjailed(bytes id, uint8 indexed type)`**:
    -   Emitted when an entity is unjailed.
