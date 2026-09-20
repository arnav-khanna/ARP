# ARP JSON Schemas

This directory contains JSON Schema (draft 2020-12) artifacts for validating ARP documents.

> **Note on schema URLs**: Schemas reference `https://schemas.arp.dev/...`. This domain is a placeholder pending registration. Until then, reference schemas by relative path.

---

## Schema Files

| Schema | Purpose | Validates |
|---|---|---|
| [discovery.schema.json](discovery.schema.json) | Discovery document | `/.well-known/arp.json` |
| [resource.schema.json](resource.schema.json) | Resource documents | `/{path}.agent.json` |
| [action.schema.json](action.schema.json) | Action objects | `actions[]` in resources |
| [governance.schema.json](governance.schema.json) | Governance policies | `/.well-known/arp-policy.json` |
| [extension.schema.json](extension.schema.json) | Extension registry entries | Extension declarations |
| [keys.schema.json](keys.schema.json) | Public key documents | `/.well-known/arp-keys.json` |
| [receipt.schema.json](receipt.schema.json) | Action receipts | Signed receipt documents |

---

## Usage

### Using ajv-cli (Node.js)

```bash
npm install -g ajv-cli

# Validate discovery document
ajv validate -s schemas/discovery.schema.json -d examples/well-known-arp.json

# Validate a resource
ajv validate -s schemas/action.schema.json -s schemas/resource.schema.json -d examples/news-article.agent.json

# Validate a signed receipt
ajv validate -s schemas/receipt.schema.json -d examples/arp-receipt-signed.json

# Validate key document
ajv validate -s schemas/keys.schema.json -d examples/arp-keys.json
```

### Using Python (jsonschema)

```python
import json
import jsonschema

with open("schemas/resource.schema.json") as f:
    schema = json.load(f)

with open("examples/news-article.agent.json") as f:
    instance = json.load(f)

jsonschema.validate(instance, schema)
print("Valid!")
```

### Using the arp_convert tool

The arp_convert tool generates draft output; validate that output against these schemas before publishing:

```bash
pip install -r tools/arp_convert/requirements.txt
python -m arp_convert.cli --fixture ecommerce
```

---

## Schema Design Principles

1. **Additive**: `additionalProperties: true` everywhere — ARP is intentionally extensible
2. **Required fields only**: Schemas validate minimum required structure, not every optional field
3. **Versioned**: ARP core documents use `"arp": "1.1"`; the receipt envelope separately uses its explicit receipt schema version, currently `"version": "1.0"`. Schema URLs are stable placeholders pending registration; a breaking schema change requires a new schema URL.
4. **Cross-referenced**: Action schema is referenced from resource schema; signature schema from receipt schema
