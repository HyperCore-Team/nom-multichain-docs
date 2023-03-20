# Liquidity Embedded Contract

The **Liquidity Embedded Contract** is the embedded contract for the **Orbital Program** deployed on the Network of Momentum.

## Contract implementation

The implementation of the **Liquidity Embedded Contract** has a total of 14 methods that can be called by sending a specifically crafted account block:

### `UpdateEmbeddedLiquidityMethod` method

Method for updating the embedded liquidity contract.

#### Parameters

- None

#### Returns

- None

### `SetTokenTupleMethod` method

Method for setting a token tuple and their corresponding reward percentages for the Orbital Program. Can only be set by the `administrator`.

#### Parameters

- `TokenStandards` - an array containing the `ZTS` allowed for staking
- `ZnnPercentages` - an array containing the `ZNN` percentage rewards for each `ZTS` in the `TokenStandards` array
- `QsrPercentages` - an array containing the `QSR` percentage rewards for each `ZTS` in the `TokenStandards` array
- `MinAmounts` - an array containing the minimum amounts for staking each `ZTS` in the `TokenStandards` array

#### Returns

- None

### `LiquidityStakeMethod` method

Method for staking the liquidity for the Orbital Program.

#### Parameters

- `stakeTime` - stake time in seconds that should be a multiple of number in seconds for a month
- `tokenStandard` - fetched from the send block
- `amount` - fetched from the send block

#### Returns

- None

### `CancelLiquidityStakeMethod` method

Method for cancelling the liquidity stake for the Orbital Program.

#### Parameters

- `id` - a hash that uniquely identifies the stake entry to be canceled

#### Returns

- None

### `UpdateRewardEmbeddedLiquidityMethod` method

Method to be called when the Liquidity Contract is updated. It contains the new mechanism for distributing dual-coin rewards.

#### Parameters

- None

#### Returns

- None

### `SetIsHalted` method

Method for halting or unhalting the liquidity staking. Can only be called by the `administrator`.

#### Parameters

- `bool` - `true` or `false`

#### Returns

- None

### `UnlockLiquidityStakeEntries` method

Method for unlocking the liquidity staking entries for a `ZTS` that is no longer allowed for staking.

#### Parameters

- `tokenStandard` - fetched from the send block

#### Returns

- None

### `SetAdditionalReward` method

Method for setting an additional reward for the liquidity staking. Can only be called by the `administrator`.

#### Parameters

- `znnRewards` - additional `ZNN` rewards for the next epochs
- `qsrRewards` - additional `QSR` rewards for the next epochs

#### Returns

- None

### `ChangeAdministratorLiquidity` method

Method for changing the `administrator` for the liquidity embedded contract. Can only be called by the `administrator`. Guarded by a time challenge.

#### Parameters

- `address` - address of the `new` administrator

#### Returns

- None

### `NominateGuardiansLiquidity` method

Method for nominating the `guardians` for the liquidity embedded contract. Can only be called by the `Administrator`. Guarded by a time challenge.

#### Parameters

- `[]address` - an array containing the new addresses

#### Returns

- None

### `ProposeAdministratorLiquidity` method

Method for proposing a new `administrator` for the liquidity embedded contract. Can only be called by a `guardian` if the embedded liquidity contract is in an emergency state.

#### Parameters

- `address` - voted address

#### Returns

- None

### `EmergencyLiquidity` method

Method for putting the liquidity embedded contract into emergency mode. Can only be called by the `administrator`.

#### Parameters

- None

#### Returns

- None
