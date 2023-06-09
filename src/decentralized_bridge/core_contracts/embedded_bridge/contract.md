# Bridge Embedded Contract

The **Bridge Embedded Contract** is the embedded contract for the **decentralized bridge** deployed on the Network of Momentum.

## Helper methods

### `CheckECDSASignature` method

Checks if the ECDSA signature is correct or returns `error` in case it fails.

#### Parameters

- `message` - the message in `[]byte` format
- `pubKeyStr` - the public key in `string` format
- `signatureStr` - the signature in `string` format

#### Returns

- `bool` - `true` if the signature passes, `false` otherwise

### `CanPerformAction` method

Checks if the parameters of the bridge are correct or returns `error` in case it fails.

#### Parameters

- `AccountVmContext` - current context of the VM

#### Returns

- `BridgeInfoVariable` - information regarding the bridge
- `OrchestratorInfo` - information regarding the orchestrator

### `CheckBridgeInitialized` method

Checks if the bridge is properly initialized or returns `error` in case it fails.

#### Parameters

- `AccountVmContext` - current context of the VM

#### Returns

- `BridgeInfoVariable` - information regarding the bridge

### `CheckOrchestratorInfoInitialized` method

Checks if the orchestrator is properly initialized or returns `error` in case it fails.

#### Parameters

- `AccountVmContext` - current context of the VM

#### Returns

- `OrchestratorInfo` - information regarding the orchestrator

### `CheckBridgeHalted` method

Checks if the bridge is halted or returns `error` in case it fails.

#### Parameters

- `BridgeInfoVariable` - information regarding the bridge
- `AccountVmContext` - current context of the VM

#### Returns

- `nil` - the bridge is not halted

## Contract implementation

The implementation of the **Bridge Embedded Contract** has a total of 20 methods that can be called by sending a specifically crafted account block:

### `WrapTokenMethod` method

Method for wrapping assets. If the bridge is initialized, not halted, the destination network and the token pair exists, than a wrap token request is created.

#### Parameters

- `NetworkClass` - the class of the destination network
- `ChainId` - the chain identifier of the destination network
- `ToAddress` - the address that can redeem the funds on the destination network
- `TokenStandard` - the `ZTS` used in the `send` block
- `Amount` - the amount used in the `send` block

#### Returns

- `AccountBlock` - only if the token is owned, a burn call to the token contract is performed that burns the received tokens

### `UpdateWrapRequestMethod` method

Method for updating the wrap request. If the `signature` is valid and is generated by the TSS public key, it is saved in the wrap request signature field and can be used to redeem the funds.

#### Parameters

- `Id` - represents the id of a wrap requests
- `Signature` - a base64 encoded ECDSA signature used to redeem funds on the destination network

#### Returns

- None

### `UnwrapTokenMethod` method

Method for unwrapping assets. It is the first step in redeeming funds on NoM. Normally this method would be called by an `orchestrator` node after a signing ceremony to register an unwrap request.

#### Parameters

- `NetworkClass` - the class of the source network
- `ChainId` - the chain identifier of the source network
- `TransactionHash` - hash of the transaction on the source network
- `LogIndex` - log index in the block of the transaction that locked/burned the funds on the source network; together with `txHash` it creates a unique identifier for a transaction
- `ToAddress` - destination NoM address
- `TokenAddress` - address of the locked/burned token on the source network
- `Amount` - amount of token that was locked/burned
- `Signature` - the signature as `base64` encoded string of the unwrap request

#### Returns

- None

### `SetNetworkMethod` method

Method for setting a network. Used to add or edit an already existing bridged network.

#### Parameters

- `NetworkClass` - class of the network
- `ChainId`- chain identifier of the network
- `Name`- `new` name of the network
- `ContractAddress` - `new` address the bridge contract was deployed to
- `Metadata` - `JSON` encoded string used in case the `orchestrator` needs additional data for the network

#### Returns

- None

### `RemoveNetworkMethod` method

Method for removing a network uniquely identified by its class and chain identifier.

#### Parameters

- `NetworkClass` - class of the network
- `ChainId`- chain identifier of the network

#### Returns

- None

### `SetNetworkMetadataMethod` method

Method for setting metadata for a network.

#### Parameters

- `NetworkClass` - class of the network
- `ChainId`- chain identifier of the network
- `Metadata` - `JSON` encoded string containing the `new` metadata

#### Returns

- None

### `SetTokenPairMethod` method

Method for setting a token pair for a network. It will add a new pair or edit an existing one; guarded by the time challenge primitive.

#### Parameters

