# Logic Contract Documentation

The `Logic` contract is responsible for handling reward calculations and conversions between FUNC tokens and Compute Units (CU) in a blockchain-based ecosystem. It provides functionality to determine FUNC burn amounts, available compute units, and allows for configurable conversion ratios, fee structures, and duration-based discounts.

---

## Key Features

1. **Conversion Logic**:

    - Calculates FUNC burn amounts based on consumed compute units (CU) and dynamic fee structures.
    - Converts FUNC staked amounts into available compute units.

2. **Configurable Tiers**:

    - Allows dynamic fee structures based on staking tiers.
    - Provides additional discounts for long-term staking using epoch-based duration tiers.

3. **Configurable Ratios**:

    - Allows updating of the stake-to-CU conversion ratio via admin-controlled functions.

4. **Access Control**:
    - Utilizes role-based access control for secure administration.

---

## Contract Roles

### Access Control

The contract uses `AccessControl` to restrict access to critical functions.

-   **`DEFAULT_ADMIN_ROLE`**:
    -   Grants full control over administrative functions like updating the stake-to-CU ratio and configuring tiers.
-   **`GOVERNANCE_ROLE`**:
    -   Provides governance with permission to update configuration parameters.

---

## State Variables

-   **`getStakeToCUNumerator`**:

    -   Numerator of the stake-to-CU conversion ratio.
    -   Default value: `1`.

-   **`getStakeToCUDenominator`**:

    -   Denominator of the stake-to-CU conversion ratio.
    -   Default value: `1`.

-   **`stakeTiers`**:

    -   Array of stake tiers defining the minimum stake required and the corresponding final fee (percentage in basis points).

-   **`epochDurationTiers`**:

    -   Array of duration tiers defining the minimum epochs required and the corresponding discount (percentage in basis points).

-   **`minEpochsRequired`**:
    -   The global minimum epochs required for staking eligibility to access lower fees.

---

## Key Functions

### Constructor

#### `constructor()`

Initializes the contract with default values and assigns the `DEFAULT_ADMIN_ROLE` to the deployer.

-   **Default Values**:
    -   `getStakeToCUNumerator`: `1`
    -   `getStakeToCUDenominator`: `1`

---

### Conversion Functions

#### `getBurnAmountByCU(uint256 _computeUnits, uint256 _stakedAmount, uint256 _currentEpoch, uint256 _stakeEpoch) -> uint256`

Converts compute units (CU) into the equivalent FUNC burn amount. Incorporates the dynamic fee structure and duration discounts.

-   **Parameters**:
    -   `_computeUnits`: The number of compute units.
    -   `_stakedAmount`: The amount of FUNC staked.
    -   `_currentEpoch`: The current epoch.
    -   `_stakeEpoch`: The epoch when staking started.
-   **Returns**:
    -   FUNC amount equivalent to the specified compute units after applying the fee structure.

---

#### `getAvailableCUByStakeAmount(uint256 _amount, uint256 _currentEpoch, uint256 _stakeEpoch) -> uint256`

Converts FUNC staked amounts into available compute units (CU), factoring in the dynamic fee structure.

-   **Parameters**:
    -   `_amount`: The FUNC amount staked.
    -   `_currentEpoch`: The current epoch.
    -   `_stakeEpoch`: The epoch when staking started.
-   **Returns**:
    -   Compute units equivalent to the specified FUNC amount after applying the fee structure.

---

#### `getFinalFee(uint256 _stakedAmount, uint256 _currentEpoch, uint256 _stakeEpoch) -> uint256`

Calculates the final fee percentage for a given stake amount and staking duration. Discounts are combined additively instead of being compounded.

-   **Parameters**:
    -   `_stakedAmount`: The amount of FUNC staked.
    -   `_currentEpoch`: The current epoch.
    -   `_stakeEpoch`: The epoch when staking started.
-   **Returns**:
    -   Final fee percentage (in basis points) after applying the dynamic fee structure and duration discounts.

---

### Configuration Functions

#### `setStakeTiers(uint256[] calldata _minStakes, uint256[] calldata _finalFees)`

Sets the stake tiers for fee calculation. Tiers must be in descending order of `minStakes` and `finalFees`.

-   **Parameters**:
    -   `_minStakes`: Array of minimum stake amounts for each tier.
    -   `_finalFees`: Array of final fees (in basis points) for each tier.
-   **Requirements**:
    -   Arrays must have the same length.
    -   Arrays must be in descending order.

---

#### `setEpochDurationTiers(uint256[] calldata _minEpochs, uint256[] calldata _discounts)`

Sets the duration tiers for additional discounts based on staking duration. Tiers must be in descending order of `minEpochs` and `discounts`.

-   **Parameters**:
    -   `_minEpochs`: Array of minimum epochs for each tier.
    -   `_discounts`: Array of discount percentages (in basis points) for each tier.
-   **Requirements**:
    -   Arrays must have the same length.
    -   Arrays must be in descending order.

---

#### `setMinEpochsRequired(uint256 _minEpochsRequired)`

Updates the global minimum epochs required for staking eligibility to access lower fees.

-   **Parameters**:
    -   `_minEpochsRequired`: New minimum epochs required.

---

#### `setStakeToCUNumeratorDenominator(uint256 _stakeToCUNumerator, uint256 _stakeToCUDenominator)`

Updates the stake-to-CU conversion ratio.

-   **Parameters**:
    -   `_stakeToCUNumerator`: New numerator for the stake-to-CU ratio.
    -   `_stakeToCUDenominator`: New denominator for the stake-to-CU ratio.
-   **Requirements**:
    -   Both `_stakeToCUNumerator` and `_stakeToCUDenominator` must be non-zero.

---

## Events

This contract does not currently emit any events.

---

## Access Control Summary

| Function                           | Role Required    |
| ---------------------------------- | ---------------- |
| `setStakeTiers`                    | Admin/Governance |
| `setEpochDurationTiers`            | Admin/Governance |
| `setMinEpochsRequired`             | Admin/Governance |
| `setStakeToCUNumeratorDenominator` | Admin/Governance |

---
