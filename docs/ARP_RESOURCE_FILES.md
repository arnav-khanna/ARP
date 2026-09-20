# ARP Resource Files

An ARP resource file describes a website entity, document, workflow, state, or capability in a way that agents can understand.

Resources are the basic nodes of the ARP graph.

## Resource model

Each resource can contain:

- `id`: a stable resource identifier.
- `url`: the related human-facing or canonical URL.
- `types`: one or more type identifiers.
- `properties`: typed facts about the resource.
- `relationships`: links to other ARP resources.
- `actions`: operations available from the resource.
- `metadata`: operational details about the representation.
- `policy`: resource-specific policy information.

## Example product resource

```json
{
  "arp": "1.1",
  "id": "product:sku-123",
  "url": "https://shop.example/products/sku-123",
  "types": ["ecommerce/product/v1"],
  "properties": {
    "name": "Everyday Backpack",
    "description": "A 20 liter backpack for daily carry.",
    "price": {
      "amount": 89.00,
      "currency": "USD"
    },
    "availability": "in-stock"
  },
  "relationships": [
    {
      "type": "hasVariant",
      "target": "https://shop.example/.well-known/arp/resources/product-sku-123-black.agent.json",
      "cardinality": "many"
    }
  ],
  "actions": [
    {
      "id": "cart.addItem",
      "title": "Add item to cart",
      "safe": false,
      "idempotent": false,
      "reversible": true,
      "safetyClass": ["state-changing"],
      "binding": {
        "method": "POST",
        "href": "https://shop.example/api/cart/items",
        "contentType": "application/json"
      },
      "inputs": {
        "type": "object",
        "required": ["sku", "quantity"],
        "properties": {
          "sku": { "type": "string" },
          "quantity": { "type": "integer", "minimum": 1 }
        }
      },
      "outputs": {
        "type": "object"
      },
  "authorization": {
    "required": false
  },
  "requiresHumanConfirmation": false,
  "sideEffects": ["cart-state-changed"]
}
  ],
  "metadata": {
    "language": "en"
  }
}
```

## Resource identifiers

Use stable identifiers. An identifier should keep the same meaning over time.

Good examples:

- `product:sku-123`
- `article:2026-07-05-agent-web`
- `form:passport-renewal`
- `service:business-license`

Avoid identifiers that depend on temporary sessions, unstable database rows, or presentation details.

## Properties

Properties are facts about the resource. They should be structured and predictable.

Examples:

```json
{
  "properties": {
    "name": "Passport Renewal",
    "agency": "Example Department",
    "estimatedCompletionTime": "P3W",
    "requiresAppointment": false
  }
}
```

Keep sensitive user-specific facts out of public resources.

## Relationships

Relationships connect resources into a graph.

```json
{
  "type": "requiresDocument",
  "target": "https://example.gov/.well-known/arp/resources/document-proof-of-address.agent.json",
  "cardinality": "many"
}
```

Common relationship uses:

- Product to variant.
- Article to author.
- Service to required document.
- Course to lesson.
- Form to submission action.
- Account to authorized resources.

## Actions on resources

A resource can include actions that are relevant to that resource. For example, a product resource can expose `cart.addItem`, while an appointment resource can expose `appointment.book`.

Do not publish an action unless the backend endpoint enforces the same authorization and constraints described in ARP.

## Deployment checklist

- Include required fields from `schemas/resource.schema.json`.
- Use stable IDs.
- Keep public resources free of private data.
- Link to related resources with clear relationship types.
- Add actions only when the site is ready to accept agent execution.
- Keep resource facts consistent with human-facing pages.
