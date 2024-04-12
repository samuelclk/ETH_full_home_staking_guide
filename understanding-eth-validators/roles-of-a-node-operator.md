# Roles of a node operator

## What do validator nodes actually do?

In short, validator nodes on the Ethereum network _**process transactions and secure the network.**_ This is done via the [proof-of-stake ](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/)consensus mechanism where each validator puts 32 ETH at stake to vouch for the validity of new blocks _- with a bundle of transactions in them -_ that either others or themselves have created.

In exchange for doing the work above, validators receive rewards from both users and the Ethereum protocol directly. However, if validators are **caught acting dishonestly** by other nodes in the network, **their stake is slashed** - forcibly burning their 32 ETH based on the severity of their actions. This mechanism is further explained in the [Rewards and penalties sub-section](rewards-and-penalties.md).

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Source: <a href="https://github.com/flashbots/mev-boost">https://github.com/flashbots/mev-boost</a></p></figcaption></figure>

Let's examine the value chain of the ETH validator network to understand this process in more detail.

1. In most cases, when users interact with Dapps built on top of the Ethereum network or directly sends Ethereum assets to one another, their transaction will first be sent into a holding area called the Mempool.
2. Block Builders retrieve transactions via 3 channels to bundle into a block before pushing it to the upcoming block proposers via Relays.
   * **Observing the Mempool directly**
   * **Working with MEV searchers:** These are another group of users that observe the Mempool for arbitrage opportunities. When they find such opportunities, they insert their own transaction and submit the arbitrage transactions set to the block builders
   * **Working directly with users** who want to send their transactions directly to block builders
3. The Relays connect to the Consensus Clients of the validator network via the mev-boost public auction service to allow validators to accept the highest bids (MEV fees)
4. The validator network picks up transactions in 2 ways:
   * **From the Mempool directly** via the Execution Clients. Transactions received this way are built into blocks locally.
   * **From the Relays** via the mev-boost public auction. Transactions received this way are pre-built into blocks by the block builders.
5. Every epoch (6.4 minutes), 32 validators are chosen randomly serve as block proposers, each proposing a block in their assigned \~12-second slots.
6. This block proposer has \~4 seconds to receive, execute, and send the block back out
7. A committee of validators (the Beacon Committee) is chosen at random to determine the validity of this new block within the remaining \~8 seconds
8. Once the new block is proposed and sufficiently attested to, it is added to the blockchain
9. At the end of every 2 epochs, all prior transactions are finalised and can no longer be reversed without burning a large portion of staked ETH across the whole network
