# Connectors

Each Orchestrator Node is connected to both NoM and the supported destination network using the `JSON-RPC API`. There are several types of connectors that provide different degrees of confidence for transaction finality.

## Local Connectors

## Full Node

The ideal configuration for an Orchestrator Node is to run its own full nodes for all supported networks. This way, it has a precise overview of the network state and does not rely to any third party that can be either maliciously corrupted or censored. Running full nodes also can detect chain reorganizations called `reorgs`.

Example of chain reorgs for a Bitcoin full node running continuously since 17 December 2016:

- Actual reorgs: 4 (1 every 84,352 blocks / 570.3 days)
- Avoided reorgs: 174 (1 every 1,939 blocks / 13.1 days)

## Light Node

Light Nodes decrease the load required to run a full node. Some implementations are better than others and if properly used can effectively displace the need for a full node.

## Remote Connectors

### Decentralized RPC Providers

Decentralized RPC Providers:

- [Ankr](https://www.ankr.com/)
- [Pockt Network](https://www.pokt.network/)

Decentralized RPC Providers should only be used for redundancy purposes.

### Centralized RPC Providers

Centralized RPC Providers:

- Infura
- Alchemy
- Quicknode

Centralized RPC Providers should only be used for redundancy purposes.
