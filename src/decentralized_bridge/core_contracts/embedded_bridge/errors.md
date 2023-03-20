# Bridge Embedded Error States

[Bridge error states](https://github.com/HyperCore-Team/go-zenon/blob/vm/constants/errors.go).

```go
ErrUnknownNetwork                       = errors.New("unknown network")
ErrInvalidToAddress                     = errors.New("invalid destination address")
ErrBridgeNotInitialized                 = errors.New("bridge info is not initialized")
ErrOrchestratorNotInitialized           = errors.New("orchestrator info is not initialized")
ErrTokenNotBridgeable                   = errors.New("token not bridgeable")
ErrNotGuardian                          = errors.New("sender is not a guardian")
ErrTokenNotRedeemable                   = errors.New("token not redeemable")
ErrBridgeHalted                         = errors.New("bridge is halted")
ErrInvalidRedeemPeriod                  = errors.New("invalid redeem period")
ErrInvalidRedeemRequest                 = errors.New("invalid request")
ErrInvalidTransactionHash               = errors.New("invalid transaction hash")
ErrInvalidNetworkName                   = errors.New("invalid network name")
ErrInvalidContractAddress               = errors.New("invalid contract address")
ErrInvalidToken                         = errors.New("invalid token standard or token address")
ErrTokenNotFound                        = errors.New("token not found")
ErrInvalidEDDSASignature                = errors.New("invalid ed25519 signature")
ErrInvalidEDDSAPubKey                   = errors.New("invalid eddsa public key")
ErrInvalidECDSASignature                = errors.New("invalid secp256k1 signature")
ErrInvalidDecompressedECDSAPubKeyLength = errors.New("invalid decompressed secp256k1 public key length")
ErrInvalidCompressedECDSAPubKeyLength   = errors.New("invalid compressed secp256k1 public key length")
ErrNotAllowedToChangeTss                = errors.New("changing the tss public key is not allowed")
ErrInvalidJsonContent                   = errors.New("metadata does not respect the JSON format")
ErrInvalidMinAmount                     = errors.New("invalid min amount")
ErrTimeChallengeNotDue                  = errors.New("time challenge not due")
ErrNotEmergency                         = errors.New("bridge not in emergency")
ErrInvalidGuardians                     = errors.New("invalid guardians")
ErrSecurityNotInitialized               = errors.New("security not initialized")
ErrBridgeNotHalted                      = errors.New("bridge not halted")
```
