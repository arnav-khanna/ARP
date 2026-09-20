# Changelog

## 1.1.0 - 2026-07-04

### Added
- **Architecture**: Transitioned to the "12 Pillars of ARP" architectural model.
- **Schemas**: Bumped all core schemas (`resource`, `action`, `discovery`) to v1.1.
- **Resource Schema**: Added `intent`, `state`, `memory_key`, `agents`, `events`, and `media` (Pillars 10, 8, 7).
- **Action Schema**: Added `cost` and `permissions` for action-planning and capability metadata.

### Changed
- **ARBench**: Replaced modeled constants with empirical token measurements via `tiktoken`. The simulation harness now actively reads physical HTML fixtures to calculate true token savings for browser vs ARP agents.
- **Documentation**: Clarified that ARBench figures are benchmark-specific measurements or research estimates, not universal deployment claims.

## 0.2.0 - 2026-07-03

### Added
- **Schemas**: Introduced `keys.schema.json` and `receipt.schema.json` to define Ed25519 signature formats and verification parameters.
- **Documentation**: Added operational specs including quickstart, signatures, and pipeline guidance.
- **Tools**: Added `arp_convert`, a Python pipeline to automatically convert HTML sites into ARP resource graphs via JSON-LD, OpenGraph, and DOM heuristics.
- **Tools**: Added `arbench`, a simulation harness benchmarking ARP agent efficiency against traditional browser automation (showing ~97% token reduction).
- **Examples**: Included comprehensive `news-article`, `saas-subscription`, `government-permit`, `telehealth-booking`, `bank-transfer`, `flight-booking`, and `shipment-tracking` examples demonstrating diverse resource and action modeling.
- **Offline Fixtures**: Added a comprehensive suite of static HTML test corpus modeled after top 500 company designs (`ecommerce.html`, `news.html`, `saas.html`, `government.html`, `healthcare.html`, `banking.html`, `travel.html`, `logistics.html`) for offline pipeline validation.

### Changed
- **Schemas**: Updated `resource.schema.json` to include a `metadata.signature` field for resource-level authenticity.
- **Documentation**: Overhauled `README.md` and the support-article set to improve clarity around the protocol's value proposition.

## 0.1.0 - 2026-06-28

- Initial ARP research paper.
- Added GitHub documentation set.
- Added community health files.
- Added initial protocol documentation set.
