# ARP Extensions

ARP has a small core model. Extensions add domain-specific vocabulary without changing the base protocol.

Use an extension when a domain needs shared terms, resource types, action identifiers, or validation rules that many sites and agents can reuse.

## When to use an extension

Use an extension for:

- Ecommerce products, carts, orders, and checkout.
- Travel inventory, booking, cancellation, and loyalty accounts.
- Government forms, eligibility rules, and submission receipts.
- Education courses, lessons, assignments, and credentials.
- Healthcare scheduling, provider directories, and consent-bound workflows.
- Financial accounts, quotes, transfers, and compliance actions.

Do not create an extension for one site's private implementation detail unless you expect other agents or sites to interoperate with it.

## Declaring extensions

Extensions are declared in the discovery file.

```json
{
  "extensions": [
    {
      "id": "ecommerce/product/v1",
      "schemaUrl": "https://arp.dev/extensions/ecommerce/product/v1/schema.json",
      "specUrl": "https://arp.dev/extensions/ecommerce/product/v1/spec.md",
      "status": "draft"
    }
  ]
}
```

## Extension identifiers

Use stable identifiers with a domain and version.

Examples:

- `ecommerce/product/v1`
- `ecommerce/cart/v1`
- `travel/hotel/v1`
- `government/form/v1`
- `education/course/v1`

Avoid vague names such as `advanced`, `custom`, or `premium`.

## Extension contents

An extension can define:

- Resource types.
- Required or recommended properties.
- Relationship types.
- Action identifiers.
- Input and output schemas.
- Safety expectations.
- Error codes.
- Receipt formats.
- Governance requirements.

## Example extension use

```json
{
  "arp": "1.1",
  "id": "product:sku-123",
  "url": "https://shop.example/products/sku-123",
  "types": ["ecommerce/product/v1"],
  "properties": {
    "name": "Everyday Backpack",
    "price": {
      "amount": 89.00,
      "currency": "USD"
    },
    "availability": "in-stock"
  },
  "relationships": [],
  "actions": [],
  "metadata": {
    "extension": "ecommerce/product/v1"
  }
}
```

## Versioning guidance

Make backward-compatible changes within the same major version.

Create a new version when:

- A required field is removed or renamed.
- An action changes meaning.
- Safety expectations change.
- Input validation changes in a breaking way.
- Existing agents would misinterpret the extension.

## Extension review checklist

- The extension solves a real interoperability problem.
- Names are stable and domain-specific.
- Schemas are explicit.
- Actions include safety and authorization expectations.
- Examples cover common use cases.
- Versioning rules are stated.
- Governance implications are documented.
