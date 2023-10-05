# Why bother with home-staking?

### Liquid staking is more convenient with higher yields

With the **high convenience offered by liquid staking providers** and the diminishing validator rewards due to a large influx of new validators, you can't help but wonder why do we bother dealing with the head space and costs required for running your own home-based validator nodes.&#x20;

Besides, liquid staking providers also smooth out block + MEV rewards across their entire validator set and auto-compounds your rewards, which leads to **higher median yields of roughly 0.5% - 0.8% (after fees) vs solo staking.**

This is because the occurrence and amounts of block rewards and MEV fees are entirely random, leading to a long right tail in the distribution of total rewards for solo validators where **median < mean.**

### But these come at a cost..&#x20;

However, as with all things in crypto (and life), these conveniences come at a cost. There are 2 main risks to consider when using liquid staking platforms.

1. **Smart contract risks -** Smart contract exploits can happen anytime and we have seen that not even blue chip projects (e.g. Curve) are immune to them
   * A huge contributing factor is because there is a fundamental imbalance between auditors and attackers - i.e. audits are performed once or at certain checkpoints while attackers are attempting to exploit the smart contract continuously&#x20;
2. **Counterparty risks -** Centralised platforms are the first to come to mind when we think about this risk but this exists even in DeFi because:
   1. Smart contracts are upgradable and governed by a multi-sig and;
   2. Governance votes can change the rules (although indirectly) of the interaction with the smart contracts

This means that to stake large amounts, you will need to monitor the developments of the liquid staking smart contracts you are using very closely. Or you only stake amounts that you comfortable with losing completely.

Another thing to note, is that there will be a rush to exit when shit hits the fan and you will likely not  be first one out the door. So the key question to ask yourself is if the additional 0.5% - 0.8% APY is worth the tail risk of your total staked ETH.

### Other benefits of solo-staking&#x20;

**Instead of tail risks, solo-stakers are exposed to "tail gains"** from the long right tail of block and MEV rewards - e.g. the largest block + MEV reward received by a single validator to-date is 691 ETH. Think of this as paying 0.5% - 0.8% APY for a continuous lottery ticket with one draw every 12 seconds.

Even if you are not a gambler, there are upcoming open source initiatives that will allow solo-stakers to enjoy rewards-smoothing benefits that were previously only available with liquid staking providers.

1. [Dappnode MEV smoothing pool](https://discourse.dappnode.io/t/mev-smoothing-pool-for-dappnode/1804) - Subscribers pool their block+MEV rewards with all other subscribers
2. [Protocol-enshrined MEV smoothing](https://notes.ethereum.org/cA3EzpNvRBStk1JFLzW8qg) - (Still under discussion) If all MEV is smoothed out across all validators, the average rewards for solo-stakers will be on par with liquid staking aside from the auto-compounding effect&#x20;
3. [Protocol-enshrined MEV burn ](https://ethresear.ch/t/burning-mev-through-block-proposer-auctions/14029) - (Still under discussion) Burning all MEV is equivalent to sharing MEV rewards with all ETH holders
