# EVM Smart Contracts

The EVM smart contracts are deployed on the EVM networks integrated into the NoM Multi-chain Infrastructure.

## Contract Anatomy

The code is written in `Solidity v0.8.19` (latest as March 2023). 

### Imports

The smart contract uses the following `openzeppelin` standard contracts:

```js
import "@openzeppelin/contracts/utils/Context.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import "@openzeppelin/contracts/utils/cryptography/ECDSA.sol";
```

- `using ECDSA for bytes32` - used to add the `recover` ECDSA signature functionality

- `using SafeERC20 for IERC20` - used to add the `safeTransfer` and `safeTransferFrom` functionalities

- `contract Bridge is Context` - inherit `_msgSender()` method to get the sender

### Modifiers

- `onlyAdministrator` can only be called by owner of the `administrator` private key

- `isNotHalted` can only be called if the bridge is not halted

### Structs

- `TokenInfo` is used to store information about the bridged tokens

- `RedeemInfo` is used for redeeming funds and information regarding time challenges

### Events

```js
event RegisteredRedeem(uint256 indexed nonce, address indexed to, address indexed token, uint256 amount);
event Redeemed(uint256 indexed nonce, address indexed to, address indexed token, uint256 amount);
event Unwrapped(address indexed from, address indexed token, string to, uint256 amount);
event Halted();
event Unhalted();
event RevokedRedeem(uint256 indexed nonce);
event PendingAdministrator(address indexed newAdministrator);
event SetAdministrator(address indexed newAdministrator, address oldAdministrator);
event PendingTss(address indexed newTss);
event SetTss(address indexed newTss, address oldTss);
event PendingGuardians();
event SetGuardians();
```

- `RegisteredRedeem` emitted when a redeem request is registered

- `Redeemed` emitted when a redeem is completed

- `Unwrapped` emitted when an unwrap request is registered

- `Halted` emitted when the bridge is halted

- `Unhalted` emitted when the bridge is unhalted

- `RevokedRedeem` emitted when the administrator revokes an invalid redeem

- `PendingAdministrator` emitted when a request for changing the administrator address is made

- `SetAdministrator` emitted when the administrator address is set

- `PendingTss` emitted when a request for changing the tss address is made

- `SetTss` emitted when the tss address is set

- `PendingGuardians` emitted when a request for changing the guardian addresses is made

- `SetGuardians` emitted when the addresses of the guardians are set

### Constants

- `uint256 constant uint256max` stores `type(uint256).max`

- `uint32 private constant networkClass` for EVM networks is hard coded to `2`

- `uint8 private constant  minNominatedGuardians` must coincide with the `MinGuardians` constant from the embedded contract and is hard coded to `5`

### Variables

- `estimatedBlockTime` is set for each network depending on the block time interval; it is used by the `orchestrator` to know how much to wait for one confirmation

- `confirmationsToFinality` is the number of confirmations required to achieve finality for a given network; it is used by the `orchestrator` to confirm an event

- `halted` indicates if the bridge is halted or not

- `allowKeyGen` indicates wether or not the tss address can be changed with a valid signature; the `admin` can always change the tss address

- `administrator` is the EVM compatible address of the `admin`

- `administratorDelay` is the delay required for changing the `admin`; it cannot be lower than `minAdministratorDelay`

- `minAdministratorDelay` is the minimum delay required for changing the `admin`

- `tss` is the TSS address jointly created during the key generation ceremony

- `softDelay` is the delay required for the time challenge security primitive; it cannot be lower than `minSoftDelay`

- `minSoftDelay` is the minimum delay required for the time challenge security primitive

- `guardians` is the array containing the addresses of the guardians

- `nominatedGuardians` is the array containing the addresses of the nominated guardians

- `guardiansVotes` is the array containing the votes for the guardians

- `votesCount` is a mapping containing the proposed `admin` votes of each guardian

- `unhaltedAt` is the last block height at which the bridge was unhalted

- `unhaltDuration` is the duration in blocks during which the bridge is still halted after the `unhaltedAt` block height; it cannot be lower than `minUnhaltDuration`

- `minUnhaltDuration` is the minimum duration in blocks during which the bridge is still halted after the `unhaltedAt` block height

- `actionsNonce` is the nonce required for a valid signature at a particular state of the bridge; must be always incremented after a signature is validated

- `contractDeploymentHeight` is the block height at which the contract was deployed; it is used by the `orchestrator` to know the height to scan events

### Contract implementation

#### `Constructor`

In the constructor we set the following variables:

