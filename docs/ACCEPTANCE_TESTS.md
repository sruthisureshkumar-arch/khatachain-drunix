# MVP acceptance tests

The prototype is done when these five tests pass. All data is synthetic. Results will be recorded here during the build window.

| # | Test | Given / When / Then | Proves | Status |
|---|---|---|---|---|
| 1 | Consent receipt commits and queries | Given a merchant grants consent, when `CreateConsentReceipt` is submitted, then the receipt commits and can be queried with its purpose, scope and expiry | Records are stored and retrievable | Not started |
| 2 | Unauthorised access is rejected | Given a party with no active receipt, or a field outside scope, when it calls `RegisterDataAccess`, then the call is rejected | Permissions are enforced by the contracts | Not started |
| 3 | Revocation blocks future access | Given an active receipt, when the merchant revokes it, then the next `RegisterDataAccess` is rejected | Consent can be withdrawn | Not started |
| 4 | Attestation can be verified | Given a recorded attestation, when a party recomputes the input fingerprint and reason digest, then they match the ledger record | Decisions are auditable | Not started |
| 5 | Dispute is visible, latency measured | Given a decision, when the merchant opens a dispute, then authorised parties can see it, and we record the time from submission to visibility | The contest process works | Not started |

## Targets for the demo

- 100% of demo decisions carry a verifiable receipt.
- 100% of unauthorised access attempts in testing are rejected.
- End-to-end latency for dispute visibility is measured and reported.

These are targets, not results.
