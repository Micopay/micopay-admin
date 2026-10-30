# MicoPay Admin

Internal console for operating the MicoPay cash network: find a user or an operation, understand why an agent cannot operate, and compare MicoPay's records with the on-chain escrow.

> **Status:** Architecture and design. There is no application code yet.
> **Network:** the MicoPay backend currently runs on Stellar Testnet.
> **Custody:** the console has no power over user funds and never acts on behalf of a user or agent.

## Who it is for

MicoPay's support and operations staff. It is not a portal for agents or app customers, and it is not a public service.

## Planned scope

1. **Operator access:** individual sign-in with two-step verification, directory search, user records, internal notes, and a history of staff actions.
2. **On-chain state:** each operation shows its MicoPay status next to its contract status, with differences flagged.
3. **Agent readiness:** for each agent, MicoPay requirements and Stellar checks, with a clear verdict and the action the agent must take in their own wallet.

The first version is read-only for accounts and funds, apart from internal notes.

## Custody model

Funds stay in user wallets or in the on-chain escrow. The console cannot move, freeze, or redirect them, because it has no function for it and the contract would not allow it.

## Relationship to other repositories

- **This repository:** the web application (planned stack: React, TypeScript, and Vite).
- **[MicoPay Protocol](https://github.com/Micopay/micopay-protocol):** the admin API, migrations, and business rules. The console does not create a second user database.

See the [ecosystem map](https://github.com/Micopay/.github/blob/main/docs/ECOSYSTEM.md).

## Plan

Read the [public plan](docs/PLAN.md).

## Setup and tests

There is nothing to run yet. This section will be completed with the first application code.

## Known limitations

- Nothing is implemented or deployed.
- The console depends on API work in MicoPay Protocol that does not exist yet.

## Security

Do not disclose vulnerabilities in public issues. Report them privately to [eric@micopay.com.mx](mailto:eric@micopay.com.mx).

## Important notice

This repository is a design document, not a financial service, an investment product, or a promise of availability.
