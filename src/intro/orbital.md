# Orbital Program

**Orbital Program** is a **protocol level liquidity program** that **incentivizes** liquidity providers to participate and pool together liquidity for the wrapped representations of `ZNN`, `QSR` and `ZTS` tokens in exchange for the aforementioned fraction of the dual-coin emission. This program will create a sustainable model for external liquidity providers to participate into the network.

Without proper incentives, LPs may lack interest to join the Orbital Program and this in turn will hurt the ecosystem by limiting the potential of external users that would swap their assets to NoM. Moreover, beyond Orbital Program, new untapped markets and gateways to access NoM are necessary for a healthy development of the ecosystem.

## What is the liquidity embedded contract?

The liquidity embedded contract is the initial placeholder that **accumulates** dual-coin rewards for the upcoming Orbital Program. The liquidity contract was upgraded to enable the Orbital Program and is based on the staking embedded contract.

## How to participate in the Orbital Program?

A liquidity provider that wants to participate in the Orbital Program must supply a liquidity pool with the corresponding assets, swap the resulting LP token to NoM and lock it up into the liquidity embedded in order to be **eligible** for dual-coin rewards.

For example, several Ethereum users wants to become a liquidity providers (LPs). They must deposit `ETH` and `wZNN` into a pool in order to participate into the Orbital Program. Depending on how much liquidity they provide for the `wZNN/ETH` pool on Uniswap, they will receive a proportional amount of `ERC-20 LP tokens`. In the next step, the LP must swap the `ERC-20 LP tokens` to the `ZTS zLP tokens` (`zLP`) on NoM using the decentralized cross-chain bridge. In order to become eligible for receiving dual-coin rewards from NoM's emission, the LP must deposit the `zLP tokens` into the liquidity embedded contract. The liquidity embedded contract mirrors the *staking embedded contract* in terms of *locking* the tokens for a *predefined time period*. However, it differs from a rewards' perspective: participating LPs get both `ZNN` and `QSR`.
