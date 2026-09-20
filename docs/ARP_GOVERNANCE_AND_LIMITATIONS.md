# ARP Governance and Limitations

ARP governance tells well-behaved agents how a site expects its agent interface to be used.

Governance can describe rate limits, audit requirements, training-use preferences, contact points, revocation URLs, reputation registries, authorization expectations, and action-specific constraints.

## Example governance policy

```json
{
  "policyVersion": "2026.07.05",
  "rateLimits": [
    {
      "tier": "default",
      "limit": 600,
      "period": "1h"
    },
    {
      "tier": "verified-agent",
      "limit": 6000,
      "period": "1h"
    }
  ],
  "trainingUse": "contact-required",
  "auditRequiredFor": ["financial", "legal", "sensitive-data"],
  "retention": {
    "actionReceipts": "P2Y"
  },
  "contact": "mailto:agent-policy@example.com",
  "revocation": "https://example.com/.well-known/arp-revocation.json",
  "reputationRegistries": [
    "https://registry.example/agents"
  ]
}
```

## What governance can express

| Governance area | Example |
| --- | --- |
| Rate limits | How many requests a default or verified agent may make. |
| Authorization | Which actions require credentials or scopes. |
| Audit | Which action classes require receipts or logs. |
| Training use | Whether separate permission is requested for training datasets. |
| Retention | How long action records should be retained. |
| Contact | Where agents or operators can ask policy questions. |
| Revocation | Where permission or policy changes are announced. |
| Reputation | Which registries a site recognizes for agent identity or trust. |

## Important limitations

ARP is voluntary. It helps cooperating agents and sites coordinate, but it does not force every crawler, scraper, or agent to behave correctly.

ARP is not a privacy boundary. Do not publish secrets, private account data, payment details, health data, legal records, or user-specific resources in public files.

ARP is not a substitute for authentication. Any action that requires identity must verify identity at execution time.

ARP is not a substitute for authorization. Every sensitive endpoint must enforce permissions even if the ARP policy says the action is restricted.

ARP is not a substitute for rate limiting. Sites should still enforce rate limits at the network and application layers.

## Recommended policy behavior for agents

Agents should:

- Read the governance policy before traversing resources or executing actions.
- Respect rate limits and retry guidance.
- Avoid executing actions outside declared scopes.
- Preserve receipts for important state-changing actions.
- Identify themselves when required.
- Stop or degrade behavior when policy changes remove permission.
- Treat missing policy as uncertainty, not permission to behave aggressively.

## Recommended policy behavior for sites

Sites should:

- Publish clear and stable policies.
- Enforce sensitive requirements on the server.
- Keep policy contact information current.
- Separate read-only discovery from privileged account resources.
- Log state-changing actions.
- Provide actionable errors when agents exceed limits or lack authorization.

