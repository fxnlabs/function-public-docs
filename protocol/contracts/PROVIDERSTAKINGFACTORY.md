# Provider Staking Factory Contract Documentation

The `ProviderStakingFactory` contract is responsible for creating and managing instances of `ProviderStaking` contracts. It facilitates model registration, minimal proxy cloning for efficiency, and integration with the `Model` and `Router` contracts.

---

## Key Features

1. **Dynamic Staking Contract Creation**:

    - Creates new `ProviderStaking` contracts using the minimal proxy pattern.
    - Links new contracts to models in the `Model` contract.

2. **Model Integration**:

    - Registers newly created `ProviderStaking` instances with the `Model` contract and links them to the `Router`.

3. **Access Control**:

    - Role-based access control ensures secure operations.
    - Supports admin and model creator roles.

4. **Efficiency**:
    - Uses the minimal proxy pattern (via OpenZeppelin's `Clones`) to optimize contract deployment.

---

## Roles and Access Control

-   **`DEFAULT_ADMIN_ROLE`**:

    -   Full administrative control over the contract.
    -   Can set the `ProviderStaking` implementation address.

-   **`MODEL_CREATOR_ROLE`**:
    -   Limited to creating new models and associated `ProviderStaking` contracts.

---

## State Variables

-   **Core Components**:

    -   `FUNC`: Address of the FUNC ERC20 token.
    -   `router`: Address of the Router contract.

-   **Provider Staking Implementation**:
    -   `providerStakingImplementation`: Address of the `ProviderStaking` implementation contract.
    -   `providerStakingImplementationAddressSet`: Tracks whether the implementation address has been set.

---

## Key Functions

### Administrative Functions

#### `setProviderStakingImplementation`

Sets the address of the `ProviderStaking` implementation contract for minimal proxy cloning.

-   **Access**: `DEFAULT_ADMIN_ROLE`
-   **Parameters**:
    -   `_implementation`: Address of the `ProviderStaking` implementation contract.

---

### Staking Contract Creation

#### `createNewProviderStaking`

Creates a new `ProviderStaking` contract using a minimal proxy, registers it with the `Model` contract, and links it in the `Router`.

-   **Access**: `DEFAULT_ADMIN_ROLE` or `MODEL_CREATOR_ROLE`
-   **Parameters**:
    -   `_modelId`: Unique ID for the model being created.
    -   `_shards`: Number of shards allocated to the model.
    -   `_computeUnits`: Number of compute units associated with the model.
    -   `_stakeAmount`: Required stake amount for the model.
    -   `_modelName`: Name of the model.
-   **Returns**: Address of the newly created `ProviderStaking` contract.

---

## Workflow

1. **Set ProviderStaking Implementation**:

    - Admin sets the address of the `ProviderStaking` implementation contract via `setProviderStakingImplementation`.

2. **Create New Provider Staking**:
    - A user with the appropriate role calls `createNewProviderStaking`.
    - A new `ProviderStaking` instance is created using the minimal proxy pattern.
    - The new instance is initialized and registered with the `Model` contract.

---

## Events

-   **`ProviderStakingCreated(uint256 indexed modelId, address indexed providerStakingAddress, string modelName)`**:
    -   Emitted when a new `ProviderStaking` contract is created.

---

## Example Usage

1. **Setting the ProviderStaking Implementation**:

```solidity
providerStakingFactory.setProviderStakingImplementation(address(providerStakingImplementation));
```

2. **Creating a New ProviderStaking Contract**:

```solidity
providerStakingFactory.createNewProviderStaking(
    1,            // Model ID
    100,          // Number of shards
    1000,         // Compute units
    500 * 10**18, // Required stake amount
    "Example Model" // Model name
);
```
