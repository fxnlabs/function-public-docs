# Model Registration Contract Documentation

The `Model` contract manages the registration and lifecycle of machine learning models within the system. It handles operations such as enabling/disabling models, updating staking requirements, and associating staking pools with models.

---

## Key Features

1. **Model Registration and Management**:

    - Supports registering new models with metadata and staking requirements.
    - Allows enabling or disabling of registered models.

2. **Stake Requirement Management**:

    - Admins can update the required staking amount for models.

3. **Integration with Staking Pools**:

    - Models are associated with staking pools for FUNC token staking.

4. **Access Control**:
    - Role-based access control for secure management.

---

## Roles

### Access Control

The contract uses `AccessControl` to secure critical functions.

-   **`DEFAULT_ADMIN_ROLE`**:
    -   Full control over all administrative functions.
-   **`MODEL_CREATOR_ROLE`**:
    -   Grants permission to create new models.

---

## State Variables

-   **`router`**:

    -   Reference to the `Router` contract for system-wide functionalities.

-   **`modelIdExists`**:

    -   Mapping of model IDs to their existence status.

-   **`existingModels`**:

    -   Mapping of model IDs to their corresponding `ModelInfo` struct.

-   **`modelIds`**:
    -   Array of all registered model IDs.

---

## Key Functions

### Constructor

#### `constructor(IRouter _router)`

Initializes the contract and assigns the deployer the `DEFAULT_ADMIN_ROLE`.

-   **Parameters**:
    -   `_router`: The address of the router contract.

---

### Model Registration

#### `createModel(uint256 _modelId, uint256 _shards, uint256 _computeUnits, uint256 _stakeAmount, IProviderStaking _staking, string _modelName)`

Registers a new model with specified parameters.

-   **Parameters**:
    -   `_modelId`: Unique ID of the model.
    -   `_shards`: Number of shards associated with the model.
    -   `_computeUnits`: Compute units required for the model.
    -   `_stakeAmount`: Required FUNC staking amount.
    -   `_staking`: Associated staking pool.
    -   `_modelName`: Name of the model.
-   **Access Control**:
    -   Callable by accounts with `MODEL_CREATOR_ROLE` or the `ProviderStakingFactory`.

---

### Model Management

#### `enableModel(uint256 _modelId)`

Enables a previously disabled model.

-   **Parameters**:
    -   `_modelId`: The ID of the model to enable.
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE`.

---

#### `disableModel(uint256 _modelId)`

Disables an active model.

-   **Parameters**:
    -   `_modelId`: The ID of the model to disable.
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE`.

---

#### `updateModel(uint256 _modelId, uint256 _shards, uint256 _computeUnits, string _modelName)`

Updates the metadata of an existing model.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
    -   `_shards`: Updated number of shards.
    -   `_computeUnits`: Updated compute units.
    -   `_modelName`: Updated model name.
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE`.

---

### Staking Management

#### `updateModelStakeRequirement(uint256 _modelId, uint256 amount)`

Updates the FUNC staking requirement for a model.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
    -   `amount`: New FUNC staking requirement.
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE`.

---

#### `updateModelStakingPool(uint256 _modelId, IProviderStaking _staking)`

Updates the staking pool associated with a model.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
    -   `_staking`: Address of the new staking pool.
-   **Access Control**:
    -   Callable by accounts with `DEFAULT_ADMIN_ROLE`.

---

### View Functions

#### `modelStakeAmount(uint256 _modelId) -> uint256`

Returns the staking requirement for a given model ID.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
-   **Returns**:
    -   Required FUNC staking amount.

---

#### `modelExists(uint256 _modelId) -> bool`

Checks whether a model with the given ID exists.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
-   **Returns**:
    -   `true` if the model exists, `false` otherwise.

---

#### `modelEnabled(uint256 _modelId) -> bool`

Checks if a model is both registered and enabled.

-   **Parameters**:
    -   `_modelId`: The ID of the model.
-   **Returns**:
    -   `true` if the model is enabled, `false` otherwise.

---

#### `modelsIds() -> uint256[]`

Returns a list of all registered model IDs.

-   **Returns**:
    -   An array of model IDs.

---

#### `models() -> ModelInfo[]`

Returns detailed information for all registered models.

-   **Returns**:
    -   An array of `ModelInfo` structs for all models.

---

## Data Structures

### `ModelInfo`

Represents information about a model.

-   **Fields**:
    -   `modelId`: Unique ID of the model.
    -   `shards`: Number of shards associated with the model.
    -   `computeUnits`: Compute units required for the model.
    -   `stakeAmount`: Required FUNC staking amount.
    -   `providerStakingPool`: Address of the associated staking pool.
    -   `modelName`: Name of the model.
    -   `enabled`: Boolean indicating if the model is enabled.

---

## Events

-   **`ModelCreated(uint256 indexed modelId, string modelName)`**:

    -   Emitted when a new model is created.

-   **`ModelEnabled(uint256 indexed modelId, bool enabled)`**:

    -   Emitted when a model is enabled or disabled.

-   **`ModelUpdated(uint256 indexed modelId, string modelName)`**:

    -   Emitted when a model's metadata is updated.

-   **`ModelStakeRequirementUpdated(uint256 indexed modelId, uint256 oldStakeAmount, uint256 newStakeAmount)`**:

    -   Emitted when a model's staking requirement is updated.

-   **`ModelStakingPoolUpdated(uint256 indexed modelId, address oldPool, address newPool)`**:
    -   Emitted when a model's staking pool is updated.
