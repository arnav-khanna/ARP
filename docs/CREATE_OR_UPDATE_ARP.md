# Create or Update ARP Files

This guide explains how to add ARP to a website or update an existing ARP deployment.

## Before you begin

Decide what kind of agent interaction your site should support. Start with the smallest useful surface area.

Good first targets include:

- Public articles or documentation.
- Product catalog resources.
- Store search and cart actions.
- Appointment or booking resources.
- Public forms with clear validation rules.
- Government service descriptions.

Do not begin with high-risk account, payment, medical, legal, or irreversible actions unless your authorization and review process is already mature.

## 1. Create the discovery file

Create:

```text
https://example.com/.well-known/arp.json
```

Minimal example:

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

The discovery file should be served over HTTPS, return a successful HTTP status, and use JSON.

## 2. Add a governance policy

Create a policy file if your discovery document links to one:

```json
{
  "policyVersion": "2026.07.05",
  "rateLimits": [
    {
      "tier": "default",
      "limit": 600,
      "period": "1h"
    }
  ],
  "trainingUse": "contact-required",
  "auditRequiredFor": ["financial", "legal", "sensitive-data"],
  "contact": "mailto:agent-policy@example.com"
}
```

Policies are instructions for cooperating agents. They do not replace server-side enforcement.

## 3. Publish at least one resource

A resource describes an entity or workflow on your site.

```json
{
  "arp": "1.1",
  "id": "resource:home",
  "url": "https://example.com/",
  "types": ["website.home"],
  "properties": {
    "name": "Example",
    "description": "Example website agent entrypoint"
  },
  "relationships": [
    {
      "type": "hasCatalog",
      "target": "https://example.com/.well-known/arp/resources/catalog.agent.json",
      "cardinality": "one"
    }
  ],
  "actions": [],
  "metadata": {
    "generatedBy": "example-arp-generator"
  }
}
```

Resources should be stable enough for agents to reference over time.

## 4. Declare actions carefully

Actions describe operations agents may execute. Start with safe read actions, then add state-changing actions after review.

```json
{
  "id": "catalog.search",
  "title": "Search catalog",
  "safe": true,
  "idempotent": true,
  "reversible": true,
  "safetyClass": ["safe-read"],
  "binding": {
    "method": "GET",
    "href": "https://example.com/api/catalog/search",
    "contentType": "application/json"
  },
  "inputs": {
    "type": "object",
    "properties": {
      "q": { "type": "string" }
    }
  },
  "outputs": {
    "type": "object"
  },
  "authorization": {
    "required": false
  },
  "requiresHumanConfirmation": false,
  "sideEffects": []
}
```

For financial, legal, externally visible, sensitive, or irreversible actions, require appropriate authorization and confirmation.

## 5. Validate the files

Validate each JSON document against the schemas in the `schemas/` directory.

Recommended checks:

- JSON parses successfully.
- Required fields are present.
- URLs resolve.
- Action endpoints enforce authorization.
- Public resources do not leak private data.
- Governance policy contact information works.
- Human-facing pages and ARP resources describe the same underlying facts.

See [Test and validate ARP](TEST_AND_VALIDATE_ARP.md) for more checks.

## 6. Monitor agent traffic

After launch, monitor:

- Requests to `/.well-known/arp.json`.
- Requests to resource files.
- Action execution success and failure rates.
- Authorization failures.
- Rate-limit events.
- Agent contact or support requests.

Use these signals to improve schemas, relationships, actions, and policies.

## Updating an existing ARP deployment

When you update ARP files:

- Keep old resource identifiers working when possible.
- Avoid changing action meaning without changing the action identifier or version.
- Keep governance policy changes visible and easy to audit.
- Test with a staging agent before production rollout.
- Publish extensions only when the core model cannot express the domain clearly.
