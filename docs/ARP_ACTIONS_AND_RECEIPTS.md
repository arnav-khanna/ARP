# ARP Actions and Receipts

An ARP action describes an operation an agent can execute.

Actions are how ARP moves beyond passive description. They tell agents what can be done, where the operation is bound, which inputs are required, what outputs are expected, and what side effects may occur.

## Action fields

| Field | Meaning |
| --- | --- |
| `id` | Stable action identifier, such as `catalog.search` or `cart.addItem`. |
| `title` | Human-readable action label. |
| `safe` | Whether the action is read-only and has no meaningful side effects. |
| `idempotent` | Whether repeating the same request has the same effect as one request. |
| `reversible` | Whether the effect can be undone through a normal workflow. |
| `safetyClass` | Risk categories such as `safe-read`, `financial`, or `legal`. |
| `binding` | HTTP or API binding used to execute the action. |
| `inputs` | Input schema. |
| `outputs` | Output schema. |
| `authorization` | Authorization requirements. |
| `preconditions` | Conditions that must be true before execution. |
| `sideEffects` | Declared effects of the operation. |
| `requiresHumanConfirmation` | Whether a person must confirm before execution. |
| `errors` | Expected error identifiers. |

`reversible` and `requiresHumanConfirmation` are the canonical core action fields for those semantics. Do not duplicate them in a `transaction` object; extensions may add separate transaction metadata only when it does not restate or override core action behavior.

## Safe read action

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
    "href": "https://shop.example/api/catalog/search",
    "contentType": "application/json"
  },
  "inputs": {
    "type": "object",
    "properties": {
      "q": { "type": "string" },
      "priceMax": { "type": "number" }
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

## State-changing action

```json
{
  "id": "checkout.placeOrder",
  "title": "Place order",
  "safe": false,
  "idempotent": false,
  "reversible": false,
  "safetyClass": ["state-changing", "financial", "externally-visible"],
  "binding": {
    "method": "POST",
    "href": "https://shop.example/api/checkout/orders",
    "contentType": "application/json"
  },
  "inputs": {
    "type": "object",
    "required": ["cartId", "paymentToken", "shippingAddressId"],
    "properties": {
      "cartId": { "type": "string" },
      "paymentToken": { "type": "string" },
      "shippingAddressId": { "type": "string" }
    }
  },
  "outputs": {
    "type": "object"
  },
  "authorization": {
    "required": true,
    "scopes": ["checkout:write"]
  },
  "sideEffects": ["payment-captured", "order-created"],
  "requiresHumanConfirmation": true
}
```

## Receipts

For state-changing actions, the site should return a receipt that records what happened.

Example receipt:

```json
{
  "receipt": {
    "id": "receipt:01HY",
    "version": "1.0",
    "action": "checkout.placeOrder",
    "resource": "order:100045",
    "origin": "https://shop.example",
    "executedAt": "2026-07-05T10:30:00Z",
    "nonce": "a1b2c3d4e5f6",
    "status": "succeeded",
    "outputs": {
      "total": {
        "amount": 89.00,
        "currency": "USD"
      }
    }
  }
}
```

Receipts help agents, users, and sites audit actions after execution.

## Safety guidance

Use `safe: true` only when the action does not change meaningful state.

Require authorization for account, payment, user-specific, or private actions.

Require human confirmation for actions that are financial, legal, externally visible, sensitive, or difficult to reverse.

Do not rely on ARP metadata alone to protect risky operations. Enforce rules at the endpoint.

## Common mistakes

- Marking a write action as safe.
- Omitting side effects.
- Publishing input schemas that do not match the backend.
- Allowing financial or legal actions without confirmation.
- Returning success without a receipt for important state changes.
- Changing action semantics without versioning.
