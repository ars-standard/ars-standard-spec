# Accountability Receipt Standard (ARS)

> The open standard for independently verifiable accountability records in consequence-bearing AI systems.

## What is ARS

ARS defines the minimum interface contract that each layer in a consequence-bearing AI decision stack must satisfy to produce a single portable independently verifiable accountability record.

ARS does not define implementations. It defines what each layer must produce. Any conformant implementation qualifies.

## The ARS Record

A valid ARS Record is a cryptographically bound self-validating record that proves:

- The decision was structurally complete before execution
- The execution happened and was recorded independently
- A human acknowledged the sealed record
- Responsibility was declared and closed
- The record survives outside all originating systems

## Five Layers

| Layer | Object | Question Answered |
|-------|--------|-------------------|
| 1 | Decision Completeness Object (DCO) | Was the decision structurally complete before execution? |
| 2 | Execution Proof Object (EPO) | What did the agent actually do? |
| 3 | Human Acknowledgment Object (HAO) | Did a human acknowledge the sealed record? |
| 4 | Responsibility Closure Object (RCO) | Who is accountable and under what authority? |
| 5 | Admissibility Output Object (AOO) | Can the record be verified outside all originating systems? |

## Conformance Profiles

| Profile | Layers |
|---------|--------|
| ARS-BASIC | DCO + EPO |
| ARS-ATTESTED | DCO + EPO + HAO |
| ARS-FULL | All five layers |
| ARS-REGULATED | ARS-FULL plus regulation-specific export format |

## Status

- **Current version:** v0.1 Draft
- **Status:** Pre-standard. Not yet suitable for compliance claims.

## Reference Implementations

| Layer | Implementation | Maintainer |
|-------|----------------|------------|
| Layer 1 (DCO) | [OIM (Objective Integrity Model)](https://github.com/donaldtewhaiti1978-ops/Objective-Integrity-Model) | Don Tewhaiti, Zero Distortion Ltd |
| Layer 2 (EPO) | [RANKIGI](https://rankigi.com) | Wesley Snow, RANKIGI Inc. (founding reference implementation) |

See [IMPLEMENTATIONS.md](IMPLEMENTATIONS.md) for the full list.

## Contributing

ARS is an open standard. Contributions, implementations, and conformance reports are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

- Spec text: [CC-BY-4.0](LICENSE)
- Reference code: [Apache-2.0](LICENSE)
