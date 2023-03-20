# Architecture Overview

The whole design architecture of the decentralized cross-chain infrastructure is focused around security considerations.

The possibility to exchange and swap assets in a trustless, decentralized way between NoM and other networks is conditioned by the existence of a decentralized bridge solution.

The liquidity embedded in its current form does not have the means to distribute rewards to liquidity providers in a trustless and automated way.

Not a single centralized entity except the end user is able to move funds and several security mechanisms are organized in a layered structure to prevent and deter malicious actors from exploiting the infrastructure. The TSS participants are only able to sign transactions; they do not have the possibility to move user funds.

There are three distinct components that will enable a multi-chain vision with Orbital Program at its core:

- **The Core Contracts**
- **The Protocol Level Liquidity**
- **The Orchestrator Layer**

The **Orchestrator Layer** will enable the relay and data transmissions between networks and will orchestrate the TSS signing ceremonies.

The **Core Contracts** will verify resulting signatures and interpret incoming events accordingly.

The **Protocol Level Liquidity** is a stand alone embedded contract that distributes the corresponding fraction of the dual-coin emission to eligible Liquidity Providers.

- **On-chain**
  - Core Contracts
  - Protocol Level Liquidity
- **Off-chain**
  - Orchestrator Layer: Orchestrator nodes based on Pillar nodes that form their own peer-to-peer network. Orchestrator nodes observe the Core Contract on each supported network and produce events that the Core Contracts validate. The orchestrator node will try to sign a confirmed event that was witnessed in the next TSS ceremony. The signing will succeed if there is a super-majority (`>66%`).

Unlike other bridge architectures, there is no additional peer-to-peer communication needed to confirm the event: each orchestrator node witness and processes all the events locally (after a finality threshold) and a successful TSS signing ceremony represents the off-chain consensus on which events are valid wraps and unwraps.

## Modus Operandi

The decentralized bridge is based on `lock-and-release` / `mint-and-burn` mechanisms: native assets are `locked` / `burned` on one side and `released` /`minted` on the other side.

This particular interoperability solution allows easy transfers from NoM to other networks. The embedded bridge contract allows the control of native ZTS assets on NoM, while EVM Solidity contracts allow the control of wrapped ZTS assets on EVM compatible chains.

Depending on the direction of the swap, the following scenarios may appear:

- Example native `ZNN/wZNN` (native `ZNN` and `wZNN` as `ERC-20` token):
Native `ZNN` is locked in the bridge embedded contract on NoM and its wrapped representation, `wZNN` is minted by the Ethereum contract. For the opposite direction, `wZNN` is burned by the Ethereum smart contract and native `ZNN` is released by the embedded bridge.

- Example stablecoin `DAI/zDAI` (`DAI` as `ERC-20` token and `zDAI` as `ZTS` token):

`DAI` stablecoin is locked in the Ethereum smart contract, while the bridge embedded contract mints the equivalent amount on NoM. For the opposite direction, `zDAI` is burned by the embedded contract and `ERC-20` `DAI` is released by the Ethereum smart contract.

## Compatibility

A robust interoperability solution must also *maximize* the *compatibility* with other distributed ledgers. Currently only NoM and EVM networks are supported. Other networks are based on different models and will be supported in the future.

### Account model compatibility

#### EVM compatibility

The initial phase will consist of an implementation specifically designed for **EVM** compatibility (*Ethereum*, *BNB Smart Chain*, etc.):

- Execution compatibility: `Ethereum virtual machine` (i.e. `Solidity`)
- Cryptographic compatibility: `ECDSA` signatures (i.e. `secp256k1`)
- API compatibility: `ETH JSON-RPC` (i.e. Ethereum `JSON-RPC API`)

This interoperability solution will unlock liquidity from a wide range of both `Layer-1` and `Layer-2` `EVM` compatible networks such as *Ethereum*, *BNB Smart Chain*, *Polygon*, *Avalanche*, *Arbitrum*, etc.

Liquidity pools will be gradually deployed on those networks for the wrapped representations of `ZTS` assets, enabling a seamless process for users to access and get onboarded into the NoM ecosystem.

#### Non-EVM compatibility

In the future, more **non-EVM** networks can be connected by implementing custom integrations (*Polkadot*, *Cosmos*, etc.):

- Execution compatibility: Virtual machine type (i.e. `WASM`)
- Cryptographic compatibility: `EdDSA` signatures (i.e. `Ed25519`)
- API compatibility: custom `API` for each network

### UTXO model compatibility

In the next phase, compatibility with **UTXO** networks (*Bitcoin*, *Litecoin*, etc.), in particular *Bitcoin*, is expected to be implemented and rolled-out. Further design changes are necessary for optimal integration with existent `BIP340`, `BIP341` (e.g. `Schnorr signatures` - `Musig2`, `MAST`, `Tapscript`, etc.).

## Confirmations to Finality

Different networks use different consensus protocols with different finality assumptions. In the table below one can find the number confirmations needed for a transaction to become final.

Orchestrator nodes proceed with the TSS signing ceremony only for finalized cross-chain transactions.

| Network Name        | Network Class     | Chain identifier | Finality |
| :------------------ | :---------------- | :--------------  | :--------|
| Network of Momentum | 1                 | 1                | 6 momentums        |
| Ethereum            | 2                 | 1                | [1 epoch](https://github.com/ethereum/consensus-specs/blob/dev/specs/phase0/beacon-chain.md) ~ 6.4 minutes   |
| Binance Smart Chain | 2                 | 56               | [33 seconds (6 secs after fast finality)](https://www.bnbchain.org/en/blog/bnb-chain-tech-roadmap-2023/)    |
| Bitcoin             | 3                 | 1                | 6 blocks ~ 60 minutes        |
---
