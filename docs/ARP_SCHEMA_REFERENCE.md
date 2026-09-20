# ARP Schema Reference

This page summarizes the JSON Schema artifacts in the `schemas/` directory.

The schemas are implementer aids. They describe the expected shape of ARP documents, but production systems must still enforce authentication, authorization, validation, and business rules at runtime.

## Version authority

The current ARP core protocol version is `1.1`. Discovery documents, resources, and key documents therefore use `"arp": "1.1"`. Receipts have an independently versioned envelope and currently use `"version": "1.0"`. The JSON Schema files in this directory are authoritative for the document shapes described below.

## Discovery schema

File:

```text
schemas/discovery.schema.json
```

Purpose:

The discovery schema validates `/.well-known/arp.json`.

Required fields:

| Field | Meaning |
| --- | --- |
| `arp` | ARP version of the discovery document. |
| `origin` | Site origin that owns the ARP publication. |
| `versions` | Supported ARP protocol versions. |

Important optional fields:

| Field | Meaning |
| --- | --- |
| `siteCapabilities` | Capability identifiers supported by the site. |
| `representations` | Media type and content-negotiation details. |
| `extensions` | Domain extensions supported by the site. |
| `authentication` | Authentication method metadata. |
| `governance` | Policy document location. |
| `resources` | Resource entrypoints. |

## Resource schema

File:

```text
schemas/resource.schema.json
```

Purpose:

The resource schema validates ARP resource documents.

Required fields:

| Field | Meaning |
| --- | --- |
| `arp` | ARP version used by the resource. |
| `id` | Stable resource identifier. |
| `types` | Type identifiers for the resource. |
| `properties` | Structured facts about the resource. |
| `relationships` | Graph edges to other resources. |
| `actions` | Operations available from the resource. |
| `metadata` | Representation metadata. |

Relationship fields:

| Field | Meaning |
| --- | --- |
| `type` | Relationship type identifier. |
| `target` | Target resource reference. |
| `cardinality` | `one`, `many`, or `unknown`. |
| `traversal` | Optional traversal metadata. |

## Action schema

File:

```text
schemas/action.schema.json
```

Purpose:

The action schema validates executable operation declarations.

Required fields:

| Field | Meaning |
| --- | --- |
| `id` | Stable action identifier. |
| `safe` | Whether the action is read-only. |
| `binding` | Endpoint or operation binding. |
| `inputs` | Input schema. |
| `outputs` | Output schema. |
| `authorization` | Authorization requirements. |
| `requiresHumanConfirmation` | Whether a person must confirm before execution. |
| `sideEffects` | Declared effects. |

Safety classes:

| Class | Meaning |
| --- | --- |
| `safe-read` | Read-only action. |
| `state-changing` | Modifies application state. |
| `externally-visible` | Produces visible or communicative effects. |
| `financial` | Moves money, creates charges, or affects financial state. |
| `legal` | Creates legal or compliance consequences. |
| `sensitive-data` | Reads or writes sensitive data. |
| `irreversible` | Cannot be normally undone. |

## Governance schema

File:

```text
schemas/governance.schema.json
```

Purpose:

The governance schema validates ARP policy documents.

Important fields:

| Field | Meaning |
| --- | --- |
| `policyVersion` | Policy version identifier. |
| `rateLimits` | Rate-limit tiers and periods. |
| `trainingUse` | Site preference for model training or dataset use. |
| `auditRequiredFor` | Action classes that require audit trails. |
| `retention` | Retention guidance for action records. |
| `contact` | Policy contact address. |
| `revocation` | URL for revocation or policy updates. |
| `reputationRegistries` | Agent or site reputation registries recognized by the site. |

## Extension schema

File:

```text
schemas/extension.schema.json
```

Purpose:

The extension schema describes domain extension metadata, such as extension identifiers, schema URLs, versions, and registry information.

Use it when publishing reusable vocabularies for ecommerce, travel, government, education, healthcare, finance, or other domains.

Discovery documents advertise extensions with `id`, `schemaUrl`, `status`, and optional `specUrl`. A registry entry validated by this schema additionally identifies its `owner`.

## Key schema

File:

```text
schemas/keys.schema.json
```

Purpose:

The key schema validates `/.well-known/arp-keys.json`. Key documents use core ARP version `1.1` and publish `keys` with `kid`, `alg`, `use`, `status`, `activatedAt`, `publicKeyJwk`, and `purposes`.

## Receipt schema

File:

```text
schemas/receipt.schema.json
```

Purpose:

The receipt schema validates an action receipt wrapped in a top-level `receipt` object. Its required receipt fields are `id`, `version`, `action`, `resource`, `origin`, `executedAt`, `nonce`, and `status`; the current receipt-envelope version is `1.0`.

## Validation guidance

Schema validation is necessary but not sufficient.

Also verify:

- Linked URLs resolve.
- Action bindings match backend behavior.
- Authorization is enforced by endpoints.
- Sensitive resources are not public.
- Human-facing and ARP-facing facts are consistent.
- Governance policy requirements are operationally enforceable.
