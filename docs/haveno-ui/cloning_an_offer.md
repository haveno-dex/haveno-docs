# Cloning an offer

Offer cloning was introduced in Haveno v1.0.19, allowing offer makers to reuse the same trade funds and security deposit across multiple offers.

### What's the difference between duplicating and cloning an offer?

**Duplicating an offer** creates a new offer with similar trade details, but it requires separate trade funds and a security deposit. Each duplicated offer must be funded independently.

**Cloning an offer** creates a copy of the original offer without reserving additional funds. This reduces locked capital, enabling you to list the same offer across multiple markets or payment methods. When one of the cloned offers is taken, the others are automatically closed, as they share the same reserved funds. Cloned offers must have the same trade amount and security deposit, but they must differ in payment method or currency.

### What are the advantages of cloning an offer?

Cloning an offer allows you to easily create multiple listings for different payment methods and currencies, increasing the chances of your offer being accepted.

For example, if you’re in the US and willing to accept payment via ACH, Strike, Wire, or Zelle, you can clone your offer to show all four payment methods without the need to create separately funded offers. This increases the visibility of your offer in the Haveno offer book, improving the likelihood that your offer is taken quickly.

### What are the disadvantages of cloning an offer?

The main disadvantage of cloning an offer is that once a cloned offer is taken, all associated clones (those sharing the same reserved funds) will disappear from the Haveno offer book.

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
