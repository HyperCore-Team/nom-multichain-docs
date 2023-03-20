# Liquidity Embedded JSON-RPC API

The `JSON-RPC API` for the liquidity embedded contract.

## JSON-RPC API Methods

### `GetLiquidityInfo` method

Returns the current liquidity information or `error` in case it fails.

#### Parameters

- None

#### Returns

- `LiquidityInfo` - the current bridge information

```go
type LiquidityInfo struct {
	Administrator types.Address `json:"administrator"`
	IsHalted      bool          `json:"isHalted"`
	ZnnReward     *big.Int      `json:"znnReward"`
	QsrReward     *big.Int      `json:"qsrReward"`
	TokenTuples   []TokenTuple  `json:"tokenTuples"`
}
```

#### Example

```js
// todo
```

### `GetSecurityInfo` method

Returns the current security information or `error` in case it fails.

#### Parameters

- None

#### Returns

- `SecurityInfoVariable` - the current security information

```go
// SecurityInfoVariable This refers to time challenge security
type SecurityInfoVariable struct {
	// addresses that can vote for the new administrator once the bridge is in emergency
	Guardians []types.Address `json:"guardians"`
	// votes of the active guardians
	GuardiansVotes []types.Address `json:"guardiansVotes"`
	// delay upon which the new administrator or guardians will be active
	AdministratorDelay uint64 `json:"administratorDelay"`
	// delay upon which all other time challenges will expire
	SoftDelay uint64 `json:"softDelay"`
}
```

#### Example

```js
// todo
```

### `GetLiquidityStakeEntriesByAddress` method

Returns the current liquidity staking entries by `address` or `error` in case it fails.

#### Parameters

- `address` - the address in `Address` format
- `pageIndex` - the page index in `uint32` format
- `pageSize` - the size of the page in `uint32` format

#### Returns

- `LiquidityStakeList` - the current liquidity staking list

```go
type LiquidityStakeList struct {
	TotalAmount         *big.Int                          `json:"totalAmount"`
	TotalWeightedAmount *big.Int                          `json:"totalWeightedAmount"`
	Count               int                               `json:"count"`
	Entries             []*definition.LiquidityStakeEntry `json:"list"`
}
```

#### Example

```js
// todo
```

### `GetUncollectedReward` method

Returns the `RewardDeposit` for a particular `address` or `error` in case it fails.

#### Parameters

- `address` - the address in `Address` format

#### Returns

- `RewardDeposit` - the current uncollected reward deposit

```go
type RewardDeposit struct {
	Address *types.Address `json:"address"`
	Znn     *big.Int       `json:"znnAmount"`
	Qsr     *big.Int       `json:"qsrAmount"`
}
```

#### Example

```js
// todo
```

### `GetFrontierRewardByPage` method

Returns the `RewardHistoryList` for a particular `address` or `error` in case it fails.

#### Parameters

- `address` - the address in `Address` format
- `pageIndex` - the page index in `uint32` format
- `pageSize` - the size of the page in `uint32` format

#### Returns

- `RewardHistoryList` - the current reward history list

```go
type RewardHistoryList struct {
	Count int64                 `json:"count"`
	List  []*RewardHistoryEntry `json:"list"`
}
```

#### Example

```js
// todo
```

### `GetTimeChallengesInfo` method

Returns a list of time challenges or `error` in case it fails.

#### Parameters

- None

#### Returns

- `TimeChallengesList` - the current a list of time challenges

```go
type TimeChallengesList struct {
	Count int                             `json:"count"`
	List  []*definition.TimeChallengeInfo `json:"list"`
}
```

```go
type TimeChallengeInfo struct {
	MethodName           string
	ParamsHash           types.Hash
	ChallengeStartHeight uint64
}
```

#### Example

```js
// todo
```
