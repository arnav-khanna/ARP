# Agent Resource Protocol (ARP)

**ARP is a proposed website-origin protocol that represents the Web as resources, relationships, actions, and policies so autonomous agents can understand and interact with websites natively.**

The human Web is built from HTML, CSS, JavaScript, links, forms, and visual interfaces. Agents can use that Web, but only by repeatedly reverse-engineering interfaces meant for people. ARP proposes an agent-native layer for the Web: websites publish machine-readable resources, semantic relationships, executable actions, governance policies, signatures, receipts, and domain extension schemas.

## Why ARP

Browser automation does not scale to billions of agent interactions.

Today, agents often need to download HTML, execute JavaScript, inspect the DOM, infer page semantics, identify controls, and simulate user interactions. ARP replaces repeated UI inference with explicit website-origin semantics.

With ARP, agents can:

- Discover a site's agent interface at `/.well-known/arp.json`
- Understand resources through typed properties and relationships
- Navigate websites as semantic graphs
- Execute declared actions with inputs, preconditions, side effects, and receipts
- Respect governance policies, authorization, rate limits, and audit requirements
- Verify signed receipts using cryptographic signatures

## Core Model

| Primitive | Meaning |
| --- | --- |
| Resource | A website entity, document, workflow, account, product, article, form, or capability |
| Property | A typed assertion about a resource |
| Relationship | A typed edge from one resource to another |
| Action | A declared operation with inputs, outputs, constraints, side effects, and governance |

Traditional Web abstraction:

```text
Web = Documents + Links
```

ARP abstraction:

```text
Web = Resources + Relationships + Actions + Policies
```

## Start Here

- [Overview](docs/INTRODUCTION_TO_ARP.md): plain-language explanation and architecture
- [Implementation (Guide & Tools Work In Progress!!)](docs/IMPLEMENTATION.md): quickstart, examples, tools, validation
- [Support articles](docs/ARP_SUPPORT_ARTICLES.md): implementation guides for site owners, platforms, and agent developers
- [Research and adoption](docs/RESEARCH_AND_ADOPTION.md): ARBench, migration, standardization


## Tools [Work in Progress!!]

- `tools/arp_convert/`: HTML-to-ARP conversion pipeline that emits ARP 1.1 documents
- `tools/arbench/`: ARBench benchmark harness
- `schemas/`: JSON Schema artifacts
- `examples/`: example discovery, resource, key, and receipt documents

## Project Status

ARP is an early research draft and proposed Internet standard. It is not yet an official IETF, W3C, ACM, IEEE, or ISO standard.

The goal is to develop ARP into a publishable research project and eventually a community-reviewed specification, following the adoption trajectory of protocols such as `robots.txt`.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md), [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md), [GOVERNANCE.md](GOVERNANCE.md), and [SECURITY.md](SECURITY.md).

## License

Released under the [Apache License 2.0](LICENSE).
