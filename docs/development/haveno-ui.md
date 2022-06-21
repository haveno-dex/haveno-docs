# The Haveno user interface

Currently Haveno is using Bisq's user interface (what we call 'legacy UI'), which we inherited when we forked Bisq. We think the legacy UI has serious performance and usability issues, so we decided to build our own interface, focused on performance and simplicity.

!!! note
    The new user interface is being developed by an external team, but works have been temporarily suspended. See the [dedicated blog post](https://haveno.exchange/blog/development-ui-suspended/) for the details.

The new interface will bring several improvements compared to the legacy UI:

- Separated backend and frontend, which will result in increased performances and user experience
- Built on ReactJS instead of the very heavy and poorly performing JavaFX
- Completely new flow built for all users not only power users

Like the rest of Haveno, the new user interface is completely open source and contributions/suggestions from the community are welcome, but remember to take a look at the [contributor guidelines](contributor-guidelines.md) first.

[:material-github: haveno-ui](https://github.com/haveno-dex/haveno-ui){ .md-button .md-button--primary }

## Contribute to Design

A dedicated developer has been designing the new user interface. Community contributions are welcome and encouraged. See the repositories for more deails.

[:material-github: haveno-design](https://github.com/haveno-dex/haveno-design){ .md-button .md-button--primary }