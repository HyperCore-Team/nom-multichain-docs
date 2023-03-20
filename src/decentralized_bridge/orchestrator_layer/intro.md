# Orchestrator Layer

The purpose of the **Orchestrator Layer** is to assure the *off-chain communication* and *TSS signing* of wrap and unwrap requests for the integrated networks.

## OS support

Currently only Linux based operating systems are supported. This is due to the fact that Pillar nodes already running on public infrastructure have dedicated support for Linux based operating systems.

| OS  | Linux | Windows | MacOS |
| --- | ---   | ---     | ---   |
|     | Yes   | No      | No    |

## Data directory

- `DefaultDataDir` is the default data directory for the orchestrator node located at `~/.orchestrator`

- `orchestrator` is executable file of the Orchestrator node

- `config.json` is the configuration file

## Configuration

### Producer key

The Pillar operator must set his `producer keyStore` in `~/.orchestrator` and provide the `name` and `passphrase` in `config.json`

After the first run, in `config.json`, from `producer key` an EVM address will automatically get generated and the field `evmAddress`  will be populated.

**IMPORTANT**: An `ETH` deposit is mandatory for the generated `evmAddress` that will be used as gas in case a `halt` transaction is necessary to be performed on-chain.

## Secrets

**IMPORTANT**: The Pillar operator must backup the `~/.orchestrator/tss` folder containing the generated TSS shard in case a hardware failure occurs. Without it, the node can no longer participate in the `keysign` ceremonies, until it participates in another successful `keygen` ceremony.

## Ports

The default port is `55055`.

If you start with the default ports, please make sure that they are not occupied by other programs or blocked by the firewall.

```bash
netstat -nlp | grep 55055 
```

## Install from source

`Go 1.20` or later is required. Check the [Go documentation](https://go.dev) for more info.

```bash
go env
```

Build from source:

```bash
go build --ldflags '-extldflags "-Wl,--allow-multiple-definition"' -o ./orchestrator
```

`orchestrator` service:

```bash
// todo
```
