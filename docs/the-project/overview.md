# Overview of Haveno

Haveno is a platform where people meet to exchange Monero for other cryptocurrencies or fiat currencies. Monero coins are used as security deposit by traders, which will then make the exchange outside Haveno (for example a bank transfer from trader A to trader B). After the exchange is completed, both traders will receive their security deposit back. One trader will receive the asset (for example, Euros or Dollars) and the other trader the amount of XMR agreed for that asset.

## The Haveno Network

When running an Haveno app, you'll be one of the nodes that composes the Haveno network. There is no central entity, trades happen directly between nodes (peers), that's why it's called a peer-to-peer network (P2P). This network runs on Tor, a technology that makes communication between peers private.

Haveno, unlike a centralized exchange, doesn't provide any liquidity to the network, but instead offers a place for people to meet and an escrow system to make sure both parties honour the terms of the trade until it's completed.

!!! note
    Haveno is fully non-custodial: the wallet is created locally and only the user has access to it.

## The Haveno Ecosystem

Haveno is revolutionary not only because it brings native, decentralized trades based on Monero, but also because of the structure of its ecosystem, composed of multiple independent entities, each with a very specific role:

- [Haveno Core Team](roles/core-team.md): Works on the code of the platform and marketing, but doesn't run it.
- [Motoro](roles/motoro.md): It's the entity that receives all the fees paid by users on Haveno, appoints arbitrators, and votes on Motoro proposals.
- [Publishers](roles/publishers.md): They make the software available and run some of the seednodes

This structure makes Haveno resistant to external attacks and ensures that the codebase is never "live".

## Behind the scenes

When users pay trade fees on Haveno, these are sent directly to Motoro. This is an important point, because the Haveno Core Team, who maintain the Haveno code and project will not have control over the revenue generated by the platform, same for the publishers.

These funds will form a treasure that the Konsilio, which manages Motoro, will decide how to administer and distribute. Anyone working or contributing to Haveno in any form will be welcome to make Motoro proposals and ask for their efforts to be rewarded using Motoro's fund.

!!! Note
    Motoro is very similar to [Monero's CCS](https://ccs.getmonero.org), but instead of generous donors providing funds, the fees paid by the users on Haveno will be the source of funds. Decisions about how to manage these funds are taken by the Konsilio.

For example, the Haveno Core Team, who created and maintains Haveno, will open periodical proposals, asking for a reward for their contribution to the ecosystem. Same for the Publishers. The Konsilio will vote on the proposal and release the funds if they approve it.

This works for individuals too! If you contribute to Haveno, for example by doing development work or creating a project that benefits Haveno's ecosystem, you can open Motoro proposals and ask to be rewarded.

!!! info "Remember"
    Motoro is not meant to be used only for Haveno-related projects, but also to fund Monero projects or individuals. Do you have an idea that would improve Monero and need funds to develop it? Open a Motoro proposal!
