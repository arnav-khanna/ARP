# ARP Overview

Agent Resource Protocol (ARP) is a proposed website-origin protocol for the Agent Web.

The Web that humans use is built from pages, visual controls, links, forms, and JavaScript. Agents can operate that Web, but they must infer meaning from artifacts that were designed for people. ARP gives websites a native way to publish agent-readable structure.

An ARP file tells autonomous agents which resources, relationships, actions, and policies a website exposes for machine interaction.

For example, a shopping site can use ARP to tell agents where product resources are, how variants are represented, which cart actions are available, and which policies apply to checkout. A government site can use ARP to describe forms, eligibility requirements, submission actions, and audit requirements.


## One-Sentence Definition

ARP represents a website as a graph of resources, relationships, actions, and policies so autonomous agents can understand, navigate, and act safely.

## The Agent Web

```text
Human Web                  Agent Web
---------                  ---------
HTML                       ARP resources
CSS                        Typed properties
JavaScript                 Relationships
Forms and buttons          Actions
Terms and bot defenses     Policies and receipts
```

The Agent Web does not replace the Human Web. It runs beside it, published by the same origins and governed by the same site owners.

## Why It Matters

Browser automation asks agents to behave like very fast, literal humans:

1. Load a page.
2. Wait for JavaScript.
3. Inspect the DOM.
4. Infer semantics.
5. Click controls.
6. Observe what changed.

ARP exposes the structure directly:

1. Discover the ARP endpoint.
2. Fetch a resource.
3. Follow typed relationships.
4. Validate a declared action.
5. Execute the action.
6. Verify a receipt.

## The Four Core Primitives

| Primitive | Purpose |
| --- | --- |
| Resource | Identifies an entity, page, object, workflow, account, or capability |
| Property | Describes a resource with typed data |
| Relationship | Connects resources into a semantic graph |
| Action | Defines what an agent can do and what will happen |

## What ARP is used for

ARP is used to help agents:

- Discover a website's machine-readable interface.
- Understand website entities as resources instead of visual pages.
- Follow semantic relationships between resources.
- Execute declared actions with known inputs, outputs, and side effects.
- Respect site governance policies such as rate limits, authorization rules, and audit requirements.
- Avoid brittle browser automation when a direct agent interface is available.

ARP is especially useful when a website expects repeated agent access, structured tasks, transactional workflows, or domain-specific data that is difficult to infer from HTML alone.

## What ARP is not used for

ARP is not an access-control system. If a resource, action, or file must be private, protect it with authentication, authorization, and server-side enforcement.

ARP is not a replacement for human-facing HTML pages. It complements the human Web by adding an agent-facing layer.

ARP is not a guarantee that every bot will behave correctly. Like other voluntary web protocols, it works when agents choose to respect it and sites enforce sensitive operations on the server.

ARP is not limited to search indexing. It is designed for discovery, navigation, action execution, governance, and domain extension.

## Where agents find ARP

The standard starting point is:

```text
https://example.com/.well-known/arp.json
```

This discovery file tells agents which ARP versions the site supports, which capabilities are available, where governance policies live, and how to find resources.

## How ARP affects different site content

| Site content | How ARP can help agents |
| --- | --- |
| Web pages | Links a human page to structured resources and actions. |
| Products | Describes price, availability, variants, reviews, cart actions, and checkout policies. |
| Articles | Describes author, publication date, topic, citations, related resources, and licensing policy. |
| Forms | Describes fields, validation rules, eligibility constraints, submission actions, and receipts. |
| Accounts | Describes account resources only after proper authorization. |
| APIs | Wraps API operations in agent-readable actions with safety and governance metadata. |
| Policies | States usage rules, rate limits, training-use preferences, contact points, and audit requirements. |

## If you use a CMS or hosted platform

If your site runs on a CMS, ecommerce platform, or website builder, you may not need to edit ARP files directly. A platform plugin or app can generate discovery files, resource files, and actions from existing content models.

Before creating ARP files manually, check whether your platform already provides ARP settings or an ARP integration.

## Common limitations

ARP only describes the agent-facing contract your site publishes. It does not make unsafe backend operations safe by itself.

Agents may support different ARP versions or extensions. Sites should publish conservative core resources first and use extensions for domain-specific behavior.

The current core version is ARP `1.1`. Receipt envelopes use their own version field and currently use `1.0`.

Sites should avoid exposing sensitive data in public ARP files. Anything requiring user identity, payment, account access, or legal consent should require authorization and, when appropriate, human confirmation.
