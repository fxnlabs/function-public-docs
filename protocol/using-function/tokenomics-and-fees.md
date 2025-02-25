## Tokenomics

#### **FUNC Token Utility**

1. **Staking:** FUNC tokens must be staked by providers and gateways to participate in the network.
2. **Rewards:** FUNC tokens are distributed to staked participants based on their contributions to the network.
3. **Governance:** FUNC tokens may be used for proposal voting and treasury management in the future.

#### **Earning Potential**

- Providers and gateways earn FUNC based on the shards (work) they contribute during each epoch.
- **Example:**
  - If the total reward pool for an epoch is 10,000 FUNC and a provider contributes 10% of the shards, they earn 1,000 FUNC.

#### **Economic Design**

1. **Fixed Supply:**
   - FUNC tokens have a fixed supply, with no inflationary mechanisms, ensuring scarcity and value retention.
2. **Treasury Management:**
   - FUNC tokens in the treasury are allocated for ecosystem development, community initiatives, and network rewards.

## Understanding Fees in the FUNC Ecosystem

The FUNC ecosystem implements a dynamic mechanism for calculating burn amounts based on consumed Compute Units (CU). This mechanism ensures fairness by rewarding users who stake more FUNC tokens or commit to longer staking durations. Below is an overview of how burn amounts are determined and the various discount mechanisms available.

> [!IMPORTANT]  
> Burn is disabled on network launch and for the foreseeable future

### Fee Calculation

The fee is the FUNC tokens deducted based on consumed Compute Units (CU). It is calculated as:

```
Fee = CU Consumed × Fee Multiplier × Conversion Factor
```

Where:

- **Fee Multiplier**: Determined dynamically based on stake size and staking duration.
- **Conversion Factor**: Adjusts the relationship between CU and FUNC, defined by a configurable numerator and denominator:
  - Conversion = `(CU × Denominator) / Numerator`
  - This ensures flexibility in setting CU-to-FUNC ratios, which may vary based on network needs.

### Types of Discounts

1. **Stake-Based Discounts**

   - Users with higher FUNC stakes qualify for lower fees.
   - Discount tiers define minimum stake levels and their associated fees (expressed as percentages).
   - Example:
     - Stake ≥ 5000 FUNC: Fee = 95%
     - Stake ≥ 1000 FUNC: Fee = 97%
     - Stake < 1000 FUNC: Fee = 100% (no discount)

2. **Duration-Based Discounts**
   - Users who stake FUNC for longer durations qualify for additional discounts.
   - Discount tiers define minimum staking durations (measured in epochs) and their associated discounts.
   - Example:
     - Staked for ≥ 180 epochs (6 months): Discount = 5%
     - Staked for ≥ 30 epochs (1 month): Discount = 2%

### Additive Discount Mechanism

The total fee multiplier applied to the Fee is calculated as:

```
Final Fee = Stake-Based Fee - Duration Discount
```

- **Stake-Based Fee**: Based on the user’s FUNC stake amount.
- **Duration Discount**: Based on the user’s staking duration (in epochs).
- The final fee cannot go below 0%.

### Example Calculation

#### User Parameters:

- **Compute Units Consumed**: 100 CU
- **FUNC Staked**: 5000 FUNC (qualifies for a 95% fee).
- **Staking Duration**: 6 months (qualifies for a 5% discount).
- **Conversion Factor**: Numerator = 1, Denominator = 2 (1 CU Costs 2 FUNC).

#### Calculation:

1. **Stake-Based Fee**: 95% (9500 basis points).
2. **Duration Discount**: 5% (500 basis points).
3. **Final Fee**: 95% - 5% = 90% (9000 basis points).

4. **Fee**:
   ```
   Fee = (CU Consumed × Final Fee In Basis × Denominator) / (Numerator × Basis Denominator)
               = (100 × 9000 × 2) / (1 × 10000)
               = 180 FUNC
   ```

### Key Takeaways

1. **Flexible Conversion**: CU and FUNC are not always 1:1. The conversion factor (numerator and denominator) allows adjustments to the CU-to-FUNC ratio to meet network needs.
2. **Stake-Based Discounts**: Encourages users to stake more FUNC by reducing fees for higher stakes.
3. **Duration-Based Discounts**: Rewards users who commit to longer staking periods.
4. **Fair Mechanism**: The additive discount approach ensures fairness, combining discounts without compounding.

This mechanism incentivizes greater participation and long-term commitment, promoting network stability and security.
