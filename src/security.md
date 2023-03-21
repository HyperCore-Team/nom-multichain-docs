# Security

The philosophy underpinning the architecture is based on the **Law of Action-Reaction**. There can be no effect without a cause: every redeem transaction processed by the bridge must adhere to this causality principle. This means that funds must be `locked`/`burned` on the source network in order to be `redeemed` on the destination network.

## Time-challenge security primitive

Every sensitive action is split into **two separate stages** and is **safeguarded** by a *time-challenge security primitive*. This primitive is used throughout the codebase for every sensitive action that either involves moving funds or other critical actions.

Additionally, the two step procedure is **enforced** in both *wrapping* or *unwrapping* assets:

1. **REQUEST**: this action signals the intention of a user to wrap or unwrap his assets.

2. **REDEEM**: this action settles the user's request and actually delivers the swapped assets to its corresponding address.

After the redeem request is created, there is a `window` during which the funds are unredeemable. Also during this `window`, the orchestrator nodes will check if there is a corresponding transaction on the source network that `locks`/`burns` the funds. In case it doesn't exist, the orchestrator nodes will start the *halting process* of the bridge.

## Realtime Monitoring

The NoM Multi-chain infrastructure project expects all participants to develop and maintain their own security monitoring strategies. This expectation is based on the value of having heterogeneous monitoring strategies across the Guardian set as a function of Wormhole's defense in depth approach, increasing the likelihood of detecting fraudulent activity.

**Pillar operators** participating in the **Orchestrator Layer** (running orchestrator nodes) should aim to capture all of the following domains with their monitoring strategies:

- Orchestrator node monitoring: TSS ceremonies, peer-to-peer messages, logs, etc.
- Blockchain node monitoring: transaction activity, peer-to-peer messages, reorgs, logs, etc.
- Smart Contracts activity: on-chain state, emitted events, etc.
- Administrator Module activity: on-chain state
- Social media monitoring
- Bug bounty program
- Github security reporting
- Audit findings
- [Forum](https://forum.zenon.org) thread

The orchestrator layer has a powerful built-in [logging](https://pkg.go.dev/go.uber.org/zap) library.

Lastly, if any user detects a security event via their monitoring system, they are urged to engage with the `Pillar operators` or the `admin` in order to activate the `emergency state`.

## Emergency Mode

The emergency mode for the decentralized bridge is designed to prevent any damage should a failure occurs.
Only the administrator can put the bridge on each network in an emergency state.

There are 4 possible states in total:

```go
LiveState      uint8 = 0
KeyGenState    uint8 = 1
HaltedState    uint8 = 2
EmergencyState uint8 = 3
```

## Distributed Halting Mechanism

The decentralized bridge has a built-in distributed halting mechanism that will halt it in case it observes an event on a destination network that has no corresponding event on the source network. 
Concretely, redeem requests without lock/burn associated transactions will trigger a signing ceremony to halt the bridge on each network.
Only the administrator and the orchestrator nodes can call the halting procedure via the dedicated halt method.

## Guardians Voting System

In the extreme case that the bridge is in emergency state the administrator address becomes `null`. A new administrator must be voted by the guardians in order to restore functionality.
Once a voted address reaches a quorum of `50% + 1` votes out of the total number of guardians, it becomes the new administrator.