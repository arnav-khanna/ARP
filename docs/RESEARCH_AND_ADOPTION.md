# Research, Benchmarking, and Adoption

ARP is both a protocol proposal and a research project.

## Core Research Claim

ARP changes the fundamental abstraction of the Web for agents:

```text
HTML Web: Documents + Links
ARP Web: Resources + Relationships + Actions + Policies
```

This moves agent interaction away from brittle browser automation and toward explicit website-origin semantics.

## Architectural Gap

| Existing Standard | What It Provides | Gap ARP Addresses |
| --- | --- | --- |
| `robots.txt` | Crawl governance | No resource/action semantics |
| `sitemap.xml` | URL discovery | No typed resource graph |
| Schema.org | Entity metadata | No first-class actions or governance |
| OpenAPI | API operation descriptions | Not a website resource graph |
| MCP | Agent-tool communication | Not website-origin publication |
| `llms.txt` | LLM-oriented documentation | Not executable protocol semantics |
| Semantic Web | General graph representation | Weak operational adoption for actions |

## ARBench

ARBench compares browser-based agents and ARP-enabled agents. Results can be accessed here:
- [ARBench](../tools/arbench-results.md). These are research estimates and benchmark measurements, not universal deployment claims.

## Agent Engine Optimization

Agent Engine Optimization (AEO) is the practice of designing, exposing, validating, and monitoring website resources so agents can complete user tasks accurately and safely.

Metrics:

- Resource coverage
- Relationship completeness
- Action availability
- Validation score
- Median ARP payload
- Task success rate
- Policy clarity
- Receipt coverage

## Migration Model

| Site Type | Deployment Path | Typical Effort |
| --- | --- | --- |
| Static blog/docs | Build-time generator | 1 day |
| WordPress site | Plugin maps posts and forms | Plugin install plus configuration |
| Shopify store | Store app exposes catalog, variants, cart actions | App install plus policy review |
| Enterprise SaaS | SDK maps internal APIs to ARP resources/actions | 2-6 engineering weeks |
| Government portal | Phased rollout for public info and forms | Multi-phase program with legal review |

## Adoption Path

ARP follows the `robots.txt` trajectory:

1. A few agents support ARP discovery.
2. Websites publish ARP because it makes agent traffic easier to govern.
3. More agents support ARP because websites expose useful structure.
4. Tooling, validators, registries, and best practices emerge.
5. The convention becomes stable enough for formal standardization.

## Certification Profiles

- Bronze: valid discovery and core resources
- Silver: graph consistency, safe search, basic governance
- Gold: signed receipts, capability negotiation, action validation
- Enterprise: high-assurance identity, audit export, sector review

## Future Work

- Live ARBench deployments
- Reference validators
- Domain extension registry
- Browser developer-tools inspector
- WordPress and Shopify prototypes
- Internet-Draft submission
