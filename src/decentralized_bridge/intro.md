# Decentralized Bridge

The purpose of the **Decentralized Bridge** is to connect different networks by **wrapping** and **unwrapping** assets.

**Decentralized Bridge** components:

1. Bridge embedded contract on NoM
2. Orchestrator Layer as middleware
3. Smart Contracts on bridged network

## EVM account model

At the moment, only **account model EVM networks** are supported, starting with the *Ethereum mainnet*.

### Wrap assets

Source *NoM* -> Destination *EVM*

* `ZNN` (`ZTS` on *NoM*) -> **wrap** -> `wZNN` (`ERC-20` on *Ethereum*)

Source *EVM* -> Destination *NoM*

* `DAI` (`ERC-20` on *Ethereum*) -> **wrap** -> `zDAI` (`ZTS` on *NoM*)

### Unwrap assets

Source *EVM* -> Destination *NoM*

* `wZNN` (`ERC-20` on *Ethereum*) -> **unwrap** -> `ZNN` (`ZTS` on *NoM*)

Source *NoM* -> Destination *EVM*

* `zDAI` (`ZTS` on *NoM*) -> **unwrap** -> `DAI` (`ERC-20` on *Ethereum*)
