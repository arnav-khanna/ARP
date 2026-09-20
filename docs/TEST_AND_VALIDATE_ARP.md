# Test and Validate ARP [Work in Progress]

Before publishing ARP to production, test the documents, links, and action behavior.

## 1. Check JSON syntax

Every ARP file must be valid JSON.

Example:

```sh
jq . .well-known/arp.json
```

## 2. Validate against schemas

Validate discovery, resource, action, governance, and extension documents against the schemas in `schemas/`.

Recommended schema files:

- `schemas/discovery.schema.json`
- `schemas/resource.schema.json`
- `schemas/action.schema.json`
- `schemas/governance.schema.json`
- `schemas/extension.schema.json`
- `schemas/keys.schema.json`
- `schemas/receipt.schema.json`

## 3. Check URL behavior

Verify that:

- `/.well-known/arp.json` returns a successful status.
- Linked resource files resolve.
- Governance policy URLs resolve.
- Action bindings point to real endpoints.
- Redirects do not change the expected origin unexpectedly.
- Public ARP files are served over HTTPS.

## 4. Check resource consistency

For each important resource:

- The `id` is stable.
- The `url` points to the correct human-facing page when applicable.
- `types` are meaningful.
- `properties` match the visible site or source system.
- `relationships` point to valid targets.
- `actions` are relevant to the resource.
- Public files do not contain private data.

## 5. Check action safety

For each action:

- `safe` is true only for read-only operations.
- `sideEffects` are accurate.
- `authorization` matches backend enforcement.
- `requiresHumanConfirmation` is true for high-risk operations.
- Input schemas match backend validation.
- Output schemas match real responses.
- Important state-changing actions return receipts.

For signed resources and receipts, verify that every signature has `alg`, `keyId`, `signedPayload`, and `value`, and that `keyId` resolves to a matching `kid` in the published key document.

## 6. Simulate an agent path

Run through a complete task as an agent would:

1. Fetch `/.well-known/arp.json`.
2. Read governance policy.
3. Select a resource entrypoint.
4. Traverse relationships.
5. Select an action.
6. Validate action inputs.
7. Execute the action in staging.
8. Verify output or receipt.

This catches many issues that schema validation alone misses.

## 7. Test failure behavior

Test:

- Missing authorization.
- Invalid input.
- Rate-limit exceeded.
- Resource not found.
- Unsupported extension.
- Action temporarily unavailable.
- Policy conflict.

Agents need clear error responses to recover safely.

## Launch checklist

- Discovery file is available at `/.well-known/arp.json`.
- Governance policy is available and current.
- Resource entrypoints work.
- Schemas validate.
- Actions are safe-classified correctly.
- Sensitive operations require authorization.
- High-risk operations require confirmation.
- Monitoring is enabled.
- A contact address is available for agent operators.
