# Haveno API

The Haveno API lets programs interact with Haveno directly, using the same backend that powers Haveno Desktop. It provides access to wallets, offers, trades, and market data. With it you can build trading bots, gather market statistics, or integrate Haveno into another application.

This guide covers the setup and a few short programs to get started.

!!! info "Just want to trade?"
    If you only want to buy and sell XMR, see [Getting Started](getting-started.md). This guide is for building on top of Haveno.

## How it works

Programs interact with a **Haveno daemon** (`havenod`), the same non-custodial backend used by Haveno Desktop. The daemon holds the wallet and connects to the Haveno network over Tor.

The API is exposed as [gRPC](https://grpc.io) services, so any gRPC-compatible client can use it. This guide uses [**haveno-ts**](https://github.com/haveno-dex/haveno-ts), a TypeScript library that wraps the API in typed calls. A small [Envoy](https://www.envoyproxy.io) proxy bridges the client to the daemon.

```
your program (haveno-ts)  →  Envoy proxy  →  Haveno daemon  →  Haveno network
```

## Requirements

- [Node.js](https://nodejs.org)
- A running Haveno daemon (see [Build and Run Haveno](../developers/installing.md))
- The [Envoy](https://www.envoyproxy.io/docs/envoy/latest/start/install.html) proxy

!!! note
    The examples assume a daemon reachable at `http://localhost:8080`. Any Haveno network can be used.

## Start a daemon

Build Haveno, then start a daemon and the Envoy proxy in separate terminals:

```bash
# terminal 1: start the Haveno daemon
make haveno-daemon-mainnet
```

```bash
# terminal 2: start the Envoy proxy
envoy -c config/envoy.yaml
```

The [Haveno sample app](https://github.com/haveno-dex/haveno-sample-app) is the quickest way to run this end to end. It includes ready-made Envoy configurations and a minimal program to build from.

!!! warning "Secure your daemon"
    The API grants full control of the daemon and its wallet. Anyone who can reach the API port and provide the password can move your funds. By default the password is empty and the daemon listens on every network interface, so an exposed daemon is open to anyone.

    Protect it with at least one of the following, and preferably both:

    - **Set a strong password** with `--apiPassword`.
    - **Block the API port** with a firewall so it is reachable only from the machine your program runs on.

## Install the library

In a project, install haveno-ts:

```bash
npm install haveno-ts
```

## Connect to a daemon

Create a client and request the daemon's version to confirm the connection.

```ts
import { HavenoClient } from "haveno-ts";

// connect to a Haveno daemon
const haveno = new HavenoClient("http://localhost:8080", "apitest");

console.log("Connected to Haveno " + await haveno.getVersion());

await haveno.disconnect();
```

The second argument is the daemon's API password, set when the daemon starts.

## Read the market

The following program reads live market data: the current price of XMR, and the open offers to buy or sell it.

```ts
import { HavenoClient, HavenoUtils, OfferDirection } from "haveno-ts";

const haveno = new HavenoClient("http://localhost:8080", "apitest");

// the current price of 1 XMR in USD
const price = await haveno.getPrice("USD");
console.log(`Market price: 1 XMR = $${price}`);

// the open offers to sell XMR for USD
const offers = await haveno.getOffers("USD", OfferDirection.SELL);
console.log(`\n${offers.length} offers to sell XMR:`);

for (const offer of offers) {
  const amount = HavenoUtils.atomicUnitsToXmr(offer.getAmount());
  console.log(`  ${amount} XMR  @  $${offer.getPrice()}  (${offer.getPaymentMethodShortName()})`);
}

await haveno.disconnect();
```

!!! note "A note on amounts"
    Haveno measures XMR in **atomic units** (1 XMR = 10¹² atomic units). Use `HavenoUtils.atomicUnitsToXmr` and `HavenoUtils.xmrToAtomicUnits` to convert between the two.

## Gather statistics

The daemon exposes the network's public trade history, which can be used for research, dashboards, or price analysis. This example counts completed trades by currency.

```ts
import { HavenoClient } from "haveno-ts";

const haveno = new HavenoClient("http://localhost:8080", "apitest");

// count completed trades by currency
const stats = await haveno.getTradeStatistics();
const counts = new Map<string, number>();
for (const stat of stats) {
  const code = stat.getCurrency();
  counts.set(code, (counts.get(code) ?? 0) + 1);
}

console.log("Trades by currency:");
for (const [code, count] of counts) {
  console.log(`  ${code}: ${count}`);
}

await haveno.disconnect();
```

## Listen for trades

Register a listener to receive updates as a trade progresses: a new trade, a payment sent, a payout confirmed.

```ts
import { HavenoClient, NotificationMessage } from "haveno-ts";

const haveno = new HavenoClient("http://localhost:8080", "apitest");

await haveno.addNotificationListener((notification: NotificationMessage) => {
  if (notification.getType() === NotificationMessage.NotificationType.TRADE_UPDATE) {
    const trade = notification.getTrade();
    console.log(`Trade ${trade?.getShortId()} is now: ${trade?.getPhase()}`);
  }
});

console.log("Listening for trade updates… press Ctrl+C to stop.");
```

## A complete trade

This example runs a full trade between two traders, Alice and Bob, each connected to their own daemon. Alice posts an offer to sell XMR for BTC, Bob takes it, and the two complete payment.

```ts
import { HavenoClient, HavenoUtils, OfferDirection, TradeInfo } from "haveno-ts";

// two traders, each connected to their own daemon
const alice = new HavenoClient("http://localhost:8080", "apitest");
const bob = new HavenoClient("http://localhost:8081", "apitest");

// each creates a payment account to send and receive BTC
const aliceAccount = await alice.createCryptoPaymentAccount("Alice's BTC", "BTC", "<alice-btc-address>");
const bobAccount = await bob.createCryptoPaymentAccount("Bob's BTC", "BTC", "<bob-btc-address>");

// Alice posts an offer to sell 0.5 XMR for BTC
const offer = await alice.postOffer({
  direction: OfferDirection.SELL,
  amount: HavenoUtils.xmrToAtomicUnits(0.5),
  assetCode: "BTC",
  paymentAccountId: aliceAccount.getId(),
  securityDepositPct: 0.15
});
console.log(`Alice posted offer ${offer.getId()}`);

// Bob takes the offer, which starts the trade
const trade = await bob.takeOffer(offer.getId(), bobAccount.getId());
console.log(`Bob took the offer, starting trade ${trade.getShortId()}`);

// the deposit transactions must confirm before payment can begin (about 20 minutes on mainnet)
await waitUntil(bob, trade.getTradeId(), (t) => t.getIsDepositsUnlocked());

// Bob (the buyer) sends BTC to Alice, then confirms payment sent
await bob.confirmPaymentSent(trade.getTradeId());
console.log("Bob sent payment");

// Alice (the seller) waits for the payment, then confirms it received
await waitUntil(alice, trade.getTradeId(), (t) => t.getIsPaymentSent());
await alice.confirmPaymentReceived(trade.getTradeId());
console.log("Alice received payment. Trade complete.");

await alice.disconnect();
await bob.disconnect();

// poll a trade until a condition is met
async function waitUntil(client: HavenoClient, tradeId: string, met: (trade: TradeInfo) => boolean) {
  while (!met(await client.getTrade(tradeId))) {
    await HavenoUtils.waitFor(10000); // check again in 10 seconds
  }
}
```

!!! note
    Both wallets must hold enough XMR to cover the trade amount, the security deposit, the trade fee, and mining fees

## Further reading

- [**API reference**](https://haveno-dex.github.io/haveno-ts/typedocs/classes/HavenoClient.HavenoClient.html): complete method documentation
- [**Sample app**](https://github.com/haveno-dex/haveno-sample-app): a runnable starting point
- [**haveno-ts**](https://github.com/haveno-dex/haveno-ts): the library source and tests
- [**Developer docs**](../developers/api.md): extending the API itself

!!! info "Questions?"
    For help, join the [Matrix room](https://matrix.to/#/#haveno:monero.social).
