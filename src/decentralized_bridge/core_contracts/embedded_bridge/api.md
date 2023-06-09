# Bridge Embedded JSON-RPC API

The `JSON-RPC API` for the bridge embedded contract.

## JSON-RPC API Methods

### `GetBridgeInfo` method

Returns the current bridge information or `error` in case it fails.

#### Parameters

- None

#### Returns

- `BridgeInfoVariable` - the current bridge information

```go
type BridgeInfoVariable struct {
	// Administrator address
	Administrator types.Address `json:"administrator"`
	// ECDSA pub key generated by the orchestrator from key gen ceremony
	CompressedTssECDSAPubKey   string `json:"compressedTssECDSAPubKey"`
	DecompressedTssECDSAPubKey string `json:"decompressedTssECDSAPubKey"`
	// This specifies whether the orchestrator should key gen or not
	AllowKeyGen bool `json:"allowKeyGen"`
	// This specifies whether the bridge is halted or not
	Halted bool `json:"halted"`
	// Height at which the administrator called unhalt method, UnhaltDurationInMomentums starts from here
	UnhaltedAt uint64 `json:"unhaltedAt"`
	// After we call the unhalt embedded method, the bridge will still be halted for UnhaltDurationInMomentums momentums
	UnhaltDurationInMomentums uint64 `json:"unhaltDurationInMomentums"`
	// An incremental nonce used for signing messages
	TssNonce uint64 `json:"tssNonce"`
	// Additional metadata
	Metadata string `json:"metadata"`
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

### `GetOrchestratorInfo` method

Returns the current orchestrator information or `error` in case it fails.

#### Parameters

- None

#### Returns

- `OrchestratorInfo` - the current orchestrator information

```go
type OrchestratorInfo struct {
	// Momentums period in which only one signing ceremony (wrap or unwrap) can occur in the orchestrator
	WindowSize uint64 `json:"windowSize"`
	// This variable is used in the orchestrator to wait for at least KeyGenThreshold participants for a key gen ceremony
	KeyGenThreshold uint32 `json:"keyGenThreshold"`
	// Momentums until orchestrator can process wrap requests
	ConfirmationsToFinality uint32 `json:"confirmationsToFinality"`
	// Momentum time
	EstimatedMomentumTime uint32 `json:"estimatedMomentumTime"`
	// This variable is a reference for the orchestrator to check the last 24h of momentums for producing pillars
	AllowKeyGenHeight uint64 `json:"allowKeyGenHeight"`
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

### `GetNetworkInfo` method

Returns the current network information or `error` in case it fails.

#### Parameters

- `networkClass` - network class in `uint32` format
- `chainId` - chain identifier in `uint32` format

#### Returns

- `NetworkInfo` - the current network information.

```go
// NetworkInfoVariable One network will always be znn, so we just need the other one
type NetworkInfoVariable struct {
	NetworkClass    uint32   `json:"networkClass"`
	Id              uint32   `json:"chainId"`
	Name            string   `json:"name"`
	ContractAddress string   `json:"contractAddress"`
	Metadata        string   `json:"metadata"`
	TokenPairs      [][]byte `json:"tokenPairs"`
}
```

#### Example

```js
// todo
```

### `GetAllNetworks` method

Returns a list of all the available networks.

#### Parameters

- `pageIndex` - the page index in `uint32` format
- `pageSize` - the size of the page in `uint32` format

#### Returns

- `NetworkInfoList` - a list of available networks

```go
type NetworkInfoList struct {
	Count int                       `json:"count"`
	List  []*definition.NetworkInfo `json:"list"`
}
```

#### Example

```js
// todo
```

### `getToken` method

Returns a token by `tokenStandard` or `error` in case it fails.

#### Parameters

- `zts` - the Zenon Token Standard, in `ZenonTokenStandard` format

#### Returns

- `Token` - the current token information

```go
type TokenInfo struct {
	Owner       types.Address `json:"owner"`
	TokenName   string        `json:"tokenName"`
	TokenSymbol string        `json:"tokenSymbol"`
	TokenDomain string        `json:"tokenDomain"`
	TotalSupply *big.Int      `json:"totalSupply"`
	MaxSupply   *big.Int      `json:"maxSupply"`
	Decimals    uint8         `json:"decimals"`
	IsMintable  bool          `json:"isMintable"`
	// IsBurnable = true implies that anyone can burn the token.
	// The Owner can burn the token even if IsBurnable = false.
	IsBurnable bool `json:"isBurnable"`
	IsUtility  bool `json:"isUtility"`

	TokenStandard types.ZenonTokenStandard `json:"tokenStandard"`
}
```

#### Example

```js
// todo
```

### `getRedeemableIn` method

Returns the number of momentums until an `UnwrapTokenRequest` becomes redeemable.

#### Parameters

- `unwrapTokenRequest` - the unwrap token request
- `tokenPair` - the token pair
- `momentum` - the momentum

#### Returns

- `redeemableIn` - the number of momentums until an `UnwrapTokenRequest` becomes redeemable in `uint64` format

```go
var redeemableIn uint64
```

#### Example

```js
// todo
```

### `getConfirmationsToFinality` method

Returns the number of confirmations to finality for a `WrapTokenRequest` or `error` in case it fails.

#### Parameters

- `wrapTokenRequest` - the wrap request for a particular token
- `confirmationsToFinality` - the number of confirmations required to achieve finality in `uint32` format
- `momentum` - the momentum

#### Returns

- `actualConfirmationsToFinality` - the number of confirmations to achieve finality on the destination network in `uint64` format

```go
var actualConfirmationsToFinality uint64
```

#### Example

```js
// todo
```

### `GetWrapTokenRequestById` method

Returns the token for the wrap request or `error` in case it fails.

#### Parameters

- `id` - the hash of the wrap request, in `Hash` format

#### Returns

- `wrapTokenRequest` - the wrap token request in `WrapTokenRequest` format

```go
type WrapTokenRequest struct {
	*definition.WrapTokenRequest
	TokenInfo               *api.Token `json:"token"`
	ConfirmationsToFinality uint64     `json:"confirmationsToFinality"`
}
```

#### Example

```js
// todo
```

### `GetAllWrapTokenRequests` method

Returns a list of tokens for all wrap requests or `error` in case it fails.

#### Parameters

- `pageIndex` - the page index in `uint32` format
- `pageSize` - the size of the page in `uint32` format

#### Returns

- `wrapTokenRequestList` - a list of wrap token requests

```go
type WrapTokenRequestList struct {
	Count int                 `json:"count"`
	List  []*WrapTokenRequest `json:"list"`
}
```

#### Example

```js
// todo
```

### `GetAllWrapTokenRequestsByToAddress` method

Returns a list of wrap requests that match a particular address or `error` in case it fails.

#### Parameters

- `pageIndex` - the page index in `uint32` format
- `pageSize` - the size of the page in `uint32` format
- `toAddress` - the address in `string` format

#### Returns

- `wrapTokenRequestList` - a list of wrap token requests

```go
type WrapTokenRequestList struct {
	Count int                 `json:"count"`
	List  []*WrapTokenRequest `json:"list"`
}
```

#### Example

```js
// todo
```

### `GetAllWrapTokenRequestsByToAddressNetworkClassAndChainId` method

Returns a list of wrap requests that match a particular address, network class and chain identifier.

#### Parameters

- `toAddress` - the address in `string` format
- `networkClass` - the network class
- `pageIndex` - the page index in `uint32` format
- `pageSize` - the size of the page in `uint32` format

#### Returns

- `wrapTokenRequestList` - a list of wrap token requests

```go
type WrapTokenRequestList struct {
	Count int                 `json:"count"`
	List  []*WrapTokenRequest `json:"list"`
}
```

#### Example

```js
// todo
```

### `GetAllUnsignedWrapTokenRequests` method

Returns a list of unsigned wrap requests or `error` in case it fails.

#### Parameters

- `pageIndex` - the page index in `uint32` format
- `pageSize` - the size of the page in `uint32` format

#### Returns

- `wrapTokenRequestList` - a list of unsigned wrap token requests

#### Example

```js
// todo
```

### `GetUnwrapTokenRequestByHashAndLog` method

Returns a list of unwrap token requests that match a particular `Hash` and `Log` or `error` in case it fails.

#### Parameters

- `txHash` - the transaction hash in `Hash` format
- `logIndex` - the log index in `uint32` format

#### Returns

- `unwrapTokenRequest` - the unwrap token request

```go
type UnwrapTokenRequest struct {
	RegistrationMomentumHeight uint64                   `json:"registrationMomentumHeight"`
	NetworkClass               uint32                   `json:"networkClass"`
	ChainId                    uint32                   `json:"chainId"`
	TransactionHash            types.Hash               `json:"transactionHash"`
	LogIndex                   uint32                   `json:"logIndex"`
	ToAddress                  types.Address            `json:"toAddress"`
	TokenAddress               string                   `json:"tokenAddress"`
	TokenStandard              types.ZenonTokenStandard `json:"tokenStandard"`
	Amount                     *big.Int                 `json:"amount"`
	Signature                  string                   `json:"signature"`
	Redeemed                   uint8                    `json:"redeemed"`
	Revoked                    uint8                    `json:"revoked"`
}
```

#### Example

```js
// todo
```

### `GetAllUnwrapTokenRequests` method

Returns a list of all unwrap token requests or `error` in case it fails.

#### Parameters

- `pageIndex` - the page index in `uint32` format
- `pageSize` - the size of the page in `uint32` format

#### Returns

- `unwrapTokenRequestList` - a list of unwrap token requests

#### Example

```js
// todo
```

### `GetAllUnwrapTokenRequestsByToAddress` method

Returns a list of all unwrap token requests that match a particular address.

#### Parameters

- `toAddress` - the address in `string` format
- `pageIndex` - the page index in `uint32` format
- `pageSize` - the size of the page in `uint32` format

#### Returns

- `unwrapTokenRequestList` - a list of unwrap token requests

#### Example

```js
// todo
```

### `GetFeeTokenPair` method

Returns the fee for a particular token pair or `error` in case it fails.

#### Parameters

- `zts` - the Zenon Token Standard, in `ZenonTokenStandard` format

#### Returns

- `ZtsFeesInfo` - ZTS fee information

```go
type ZtsFeesInfo struct {
	TokenStandard  types.ZenonTokenStandard `json:"tokenStandard"`
	AccumulatedFee *big.Int                 `json:"accumulatedFee"`
}
```

#### Example

```js
// todo
```