- `administrator`
- `minUnhaltDuration`
- `unhaltDuration`
- `minAdministratorDelay`
- `administratorDelay`
- `minSoftDelay`
- `softDelay`
- `guardians`
- `guardiansVotes`
- `estimatedBlockTime`
- `confirmationsToFinality`
- `contractDeploymentHeight`

### redeem method

The first and the second step of the redeem process. During the first step a time challenge will start. During the second step, after `softDelay`, the funds will be released/minted. The bridge must not be halted, the request must not have been revoked before, the token should be `redeemable` and the TSS signature valid.

#### Parameters

- `to` - EVM address that will receive the funds
- `token` - `EVM-20` token address
- `amount` - amount of token
- `nonce` - unique identifier of the redeem request
- `signature`  - signature generated by the current TSS

#### Returns

- None

### unwrap method

Register an unwrap request. Tokens will be locked/burned. The bridge must not be halted and the token must be `bridgeable`.

#### Parameters

- `token` - `EVM-20` token address
- `amount` - amount of token
- `to` - NoM address

#### Returns

- None

### setTokenInfo method

Adds or edits an existing `tokenInfo`. It contains information about permitted tokens to swap or redeem and their delays. Can be called only by the `administrator`. Guarded by a time challenge.

#### Parameters

- `token` - `EVM-20` token address
- `minAmount` - minimum amount of the token for unwrapping
- `redeemDelay` - delay in blocks after the first redeem step in order to receive the funds
- `bridgeable` - whether the token can be unwrapped
- `redeemable` - whether the token can be redeemed
- `isOwned` - whether this contract has owner rights for the `EVM-20`

#### Returns

- None

### halt method

Halts the network. It does not require a signature if called by the `administrator`. Otherwise, a TSS signature is needed.

#### Parameters

- `signature` - current TSS signature

#### Returns

- None

### unhalt method

Sets halted to `false` and updates the `haltedAt` block such that `unhaltDelay` starts. Can only be called by the `administrator`.

#### Parameters

- None

#### Returns

- None

### revokeRedeems method

Revokes invalid redeem requests.

#### Parameters

- `nonces` - an array containing the unique identifiers for each request

#### Returns

- None

### setAdministrator method

Changes the `administrator` address. Can only be called by the `administrator`. Guarded by a time challenge.

#### Parameters

- `newAdministrator` - `new` administrator address

#### Returns

- None

### setTss method

Changes the TSS address. If called by the `administrator`, no signature is required and the time challenge starts. Otherwise, a valid TSS signature is required in order to validate the new address.

#### Parameters

- `newTss` - `new` TSS address
- `oldSignature` - signature from the current TSS
- `newSignature` - signature from the new TSS

#### Returns

- None

### emergency method

Sets administrator to `address(0)`, TSS to `address(0)` and halts the bridge. Can only be called by the `administrator`. It enables `guardians` to propose.

#### Parameters

- None

#### Returns

- None

### nominateGuardians

Nominate the `guardians` that are responsible to propose a new administrator in case of an emergency. Guarded by a time challenge. Can only be called by the `administrator`.

#### Parameters

- `newGuardians` - an array containing the new `guardian` addresses

#### Returns

- None

### proposeAdministrator method

Will vote for a new `administrator` address. Can only be called by a `guardian` only if the bridge is in emergency state (the address of the `administrator` is `address(0)`).

#### Parameters

- `newAdministrator` - new `administrator` address

#### Returns

- None

### setSoftDelay method

Sets the delay for the time challenge. Can only be called by the `administrator`.

#### Parameters

- `delay` - delay in blocks

#### Returns

- None

### setUnhaltDuration method

Sets the delay during which the bridge remains halted after calling `unhalt`. Can only be called by the `administrator`.

#### Parameters

- `duration` - duration in blocks

#### Returns

- None

### setEstimatedBlockTime method

Sets the delay in which the bridge remains halted after calling `unhalt`. Can only be called by the `administrator`.

#### Parameters

- `blockTime` - block time in seconds; it is used by the `orchestrator` layer

#### Returns

- None

### setAllowKeyGen method

Sets `allowKeyGen` to true that permits the TSS address to be changed by an address different from the `administrator`. Can only be called by the `administrator`.

#### Parameters

- `value` - `true` or `false`

#### Returns

- None

### setConfirmationsToFinality method

Sets the confirmations required by an event to be confirmed. Used by the `orchestrator` layer. Can only be called by the `administrator`.

#### Parameters

- `confirmations` - number of confirmations in blocks

#### Returns

- None
