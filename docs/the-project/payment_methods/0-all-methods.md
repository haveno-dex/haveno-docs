# Payment methods

Haveno offers several payment methods to buy and sell Monero for fiat currencies or other cryptocurrencies.

Every offer on Haveno designates a payment method for traders to settle payments. The Haveno software does not actually integrate with any payment methods—**all non-monero fund transfers are made outside of Haveno software.**

Unlike cryptocurrency transfers, many fiat payment transfers are riddled with flaws and limitations that require Haveno to employ special measures to encourage fair and honest trades. One of these measures is lower per-trade limits, which are covered below. Another key measure is [account signing](../account_limits.md/#account-signing).

## 1. Fiat payment methods

The payment methods vary in chargeback risk, regional availability, transaction size, fees, privacy, verifiability, and other characteristics. The top consideration for maintaining payment methods is chargeback risk.

!!! warning
    Chargebacks for payments made with PayPal, Venmo and Cash App are relatively easy. Using these payment methods involves additional risk, which should be considered when selling Monero. (e.g. you may limit the amount per trade or adjusting the premium)

We welcome suggestions for new payment methods on Haveno, especially for those that enable new markets. Documentation on the criteria and process for adding new payment methods is found [here](https://github.com/haveno-dex/listing). Please also feel free to make suggestions by reaching out on Matrix room **Haveno Development** [#haveno-development:monero.social](https://matrix.to/#/#haveno-development:monero.social) relayed on IRC/Libera (`#haveno-development`)

Below is a list of fiat payment methods Haveno currently supports.

!!! note
    The maximum trade sizes listed below are not available for most newly-created payment accounts. Please refer to Account signing for details on how to enable larger trade sizes for your payment accounts.

#### 1.1 Fiat payment methods

|      Payment Method       |             Region               | Trading Period |       Limit per Trade        |                                     Notes                                       |
| ------------------------- | -------------------------------- | -------------- | ---------------------------- | ------------------------------------------------------------------------------- |
| [ACH](ACH.md)* | USA | 5 days | 12.00 XMR |
| Advanced Cash* | Global (BRL, EUR, GBP, KZT, RUB, UAH, USD) | 1 day | 128.00 XMR | Not available in the USA. |
| Alipay* | China | 1 day | 48.00 XMR |
| [Amazon eGift card](Amazon_eGift_card.md)* | Global | 1 day | 12.00 XMR | Supported in USD, EUR, CAD and a [handful of other markets.](Amazon_eGift_card.md/#2-regional-availability) |
| Australian PayID* | Australia | 1 day | 48.00 XMR |
| [Bizum](Bizum.md)* | Spain | 1 day | 12.00 XMR | Bizum limits to €1000 per trade, €2000 per day. |
| [Capitual](Capitual.md)* | Global (BRL, EUR, GBP, USD) | 1 day | 12.00 XMR |
| Cardless Cash (Cash at ATM)* | Global | 4 days | 12.00 XMR |
| Cash App | Global (GBP, USD) | 1 day | buy: 3.00 XMR  sell: 12.00 XMR | High risk of chargeback! |
| Cash deposit* | N/A | 4 days | 12.00 XMR | Not all banks allow 3rd party cash deposits. |
| Cell Pay* | Global (AUD, CAD, GBP, HKD, USD) | 1 day | 12.00 XMR | CelPay limits to $2,500 per 24h, supports multiple stablecoins. |
| [Domestic wire transfer](Domestic_Wire_Transfer.md)* | USA | 3 days | 12.00 XMR |
| [Face to Face (F2F)](F2F.md)* | Global | 4 days | 48.00 XMR | [See article](F2F.md) for special guidance on F2F transactions. |
| [Faster Payments](Faster_Payments.md)* | UK | 1 day | 12.00 XMR | [See article](Faster_Payments.md) for recent changes to avoid issues. |
| HalCash* | Spain | 1 day | 48.00 XMR |
| [IMPS](IMPS.md)* | India | 1 day |12.00 XMR | IMPS limits to Rs. 1,000,000 per day. |
| [Interac e-Transfer](interac_e-transfer.md) | Canada | 1 day | buy: 3.00 XMR  sell: 12.00 XMR | Interac e-Transfer Autodeposits are not supported. |
| Japan Zengin Furikomi* | Japan | 1 day | 12.00 XMR |
| [Monese](Monese.md)* | Europe (GBP, EUR and RON) | 1 day | 12.00 XMR |
| MoneyBeam (N26) | Europe | 1 day | buy: 3.00 XMR  sell: 12.00 XMR |
| MoneyGram* | Global | 4 days | 24.00 XMR |
| National Bank Transfer | N/A | 4 days | buy: 3.00 XMR  sell: 12.00 XMR |
| [Nequi](Nequi.md)* | Colombia | 1 day |12.00 XMR | Nequi limits to COP 7,000,000 per month. |
| [NEFT](NEFT.md)* | India | 1 day | 12.00 XMR | NEFT limits to Rs. 50,000 per transaction. |
| [Paxum](Paxum.md)* | Global | 1 day | 12.00 XMR |
| [Pay by Mail](Pay_By_Mail.md) (Cash by Mail) | Global | 8 days | 48.00 XMR | Please check [the article](Pay_By_Mail.md) and follow the instructions to avoid disputes. |
| PayPal | Global | 1 day | buy: 3.00 XMR  sell: 12.00 XMR | High risk of chargeback! |
| Paysafe | Global | 1 day | buy: 3.00 XMR  sell: 12.00 XMR | Paysafe has different limits per Tx & per year depending on the country. |
| [Paysera](Paysera.md)* | Global | 1 day | 12.00 XMR |
| [PayTM](PayTM.md)* | India | 1 day |12.00 XMR | PayTM limits to Rs. 100,000 per transaction. |
| Perfect Money* | Europe and USA | 1 day | 48.00 XMR |
| [Pix](Pix.md)* | Brazil | 1 day | 12.00 XMR |
| Popmoney | USA | 1 day | buy: 3.00 XMR  sell: 12.00 XMR |
| PromptPay* | Thailand | 1 day | 48.00 XMR |
| [Revolut](Revolut.md) | Global | 1 day | buy: 3.00 XMR  sell: 12.00 XMR | [See article](Revolut.md) for recent changes to avoid issues. |
| [RTGS](RTGS.md)* | India | 1 day | 12.00 XMR | RTGS Minimum of Rs. 200,000, Maximum of Rs. 1,000,000 per transaction. |
| [Satispay](Satispay.md)* | Italy | 1 day | 12.00 XMR |
| [SEPA](SEPA.md) | Europe | 6 days | buy: 3.00 XMR  sell: 12.00 XMR |
| [SEPA Instant](SEPA_Instant.md) | Europe | 1 day | buy: 3.00 XMR  sell: 12.00 XMR |
| Skrill* | Global | Varies | 12.00 XMR |
| [Strike](Strike.md)* | USA | 1 day | 12.00 XMR | Non-KYC strike users have lower limits. |
| [SWIFT](SWIFT.md)* | Global | 7 days | 24.00 XMR |
| Swish* | Sweden | 1 day | 48.00 XMR |
| Tikkie* | Netherlands | 1 day | 12.00 XMR | Tikkie limits to €2500 per 24h (€750/Tx) |
| Transfer with Same Bank | N/A | 2 days | buy: 3.00 XMR  sell: 12.00 XMR |
| Transfer from Specific Banks | N/A | 4 days | buy: 3.00 XMR  sell: 12.00 XMR |
| [US Postal Money Order](US_Postal_Money_Order.md)* | USA | 8 days | 12.00 XMR | Within USA - Max $1000 per money order, up to $3000 per day without ID. |
| Uphold | Global | 1 day | buy: 3.00 XMR  sell: 12.00 XMR |
| [UPI](UPI.md)* | India | 1 day | UPI limits Rs. 100,000 per transaction. |
| Venmo | USA | 1 day | buy: 3.00 XMR  sell: 12.00 XMR | High risk of chargeback! |
| [Verse](Verse.md)* | Europe (EUR, SEK, HUF, DKK, PLN) | 1 day | 12.00 XMR | Verse limits €10,000 per year. |
| WeChat Pay* | China | 1 day | 48.00 XMR |
| Western Union | Global | 4 days | 24.00 XMR |
| [Wise](Wise.md)* | Global except USD trades | 4 days | 12.00 XMR | formally TransferWise |
| [Wise-USD](Wise-USD.md)* | Global for USD trades | 4 days | 12.00 XMR | formally TransferWise |
| Zelle | USA | 4 days | buy: 3.00 XMR  sell: 12.00 XMR | See [this page](https://www.zellepay.com/get-started) to see if/how your bank works with Zelle (including sending limits). |

!!! note
    `*` These payment methods **do not** need to be signed to lift the 3 XMR buying limit. (See [account signing](../account_limits.md/#account-signing) for more details).

## 2. Cryptocurrency payment methods

Haveno also supports a variety of cryptocurrencies for buying and selling Monero, like BTC, ETH, LTC, BCH, USDT, USDC. Because cryptocurrency transfers are irreversible and relatively quick, cryptocurrency trades can be up to 528 XMR in size right away (no need to wait for account aging or account signing).

|  Payment Method  | Trading Period | Trade limit |
| ---------------- | :------------: | :---------: |
| Cryptocurrencies | 1 Day | 528.00 XMR |
| Cryptocurrencies Instant | 1 Hour | 528.00 XMR |

- Cryptocurrencies Instant trades go by fast! Please remember to disable Instant offers in `Portfolio` > `My Open Offers` if you might not be around to settle an Instant trade.
