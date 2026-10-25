# Data model (proposed)

All examples are synthetic illustrations.

## On-ledger records

### Consent receipt

```json
{
  "receiptId": "CR-0001",
  "merchantRef": "M-SYN-001",
  "recipient": "LenderOrg",
  "purpose": "working-capital review",
  "scope": ["daily_inflow_summary"],
  "createdAt": "2026-11-12T10:00:00Z",
  "expiresAt": "2026-12-12T10:00:00Z",
  "status": "active"
}
```

### Decision attestation

```json
{
  "decisionId": "D-0001",
  "receiptId": "CR-0001",
  "modelVersion": "v0.1",
  "reasonDigest": "sha256:...",
  "inputFingerprint": "sha256:...",
  "recordedAt": "2026-11-12T10:05:00Z"
}
```

### Dispute

```json
{
  "disputeId": "DP-0001",
  "decisionId": "D-0001",
  "reason": "incorrect input",
  "evidenceRef": "ref:...",
  "state": "open",
  "outcome": null
}
```

## Off-chain only

Raw transactions, personal identifiers, feature values and model inference output stay in the owning organisation's systems. Sensitive details shared between organisations are intended to travel through DRUNIX private data, to be confirmed with mentors.

## Fingerprints

`inputFingerprint` and `reasonDigest` are cryptographic hashes computed off-chain. A party that holds the underlying data can recompute the hash and compare it with the ledger record. The ledger never needs the data itself.

## Illustration

A synthetic merchant with an average inflow of 8,000 rupees a day and 4,500 rupees a day of supplier outflow receives a suggested review band of 15,000 to 25,000 rupees, with reasons and an audit receipt. This is an illustration on synthetic data, not a credit recommendation.
