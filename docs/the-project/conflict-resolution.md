# Conflict Resolution

Trades don't always go smooth. Things can go wrong for multiple reasons, like a trader being suddenly unresponsive, wrong bank details provided, etc. When that happens and using the trade chat to talk with the other trader didn't help resolve the problem, human intervention is necessary to unlock the situation.

As we explained in the [trade protocol](trade-protocol.md) page, trades are secured by Monero's multisignature and one of the 3 keys is given to an [arbitrator](roles/arbitrators.md), which will not intervene until there is an issue the two traders cannot resolve by themselves using the trade chat.

Once the arbitrator is summoned, they will assess which party is in the right and which one is in the wrong and use the key the were provided to side with one of the two traders.

## Safety Assurances

Arbitrators are appointed by the [Konsilio](roles/motoro.md#the-konsilio) and they are given one key for each trade, so that when they are called, they have the power to side with one of the traders and move funds.

This makes the arbitrator a sensitive role. Since a trade can be completed if 2 of the 3 keys confirm the transaction, a trader could find themselves in the situation where a malicious arbitrator sides with the other trader with no merit and push the transaction to them, resulting in the honest traded being factually scammed.

We take multiple measures to make this situation very unlikely and discouraged:

- The arbitrator is picked randomly from the pull of available arbitrators.
- Arbitrators will be anonymous on the platform.
- Arbitrators will be people with a high reputation in the Monero and Haveno communities.

!!! note
    Bonds are a powerful tool to ensure an arbitrator doesn't behave maliciously and they will be probably added in the future.

## Conflict resolution example

A practical example of a conflict resolved by an arbitrator on Haveno:

1. Alice and Bob have a trade in progress, where Alice is selling XMR and Bob is buying in exchange for EUR.
2. Alice sees that Bob is signaling on Haveno that he sent the money through bank transfer, as agreed, but the money haven't arrived on Alice's bank account yet.
3. After Alice has waited enough to ensure the transfer is not simply delayed, she will contact the other trader through the built-in chat in the trade.
4. Bob claims he sent the payment, but Alice still hasn't received it. She has no other choice than to summon an arbitrator
5. The arbitrators will be called in and will chat with both traders, asking for details and an overview of the situation
6. The arbitrators, using tools that assure the authenticity of the resources provided, sees that Bob has actually never sent the payment.
7. Since Bib is misbehaving (claims to have sent the payment but he didn't), he will be punished by the arbitrator, which will send her deposit back to Alice and will take the security deposit from Bob as punishment.
8. Alice got her locked deposit and XMR back, while Bob lost his deposit. Conflict resolved.

!!! note
    The deposit is taken away as punishment only in case of malicious trader. Technical or other problems will be simply resolved by sending each trader's deposit (minus the platform fee) back to them.