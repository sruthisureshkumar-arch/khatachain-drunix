# Transaction specifications

These are proposed application-level chaincode transactions. The names are ours, not built-in DRUNIX APIs. Endorsement policies are proposed and to be confirmed.

## T1 `CreateConsentReceipt`

- **Triggered by:** merchant
- **Inputs:** merchant reference, recipient (lender), purpose, scope, expiry
- **Checks:** purpose and expiry must be present; expiry must be in the future; recipient must be a known organisation
- **Writes:** consent receipt with timestamp and status `active`
- **Endorsement (proposed):** merchant side and the named recipient
- **Why:** every later access can be tested against a clear, provable agreement

## T2 `RegisterDataAccess`

- **Triggered by:** policy layer
- **Inputs:** receipt ID, accessing party, fields accessed
- **Checks:** receipt exists and is active, not expired, not revoked; accessing party matches the recipient; fields are within scope
- **Writes:** access audit event. No raw data
- **Rejects:** any access outside scope, after expiry or after revocation
- **Why:** builds an access trail without exposing raw data

## T3 `RecordDecisionAttestation`

- **Triggered by:** scoring service
- **Inputs:** receipt ID, decision ID, model version, reason digest, input fingerprint
- **Checks:** receipt is active; model version is registered; digests are well formed
- **Writes:** attestation, immutable once committed
- **Endorsement (proposed):** scoring provider and lender
- **Why:** the scorer cannot quietly rewrite what it ran, and any authorised party can check a decision against the record

## T4 `CreateOffer` and `AcceptOffer`

- **Triggered by:** lender (offer), merchant (acceptance)
- **Inputs:** decision ID, offer terms; acceptance reference
- **Checks:** a valid attestation exists; acceptance is explicit and by the same merchant
- **Writes:** offer and acceptance state
- **Endorsement (proposed):** lender and merchant side
- **Why:** neither side can later dispute the terms

## T5 `RecordRepaymentEvent`

- **Triggered by:** demo service
- **Inputs:** offer ID, synthetic repayment event
- **Checks:** offer is accepted
- **Writes:** repayment event. **Synthetic only. No real money moves**
- **Why:** lets us show the full lifecycle without real lending

## T6 `OpenDispute` and `ResolveDispute`

- **Triggered by:** merchant (open), reviewer (resolve)
- **Inputs:** decision ID, reason, evidence reference; outcome
- **Checks:** dispute refers to an existing decision; only a reviewer can resolve
- **Writes:** dispute state, evidence reference, outcome
- **Why:** contesting a wrong input becomes a defined, auditable process

## Revocation

`RevokeConsent` (merchant) sets a receipt to `revoked`. After that, T2 rejects any access. This supports acceptance test 3.
