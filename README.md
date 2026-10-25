# KhataChain

**Verifiable credit for India's micro-merchants, built on DRUNIX.**

Team Dhurandhar · Citi × NPCI DRUNIX Hackathon 2026

> **Status: proposal stage.** This repository currently holds the design, transaction specifications and acceptance tests. Code is built during the build window (10 to 22 November 2026). All data in the prototype is synthetic. No real lending and no real money movement.

## The problem

Merchants like Meena, a tea-stall owner, earn digitally every day but have little formal credit history. Lenders cannot cheaply verify their cash-flow evidence, a declined merchant rarely learns why, and nobody can later check which model made a decision on which inputs. Consent is hard to prove, revoke or audit.

This is a trust problem across several organisations, not only a scoring problem.

## What KhataChain does

Our thesis: **make evidence portable, not personal data public.** Scoring and raw data stay off-chain. Shared workflow state lives on DRUNIX.

| Function | What it means |
|---|---|
| Provable consent | Scoped, expiring, revocable consent receipts, checked on every data access |
| Evidence without exposure | Only fingerprints and decisions reach the shared ledger. Raw data stays with its owner |
| Reconstructable decisions | Each decision is anchored with model version, reason digest and input fingerprint |
| Shared dispute workflow | A merchant opens a dispute, a reviewer resolves it, and status and trail are visible to authorised parties |

A licensed lender makes the credit decision and owns policy, pricing, disclosures and servicing. KhataChain supports the lender. It does not lend.

## Architecture

```
Merchant app -> API / policy layer -> Scoring service
                       |                    |
                       v                    v
              +--------------------------------------+
              |            DRUNIX network            |
              |  merchant side | scorer | lender |   |
              |  reviewer (separate organisations)   |
              +--------------------------------------+
```

- **Off-chain, private:** raw transactions, feature extraction, model inference, access and revocation enforcement.
- **On-ledger, minimal:** consent receipts, decision attestations, offer and lifecycle state, dispute trail, audit events.

See [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for the diagram and component details.

## DRUNIX capabilities we plan to use

Exact support and configuration will be confirmed with DRUNIX mentors before implementation.

| Capability | How we plan to use it |
|---|---|
| Go chaincode | Business rules for the six transactions, including checks that reject unauthorised calls |
| Multi-organisation network | Merchant side, scoring provider, lender and reviewer endorse the steps that concern them |
| KeyDB transient store and private data | Sensitive details reach only the parties that need them. The ledger holds fingerprints |
| SQL-capable state database (YugabyteDB is a candidate) | Lenders and reviewers query decisions and disputes for audit |
| Stateless validation service | Signature and policy validation that scales as event volume grows |

## Transactions

These are our own application-level chaincode names, not built-in DRUNIX APIs. Full specifications are in [docs/TRANSACTIONS.md](docs/TRANSACTIONS.md).

| # | Transaction | Triggered by | Purpose |
|---|---|---|---|
| T1 | `CreateConsentReceipt` | Merchant | Record purpose, recipient, scope, timestamp and expiry |
| T2 | `RegisterDataAccess` | Policy layer | Log a permitted access against a valid receipt |
| T3 | `RecordDecisionAttestation` | Scoring service | Anchor decision ID, model version, reason digest, input fingerprint |
| T4 | `CreateOffer` / `AcceptOffer` | Lender, merchant | Record the offer and explicit acceptance |
| T5 | `RecordRepaymentEvent` | Demo service | Log synthetic repayment events |
| T6 | `OpenDispute` / `ResolveDispute` | Merchant, reviewer | Track review state and an evidence reference |

## Acceptance tests

The MVP is done when these five pass. Details in [docs/ACCEPTANCE_TESTS.md](docs/ACCEPTANCE_TESTS.md).

1. A consent receipt commits and can be queried.
2. Unauthorised access is rejected.
3. Revocation blocks future access.
4. An attestation can be verified.
5. A dispute is visible, and its latency is measured.

## Build plan

| Date | Milestone |
|---|---|
| 25 Oct 2026 | Idea and deck submission |
| 10 Nov | Top 25 announced |
| 10 to 22 Nov | Prototype build: local multi-organisation DRUNIX network, chaincode, scoring service on synthetic data, merchant and lender views |
| 22 Nov | Final submission with this repository, setup guide and results |

## Planned repository layout

```
chaincode/      Go chaincode for the six transactions (to be added)
scoring/        Synthetic data generator and explainable scoring service (to be added)
app/            Merchant and lender demo views (to be added)
network/        Local multi-organisation DRUNIX network scripts (to be added)
docs/           Architecture, transaction specs, data model, acceptance tests
```

## Running locally

Not available yet. Setup and run instructions will be added during the build window.

## AI approach and limits

An explainable scoring model with reason codes, starting with gradient-boosted trees. Features: inflow level and volatility, repeat customers, seasonality, outflow ratio and missing-data flags. Borderline cases go to human review.

Accuracy, calibration, fairness and latency are **unvalidated**. We will measure them and report results honestly. Any rupee figures in our documents are illustrations on synthetic data, not credit recommendations. A ledger record proves what was committed, not that a model is correct. Regulatory alignment is a design objective, not a certification. RBI and data-protection requirements must be validated before any real-data pilot.

## Contributing back to DRUNIX

We plan to contribute, following the project's contribution guidelines:

- a consent-receipt contract with tests,
- a decision-attestation transaction with tests,
- a synthetic end-to-end demo,
- a setup and transaction walkthrough,
- documentation of gaps, measured results and limitations.

## Team

Team Dhurandhar: Sruthi Sureshkumar (Team Leader), Thaarini Balagapandian.
