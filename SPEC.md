# ARS Specification v0.1

- **Status:** DRAFT. Pre-standard. Not suitable for compliance claims.
- **Date:** May 2026
- **Authors:** Wesley Snow (RANKIGI)

## Purpose

Define the minimum interface contract that each layer in a consequence-bearing AI decision stack must satisfy to produce a single portable independently verifiable ARS Record.

## Scope

ARS defines what each layer must produce. ARS does not define implementations, programming languages, frameworks, or deployment architectures.

## Layer 1: Decision Completeness Object (DCO)

### What it must contain

- Packet ID: unique, non-empty
- Declared options: minimum 2
- Declared assumptions: minimum 1
- Declared invalidation conditions: minimum 1
- Human acknowledgment of any advisory signals with written justification
- Structural completeness verdict: `PERMIT` or `REFUSE`
- Packet fingerprint: SHA-256 of canonical JSON (sorted keys, no whitespace, UTF-8)
- Timestamp: ISO 8601

### Conformance test

Same input always produces same output forever. The fingerprint must be independently recomputable by any third party using standard tools.

### What it must NOT do

- Interpret semantic meaning
- Assess quality or correctness
- Make decisions
- Learn or adapt

## Layer 2: Execution Proof Object (EPO)

### What it must contain

- Agent identity reference
- Per-action event records, each containing:
  - Action type
  - Input hash (SHA-256)
  - Output hash (SHA-256)
  - Timestamp (ISO 8601)
  - Previous event hash
  - Event hash (SHA-256 of canonical event fields)
- Chain head hash
- Independent anchor reference (transparency log inclusion proof or equivalent)
- Verification pathway (publicly documented, no trust in issuing system required)

### Conformance test

Any third party can recompute every event hash and verify the chain independently using standard tools. No API call to the issuing system required.

### What it must NOT do

- Interpret agent intent
- Assess action correctness
- Sit in the execution path

## Layer 3: Human Acknowledgment Object (HAO)

### What it must contain

- Reference to the sealed execution summary being acknowledged
- Hash of the exact bytes shown to the human
- Declared signature meaning: `APPROVAL`, `RESPONSIBILITY`, `REVIEW`, or `ACKNOWLEDGMENT`
- Declared authority and delegation chain
- Cryptographic signature (Ed25519 or equivalent)
- Timestamp: ISO 8601
- Independent anchor reference

### Conformance test

The human signed over exact normalized bytes. The signature is independently verifiable without calling back to the originating system.

### What it must NOT do

- Modify the execution chain
- Gate other layers
- Filter what the closure layer receives

> Note: HAO operates in parallel with RCO. Neither gates the other.

## Layer 4: Responsibility Closure Object (RCO)

### What it must contain

- Reference to the EPO
- Declared accountable party
- Declared authority basis
- Closure kind
- Declared at timestamp (ISO 8601)
- Closure object hash (SHA-256 of canonical closure fields)
- Independent anchor reference
- Subject reference and lifecycle state

### Conformance test

A third party can verify the closure object hash and anchor independently. The record is portable outside both the execution system and the closure system.

### What it must NOT do

- Exist inside the execution chain
- Depend on HAO completion

## Layer 5: Admissibility Output Object (AOO)

### What it must contain

- Reference to all upstream objects (DCO, EPO, HAO, RCO)
- Normalization rules used for each object
- Hash of each upstream object at time of export
- Timestamp of export (ISO 8601)
- Export format version
- Self-validation instructions

### Conformance test

A qualified third party with no access to any originating system can verify the complete ARS Record using standard tools (OpenSSL, SHA-256, public transparency log queries).

### What it must NOT do

- Require access to any originating system to verify
- Contain raw sensitive data

## The ARS Record Structure

```json
{
  "ars_version": "0.1",
  "record_id": "uuid",
  "generated_at": "ISO 8601",
  "profile": "ARS-BASIC | ARS-ATTESTED | ARS-FULL | ARS-REGULATED",
  "layers": {
    "dco": {
      "packet_fingerprint": "",
      "anchor_ref": ""
    },
    "epo": {
      "chain_head": "",
      "anchor_ref": ""
    },
    "hao": {
      "attestation_hash": "",
      "anchor_ref": ""
    },
    "rco": {
      "closure_hash": "",
      "anchor_ref": ""
    }
  },
  "record_hash": "SHA-256 of canonical JSON of above",
  "self_validation": {
    "tools_required": ["openssl", "curl"],
    "verification_steps": []
  }
}
```

## Cross-reference rule

- HAO must commit to specific DCO and EPO-head digests.
- RCO must commit to HAO and EPO-head-at-closure digests.
- AOO must commit to all four layer digests.

## Conformance Profiles

| Profile | Layers | Description |
|---------|--------|-------------|
| ARS-BASIC | DCO + EPO | Minimum viable accountability record. Proves structural completeness and execution proof. |
| ARS-ATTESTED | DCO + EPO + HAO | Adds human acknowledgment layer. |
| ARS-FULL | All five layers | Complete accountability record. |
| ARS-REGULATED | ARS-FULL plus regulation-specific export format | EU AI Act, HIPAA, SOC 2, 21 CFR Part 11. |

## What ARS Does Not Define

- Which implementation produces each layer
- Programming language or framework
- Which transparency log (any append-only independently auditable log qualifies)
- Which signature scheme (Ed25519 recommended, ML-DSA-65 for post-quantum)

## Disclaimer

ARS v0.1 is a pre-standard draft. It is not suitable for compliance claims, legal admissibility assertions, or regulatory submissions without jurisdiction-specific review.
