# Provider Registry Contract Documentation

The `ProviderRegistry` contract manages provider registration, metadata updates, staking integration, and lifecycle management. It is a core component of the system, facilitating interactions between providers and the broader staking and rewards framework.

---

## Key Features

1. **Provider Registration**:

    - Providers can register with or without metadata.
    - Supports staking during registration for seamless integration.

2. **Metadata Management**:

    - Providers can update their metadata post-registration.

3. **Staking Integration**:

    - Automatically stakes FUNC tokens during registration if enabled.
    - Integrates with `ProviderStaking` for staking and reward management.

4. **Lifecycle Management**:

    - Supports activation and deactivation of providers.
    - Handles provider unstaking and reward claims.

5. **Access Control**:

    - Role-based access ensures secure operations.
    - Owner-only modifications for registered providers.

6. **Whitelisting**:
    - Automatic or manual whitelisting of providers upon registration.

---

## Roles and Access Control

-   **`DEFAULT_ADMIN_ROLE`**:

    -   Full administrative control over the contract.

-   **Modifiers**:
    -   `onlyRegisteredProvider`: Ensures the caller is the owner of a registered provider.

---

## State Variables

-   **Core Components**:

    -   `FUNC`: The FUNC token used for staking.
    -   `router`: Router contract for system integration.

-   **Provider Data**:

    -   `providerIds`: List of all registered provider IDs.
    -   `providers`: Mapping from provider IDs to provider details.
    -   `providerIdsByOwner`: Mapping from owner addresses to their provider IDs.

-   **Whitelisting**:
    -   `autoWhitelist`: Boolean to control automatic whitelisting of registered providers.

---

## Key Functions

### Registration

#### `register`

Registers a provider with optional metadata.

-   **Parameters**:
    -   `_id`: Unique provider ID.
    -   `_modelId`: Model ID associated with the provider.
    -   `metadata`: Metadata string (optional).

---

#### `registerAndStake`

Registers a provider and stakes FUNC tokens for the associated staking contract.

-   **Parameters**:
    -   `_id`: Unique provider ID.
    -   `_modelId`: Model ID associated with the provider.
    -   `metadata`: Metadata string.

---

### Provider Management

#### `deactivate`

Deactivates a provider, marking it as inactive.

-   **Parameters**:
    -   `_id`: Unique provider ID.

---

#### `reactivate`

Reactivates a provider, marking it as active.

-   **Parameters**:
    -   `_id`: Unique provider ID.

---

#### `updateMetadata`

Updates the metadata for a specific provider.

-   **Parameters**:
    -   `_id`: Unique provider ID.
    -   `metadata`: New metadata string.

---

### Staking and Rewards

#### `claimRewardsCluster`

Claims rewards for all providers owned by the caller within a specified epoch range.

-   **Parameters**:
    -   `_fromEpoch`: Starting epoch.
    -   `_toEpoch`: Ending epoch.

---

#### `unstakeCluster`

Unstakes all providers owned by the caller.

---

### Whitelisting

#### `toggleAutomaticWhitelist`

Toggles the automatic whitelisting feature.

---

### View Functions

#### `getProvider`

Retrieves details of a provider by ID.

-   **Parameters**:

    -   `_id`: Unique provider ID.

-   **Returns**:
    -   `Provider` struct containing provider details.

---

#### `getActiveProviders`

Returns a list of all active providers.

-   **Returns**:
    -   Array of `Provider` structs representing active providers.

---

#### `getActiveProviderIds`

Returns a list of IDs for all active providers.

-   **Returns**:
    -   Array of provider IDs.

---

#### `getProviderIds`

Returns a list of all registered provider IDs.

-   **Returns**:
    -   Array of provider IDs.

---

#### `getProviderIdsByOwner`

Returns a list of provider IDs owned by a specific address.

-   **Parameters**:

    -   `_owner`: Owner address.

-   **Returns**:
    -   Array of provider IDs.

---

#### `active`

Checks if a provider is active.

-   **Parameters**:

    -   `_id`: Unique provider ID.

-   **Returns**:
    -   Boolean indicating if the provider is active.

---

#### `registered`

Checks if a provider is registered.

-   **Parameters**:

    -   `_id`: Unique provider ID.

-   **Returns**:
    -   Boolean indicating if the provider is registered.

---

## Events

-   **`ProviderRegistered(bytes id)`**:

    -   Emitted when a provider is registered.

-   **`ProviderRegisteredAndStaked(bytes id)`**:

    -   Emitted when a provider is registered and staked.

-   **`ProviderDeactivated(bytes id)`**:

    -   Emitted when a provider is deactivated.

-   **`ProviderReactivated(bytes id)`**:

    -   Emitted when a provider is reactivated.

-   **`ProviderMetadataUpdated(bytes id)`**:

    -   Emitted when a provider's metadata is updated.

-   **`RewardClaimedForCluster(address indexed owner, uint256 succeeded, uint256 attempted, uint256 fromEpoch, uint256 toEpoch)`**:

    -   Emitted when rewards are claimed for a cluster of providers.

-   **`UnstakeCluster(address indexed owner, uint256 succeeded, uint256 attempted)`**:
    -   Emitted when unstaking is performed for a cluster of providers.
