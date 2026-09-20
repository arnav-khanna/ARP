# ARP Implementation Guide [Tools for Conversion are a work in progress]

This guide consolidates quickstart, tooling, validation, and implementation notes.

## Website Quickstart

Create `/.well-known/arp.json`:

```json
{
  "arp": "1.1",
  "origin": "https://your-site.example",
  "versions": ["1.1"],
  "siteCapabilities": ["arp/1.1"],
  "governance": {
    "policy": "https://your-site.example/.well-known/arp-policy.json"
  }
}
```

Create at least one resource document:

```text
/products/rain-jacket.agent.json
```

Validate it against:

```text
schemas/resource.schema.json
```

## Validation

Using `jq` for syntax:

```bash
jq empty examples/*.json schemas/*.json
```

Using `ajv-cli`:

```bash
npm install -g ajv-cli
ajv validate -s schemas/discovery.schema.json -d examples/well-known-arp.json
ajv validate -s schemas/action.schema.json -s schemas/resource.schema.json -d examples/news-article.agent.json
ajv validate -s schemas/receipt.schema.json -d examples/arp-receipt-signed.json
```

Using Python:

```python
import json
import jsonschema

with open("schemas/resource.schema.json") as f:
    schema = json.load(f)

with open("examples/news-article.agent.json") as f:
    instance = json.load(f)

jsonschema.validate(instance, schema)
```

## Agent Implementation

Minimum ARP agent behavior:

1. Fetch `/.well-known/arp.json`.
2. Validate discovery.
3. Load governance policy.
4. Retrieve entrypoint resources.
5. Traverse allowed relationships.
6. Validate action inputs before execution.
7. Refuse unknown high-risk actions.
8. Require human confirmation for financial, legal, sensitive, externally visible, or irreversible actions.
9. Verify receipts where signatures are provided.

## Signatures

ARP uses signed receipts and key documents for high-risk operations.

Key document:

```text
/.well-known/arp-keys.json
```

Receipt verification:

1. Resolve `signature.keyId`.
2. Fetch the corresponding public key.
3. Canonicalize the signed payload.
4. Verify the Ed25519 signature.
5. Check timestamp, nonce, and expected action id.

## Implementation Checklist

- [ ] Publish discovery document.
- [ ] Publish governance policy.
- [ ] Publish at least one resource document.
- [ ] Validate all JSON.
- [ ] Mark actions with safety classes.
- [ ] Require confirmation for high-risk actions.
- [ ] Return receipts for state-changing actions.
- [ ] Sign receipts for high-risk workflows.
- [ ] Document contact and abuse-reporting channels.
