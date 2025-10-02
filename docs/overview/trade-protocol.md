# Trade Protocol

Haveno is a decentralized network where people meet to exchange XMR for fiat or other cryptocurrencies. There are no central entities involved and trades happen directly between traders, which are both required to deposit and lock some XMR until the trade is completed. In case of disagreement between traders, both of them can request the involvement of an arbitrator, which will resolve the dispute.

## Secure trade with multisignature

Trades are secured using Monero's "Multisignature" technology

In Haveno we use 2-of-3 multisignature wallets because the number of keys generated is 3, but only 2 of those are needed to send a transaction.

!!! question "What is Multisignature?"
    A multisignature wallet is like a normal Monero wallet. The difference is that in a normal wallet there is only one key to be used to "sign" (send) transactions, while in a multisignature wallet is more than one (multi-signature).

When two people start a trade, their deposits and fees are sent to a newly created multisignature wallet 2-of-3, where the 3 keys are distributed one each to the two traders and one to the arbitrator. Now three entities have 1 key each, but only 2 are needed to complete the trade.

The two traders will start the trade. So one of the two will have to send crypto or fiat currencies to the destination specified by the other trader.

!!! info "Remember"
    The payment happens outside of Haveno, which is mostly used to hold the two trader's deposits in escrow and the amount of XMR to be exchanged.

When the recipient of the payment confirms that they received the money (for example, a bank transfer), they will sign a transaction using the key they received when they added their deposit. All this process is made as simple as needing to click "Send Payment" after a payment is sent and "Payment Received", after it's received.

## Overview of a trade

!!! note
    This is a simplification to give an idea of the typical flow of a trade on Haveno. For technical details [see the PDF](../resources/trade-protocol.pdf).

Roles:

- **Maker** - the person making the offer
- **Taker** - the person taking the offer
- **Arbitrator** - entity resolving possible disputes

!!! remember
    We assume the maker is selling XMR and the taker is buying them in exchange for fiat.

1. (optional) Maker **deposits funds** to Haveno wallet and waits for them to be unlocked (~20 minutes).
2. Maker **creates an offer** which reserves funds for the security deposit + amount to be sent to the taker. Also creates a penalty transaction that penalizes the maker if they break protocol.
3. Taker **accepts offer** which reserves funds for the security deposit + amount to be sent to the maker. As for the maker in the step above, a penalty transaction is created.
4. A **2/3 multisignature wallet is created** between the maker, taker, and arbitrator. The arbitrator holds the third key so that they can be summoned in case of disputes. If there is no dispute, the traders will complete the transaction without involving an arbitrator.
5. Both users have their deposits locked in multisig. Now they wait until the funds are spendable (~20 minutes). In the meantime, the XMR buyer can send payment outside Haveno (e.g. send a bank transfer to the other trader).
6. When the taker has received the agreed amount with the agreed payment method, they **signal to the maker** that they have received the payment.
7. The maker checks they have received the agreed amount and if everything is ok, they **confirm the completed payment**.
8. When the XMR seller confirms they received the payment outside Haveno, the agreed amount in XMR is sent to the buyer, while the security deposits of both traders are returned to them. **Trade completed**.

This protocol ensures trades on Haveno are non-custodial (Haveno never has access to your funds), peer-to-peer (there is no central entity, people trade among themselves), and safe (thanks to the security deposit and opt-in arbitration).
