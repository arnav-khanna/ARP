# ARP Discovery File

The ARP discovery file is the starting point for agents.

Agents look for:

```text
https://example.com/.well-known/arp.json
```

The discovery file tells agents which ARP version the site supports, which capabilities are available, where policies are published, and how to find resources.

## Basic discovery file

```json
{
  "arp": "1.1",
  "origin": "https://example.com",
  "versions": ["1.1"],
  "siteCapabilities": ["arp/1.1"],
  "representations": {
    "defaultMediaType": "application/arp+json",
    "contentNegotiation": true
  },
  "governance": {
    "policy": "https://example.com/.well-known/arp-policy.json"
  },
  "resources": {
    "home": "https://example.com/.well-known/arp/resources/home.agent.json"
  }
}
```

## Required fields

| Field | Meaning |
| --- | --- |
| `arp` | The ARP version used by the discovery document. |
| `origin` | The site origin that owns the ARP publication. |
| `versions` | ARP protocol versions supported by the site. |

## Common optional fields

| Field | Meaning |
| --- | --- |
| `siteCapabilities` | Capability identifiers supported by the site. |
| `representations` | Media type, content negotiation, and URL-template information. |
| `extensions` | Domain extensions supported by the site. |
| `authentication` | Authentication methods or authorization references. |
| `governance` | Location of governance and policy documents. |
| `resources` | Named entrypoints into the site resource graph. |

## Capability identifiers

Capabilities are short strings that tell agents what the site supports.

Examples:

```json
{
  "siteCapabilities": [
    "arp/1.1",
    "ecommerce/product/v1",
    "ecommerce/cart/v1",
    "safe-query-actions/v1"
  ]
}
```

Use capability identifiers to describe behavior agents can rely on. Avoid using them as marketing labels.

## Resource entrypoints

The `resources` object should give agents a small number of useful starting points.

Examples:

```json
{
  "resources": {
    "home": "https://example.com/.well-known/arp/resources/home.agent.json",
    "catalog": "https://example.com/.well-known/arp/resources/catalog.agent.json",
    "support": "https://example.com/.well-known/arp/resources/support.agent.json"
  }
}
```

Large sites do not need to list every resource in the discovery file. They can provide entrypoints and search actions.

## Common mistakes

- Publishing a discovery file that does not match the site origin.
- Linking to resources that return errors.
- Declaring capabilities that the site does not actually support.
- Publishing governance links that are missing or stale.
- Exposing private user resources without authentication.
- Treating the discovery file as a security boundary.

## Deployment checklist

- Serve the file from `/.well-known/arp.json`.
- Use HTTPS.
- Return valid JSON.
- Include `arp`, `origin`, and `versions`.
- Link to a governance policy if agents are expected to follow site-specific rules.
- Include at least one resource entrypoint.
- Validate with `schemas/discovery.schema.json`.
