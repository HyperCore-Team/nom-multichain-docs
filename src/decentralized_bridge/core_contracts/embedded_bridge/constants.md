# Bridge Embedded Constants

[Bridge constants](https://github.com/HyperCore-Team/go-zenon/blob/vm/constants/embedded.go) are hard coded into the `embedded.go` file as network constants.

- `InitialBridgeAdministrator` is the hard-coded address for the administration module, managed by the maintainers of the codebase as `Address`

- `MaximumFee` is the maximum fee for a wrap request as `uint32`; equivalent to `100%` fee

- `MinUnhaltDurationInMomentums` is the minimum amount of time, measured in momentums, until the bridge functionality is restored from a halted state as `uint64`

- `MinAdministratorDelay` is the minimum delay required by the `time challenge` for `admin` operations as `uint64`

- `MinSoftDelay` is the minimum delay required by `time challenge` for all operations excluding the `admin` and the `guardians` as `uint64`

- `MinGuardians` is the minimum number of guardians required for the bridge to become operational as `unit32`

- `DecompressedECDSAPubKeyLength` is the size of the decompressed `ECDSA` public key in `bytes`

- `CompressedECDSAPubKeyLength`is the size of the compressed `ECDSA` public key in `bytes`

- `ECDSASignatureLength` is the size of the `ECDSA` signature in `bytes`

- `EdDSAPubKeyLength` is the size of the `EdDSA` public key in `bytes`

```go
InitialBridgeAdministrator   = types.ParseAddressPanic("z1qz8q0x3rs36z2kw8eltf8r323hlcn64jnujkuz")
MaximumFee                   = uint32(10000)
MinUnhaltDurationInMomentums = uint64(6 * MomentumsPerHour) //main net
MinAdministratorDelay        = uint64(2 * MomentumsPerEpoch) // main net
MinSoftDelay                 = uint64(MomentumsPerEpoch)     // main net
MinGuardians                 = 5
DecompressedECDSAPubKeyLength = 65
CompressedECDSAPubKeyLength   = 33
ECDSASignatureLength          = 65
EdDSAPubKeyLength             = 32
```
