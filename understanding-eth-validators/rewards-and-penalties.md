# Rewards and penalties

## Rewards

In exchange for processing transactions on the Ethereum network and providing economic security, validators receive rewards through issuance, transaction fees (tips), and MEV bribes. Issuance is received from performing attestations, and tips + MEV bribes are received from proposing blocks.&#x20;

The breakdown of these fees on average are as follows:

1. Consensus Layer Rewards: **2.88%**
   * Block attestation
   * Sync committee duties
2. Block rewards (MEV and transaction tips): **0.85%**

**Average total: \~3.73% APR**

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Example of average staking APR across major staking pools</p></figcaption></figure>

The average yield of a 32 ETH stake will be 1.344 ETH per annum. However, because the APR from tips and MEV bribes is random and tends to be large when they occur (i.e., a heavy right skew), the median total APR, which most validators will be getting, is closer to 2.5%.&#x20;

These rewards are received every \~6.4 minutes (every epoch), and the accumulated balance is automatically withdrawn to your designated wallet every 6 days.

## Penalties

### Uncorrelated downtime

On the other hand, you will be penalised if your validator goes offline or is otherwise inactive. Under normal circumstances, your validator should not go offline together with other validators—i.e., "uncorrelated downtime"—and if so, your inactivity penalties per epoch will be roughly the same as your rewards rate per epoch—unless, of course, you miss your turn to propose a block while you were inactive.

This means that you typically don't have to panic when you start seeing missed attestations, but you should rather take your time to make sure you don't make a mistake when troubleshooting your validator node.

### Correlated downtime

However, if your validator node goes down with a large portion of the network, you will be penalised more heavily.&#x20;

In an extreme scenario where more than 1/3 of the network goes offline, causing the chain to be in danger of splitting, the ["inactivity leak"](https://eth2book.info/capella/part2/incentives/inactivity/) will be triggered.

The inactivity leak is a state of emergency to compel the inactive 1/3 to resume their duties or otherwise removing them (and their ETH) from the network until the network has >2/3 of active validators once again.&#x20;

Two main mechanisms will occur during an inactivity leak:

1. **Normal attestation rewards will stop for all validators.** Block proposers will still receive their normal rewards from all 3 sources.
2. **Quadratic leak** - where inactive validators will suffer increasing penalties that grow quadratically over time until the network has >2/3 of active validators once again

## Double signing

Double signing is a slash-able offence and occurs when your validator:

1. Signs two different beacon blocks for the same slot while serving as proposer - i.e. endorsing two different versions of the chain's history
2. Signs an attestation that “surrounds” another one while serving as an attester - i.e. trying to change the history of the chain&#x20;
3. Signs two different attestations with the same target while serving as an attester - i.e. signing the same block twice using the same key

However, these slashing mechanisms were set in place to discourage dishonest or malicious behaviour by the validator network. They can be caused by negligence and misconfiguration as well. In fact, all of the slashing incidents to date have been due to common technical mistakes, and you can learn how to prevent them below.

{% content-ref url="../best-practices/slashing-prevention.md" %}
[slashing-prevention.md](../best-practices/slashing-prevention.md)
{% endcontent-ref %}

When your validator is slashed, the following events will occur.

1. Your validator will be forcibly scheduled to exit the network within the next 36 days.
2. Receive a minimal penalty of 1/32 of your effective balance initially when a whistleblowing validator reports your validator.
3. Incur additional penalties for missing your validator duties over the next 36 days until your validator exits the network.&#x20;
4. Additionally, your validator will be subject to a special penalty for correlated slashing. This penalty increases with the number of other validators slashed over the same period as yours and can even reach your entire effective balance.
