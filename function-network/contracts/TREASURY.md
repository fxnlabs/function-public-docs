# Treasury Contract Documentation

The `Treasury` contract is responsible for managing FUNC tokens held within the treasury and distributing rewards to providers. It ensures secure and efficient reward handling with proper access control and integration with the FUNC ecosystem.

---

## Key Features

1. **Reward Distribution**:

    - Allows distribution of FUNC tokens to providers as rewards.
    - Verifies the validity of reward claims using the Router contract.

2. **Access Control**:

    - Ensures only authorized entities (admins or staking contracts) can distribute rewards.

3. **Reentrancy Protection**:
    - Incorporates protection mechanisms to prevent reentrancy attacks.

---

## Roles and Access Control

-   **`DEFAULT_ADMIN_ROLE`**:
    -   Full administrative control over the contract.
    -   Can distribute rewards to providers.

---

## State Variables

-   **`FUNC`**:
    -   Reference to the FUNC ERC20 token contract.
-   **`router`**:
    -   Reference to the Router contract for accessing provider and staking information.

---

## Constructor

### `constructor`

Initializes the Treasury contract with the FUNC token and Router contract addresses.

-   **Parameters**:

    -   `_FUNC`: Address of the FUNC ERC20 token contract.
    -   `_router`: Address of the Router contract.

-   **Access Control**:
    -   Grants the deployer the `DEFAULT_ADMIN_ROLE`.

---

## Key Functions

### Reward Distribution

#### `rewardProvider`

Distributes FUNC rewards to a provider.

-   **Access**:

    -   Only callable by the admin or the staking contract for the provider's model.

-   **Parameters**:

    -   `_id`: Unique identifier of the provider.
    -   `_rewardAmount`: Amount of FUNC tokens to reward the provider.

-   **Process**:

    1. Retrieves the provider's model ID using the Router.
    2. Ensures the caller is authorized (either the admin or the staking contract for the model).
    3. Fetches the provider's owner address from the Router.
    4. Transfers the specified reward amount to the provider's owner.
    5. Emits a `ProviderRewarded` event upon successful transfer.

-   **Events**:
    -   **`ProviderRewarded(bytes _id, address indexed providerOwner, uint256 rewardAmount)`**:
        -   Emitted when a provider successfully receives their reward.

---

## Events

-   **`ProviderRewarded(bytes _id, address indexed providerOwner, uint256 rewardAmount)`**:
    -   Logs the distribution of rewards to a provider.

---

## Reentrancy Protection

The contract implements the `ReentrancyGuard` modifier to prevent reentrancy attacks during reward distribution.

---

## Security Considerations

-   Only authorized entities (admin or staking contracts) can distribute rewards.
-   Ensures the recipient's owner address is valid before transferring tokens.
-   Prevents reward distribution with zero amounts to avoid unnecessary operations.

---

## Fallback Functions

-   **`receive()`**: Not implemented; rejects Ether transfers.
-   **`fallback()`**: Not implemented; rejects Ether transfers and invalid calls.
