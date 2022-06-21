# Arbitrators

Arbitrators are the key of [conflict resolution](../conflict-resolution.md) in Haveno. They are the entity that holds the third key of each trade, so that they can intervene and unlock a trade when summoned.

Arbitrators are a sensitive role, because they hold 1 of the 3 keys involved in a trade and they can potentially collude with one of the traders, since with 2 of the 3 keys, it's possible to send the deposited XMR to one of the two trader's address. Given the risk, there are several mechanism in place that make very unlikely that an Arbitrator go rogue. See the [trade protocol](../trade-protocol.md) for details.

## Nomination

People who wish to work as arbitrators on Haveno will open a dedicated Motoro proposal, detailing why they would like to be Arbitrators, their previously experience in Monero and other details that make them trustworthy for the role. In the future, bonds might be introduced, to provide an additional layer of security.

## The work of the arbitrator

An arbitrator will run an Haveno instance and will stay available to traders. When called, they will evaluate the complaints and, after making the appropriate investigation, will decide which part is in the right and which part in the wrong and will award escrowed funds accordingly.

## Reward

Arbitrators are free to ask for a reward for their work to [Motoro](motoro.md), by opening a Motoro Proposal, which will be evaluated by the Konsilio, like every other proposal.