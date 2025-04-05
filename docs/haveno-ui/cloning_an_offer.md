# Cloning an offer

`Cloning an offer` is a feature added in Haveno v1.0.19.

Cloning an offer allows offer makers to use the same security deposit for multiple offers.

### What is this difference between duplicating and offer and cloning an offer?

Duplicating an offer creates a new transaction. Therefore, each duplicated offer has a cost in terms of a new mining and trade fee for the on-chain transaction.

Cloning an offer uses the original deposit transaction of the cloned offer to create a new offer that shares the original deposit transaction, therefore, no additional on chain transaction or fees are needed.

### What are the advantages of cloning an offer?

The advantages of cloning an offer are the ability to create more offers for different payment methods and/or different currencies, increasing the likelihood that someone will take your offer.

For example if you are in the US and happy to accept payment by ACH, Strike, Wire, or Zelle, you could clone your offer to show all 4 of these payment methods without paying to create an additional offer. This will result in 4 offers been shown in the Haveno offer book and increase the chance that your offer is taken quickly.

### What are the disadvantages of cloning an offer?

The disadvantages of cloning an offer is that once your cloned offer is taken all associated clones (those that share the same reserved funds & `Group ID`) will disappear from the Haveno offer book.

### What offers can be cloned?

Any maker offer can be cloned.

Clones *MUST* be for a different payment method and/or currency.

For example you cannot create 5 Zelle clones (as these would all be for USD and Zelle)

You could however create 5 Revolut clones for USD, EUR, AUD, GBP, CAD (as these would be for the same payment method but importantly 5 different currencies).

You could also create multiple clones of the same currency offers provding you use different payment methods for example a EUR trade for; SEPA, SEPA Instant, Wise, Revolut.

### How do I clone an offer?

There are two ways to clone an offer. Go to `PORTFOLIO` -> `MY OPEN OFFERS`:

* Right click the offer and click 'Clone offer with shared funds'.
* Press the square clone icon on the right of the offer.

![Clone Offer](../resources/img/haveno-ui/cloning-offer_dark.png#only-light)
![Clone Offer](../resources/img/haveno-ui/cloning-offer_light.png#only-dark)
/// caption
Clone offer with shared funds
///

### How do I see which of my offers are cloned?

Cloned offers share the same Group ID. You can see them in `MY OPEN OFFERS` under the `Group ID` column. (Visible in the picture above.)

### How do I edit a cloned offer?

You can edit a cloned offer just like any other offer. Click the pencil icon next to the offer in `MY OPEN OFFERS`

!!! note
    Cloned offers share the same XMR amount and must have different payment methods and/or currencies used for them. You will see a warning message if you try and change anything that cannot be changed.

### How many times can I clone an offer?

You can clone an offer 10 times. You will get an error message if you try and clone an offer more than 10 times.

You can however have multiple offers and clone them each up to 10 times.

### What happens when my clone offer is taken?

When your cloned offer is taken you will see it in your `OPEN TRADES` the same as if any of your other offers were taken.

The clones of the offer sharing the same `Group ID` will then disappear from your `MY OPEN OFFERS` and the Haveno offer book.

### Is there a way to create the same cloned offers again quickly?

Not currently, for now you will need to re-add your cloned offers manually.