- NetworkClass - network class of the network to add this pair to
- ChainId - chain identifier of the network to add this pair to
- TokenStandard - first component of the pair i.e. `ZTS`
- TokenAddress - second component of the pair i.e. `EVM-20`
- `Bridgeable` - boolean variable that specifies if the `ZTS` can be wrapped on NoM
- `Redeemable` - boolean variable that specifies if the `EVM-20` can be redeemed on NoM
- `Owned` - boolean variable that specifies if the embedded bridge contract is the owner of the `ZTS` from the pair; if true, when wrapping the ZTS, the token will be burned, otherwise it will be locked
- `MinAmount` - minimum amount for wrapping the `ZTS`
- `FeePercentage` - fee deducted from the amount when wrapping
- `RedeemDelay` - delay in momentums until redeeming is possible on NoM
- `Metadata` - `JSON` encoded string containing metadata about the pair; used by the `orchestrator`

#### Returns

- None

### `RemoveTokenPairMethod` method

Method for removing a token pair for a network.

#### Parameters

- `NetworkClass` - class of the network that contains the pair
- `ChainId` - chain identifier of the network that contains the pair
- `TokenStandard` - `ZTS` of the pair</li>
- `TokenAddress` - `EVM-20` address of the pair

#### Returns

- None

### `HaltMethod` method

Method for halting the bridge. If called by the `administrator` it doesn't need a `signature`, otherwise it needs a TSS signature with the current `tssNonce`.

#### Parameters

- `Signature` - `base64` encoded string containing the TSS signature

#### Returns

- None

### `UnhaltMethod` method

Method for unhalting the bridge. Can be called only by the `administrator`.

#### Parameters

- None

#### Returns

- None

### `EmergencyMethod` method

Method for putting the bridge embedded contract into emergency mode. This method nullifies the `administrator` address, tss public key and halts the bridge. It can be called in the extreme case when the `administrator` private key has been compromised.

#### Parameters

- None

#### Returns

- None

### `ChangeTssECDSAPubKeyMethod` method

Method for changing the TSS public key. Guarded by time challenge when called by the `administrator`. Otherwise it can be changed by the TSS using a signature from the current tss, one from the new tss and the new tss public key.

#### Parameters

- `PubKey` - `base64` encoded string representing the new TSS public key
- `OldPubKeySignature` - `base64` encoded string representing the `changeTss` message signed by the current TSS
- `NewPubKeySignature` - `base64` encoded string representing the `changeTss` message signed by corresponding private key

#### Returns

- None

### `ChangeAdministratorMethod` method

Method for changing the admin. Can be called only by the `administrator`. Guarded by a time challenge with `administratorDelay`.

#### Parameters

- `Address` - address of the new `administrator`

#### Returns

- None

### `SetAllowKeygenMethod` method

Method that enables or disables the TSS key generation.

#### Parameters

- `Bool` - `true` or `false` value that indicates whether the `orchestrator` nodes should start a key generation ceremony or not

#### Returns

- None

### `SetOrchestratorInfoMethod` method

Method for setting the information regarding the orchestrator layer.

#### Parameters

- `WindowSize` - size in momentums of a window used in the `orchestrator` to determine which signing ceremony should occur, wrap or unwrap request and to determine the key sign ceremony timeout
- `KeyGenThreshold` - minimum number of participants of a key generation ceremony
- `ConfirmationsToFinality` - minimum number of momentums to consider a wrap request confirmed
- `EstimatedMomentumTime` - time in seconds between momentums

#### Returns

- None

### `SetBridgeMetadataMethod` method

Method for setting the metadata for the bridge.

#### Parameters

- `String` - `JSON` encoded string containing additional information needed for the `orchestrator` layer

#### Returns

- None

### `RevokeUnwrapRequestMethod` method

Method for revoking an unwrap request. This should be called if an unwrap request does not a have corresponding transaction on the source network that locks/burns the tokens.

#### Parameters

- `TransactionHash` - first component to uniquely identify an unwrap request
- `LogIndex` - second component to uniquely identify an unwrap request

#### Returns

- None

### `RedeemMethod` method

Method for redeeming assets. It is the second step in order to redeem funds after an unwrap request is created. In order to redeem the unwrap request the following conditions must be present: the bridge must not be halted, the network and token pair must exist and the request have not been redeemed or revoked before.

#### Parameters

- `TransactionHash` - first component to uniquely identify an unwrap request
- `LogIndex` - second component to uniquely identify an unwrap request

#### Returns

- `AccountBlock` - if the `ZTS` to be redeemed is owned, an account block to mint the amount will be sent to the token contract, otherwise a block sending the tokens will be sent to the user

### `NominateGuardiansMethod` method

Method for nominating guardians. Can only be called by the `administrator` in order to propose new `guardians`. Guarded by a time challenge. The number of guardian addresses must be higher than `minGuardians` and each address in the array must be unique. `ZeroAddress` is not accepted.

#### Parameters

- `[]Address` - contains the new guardian addresses

#### Returns

- None

### `ProposeAdministratorMethod` method

Method for proposing an admin. This method can only be called by a `guardian` if the bridge is in emergency mode i.e. the administrator address is set to `ZeroAddress`. After each `guardian` votes for an address, the votes are counted and if there is a majority (`50% + 1`), the `administrator` will be set with that address.

#### Parameters

- `Address` - voted address

#### Returns

- None
