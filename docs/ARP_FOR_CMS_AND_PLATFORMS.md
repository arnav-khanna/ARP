# ARP for CMS and Platforms

Many sites should not hand-write ARP files. CMSs, ecommerce platforms, government publishing systems, and SaaS platforms can generate ARP from existing content models and APIs.

## Platform responsibilities

A platform integration should:

- Generate the discovery file.
- Generate resources from existing content and product models.
- Declare safe read actions first.
- Map authenticated operations to explicit actions.
- Publish governance policy settings.
- Validate output against ARP schemas.
- Keep ARP facts consistent with the source system.
- Give site owners controls for enabling or disabling ARP surfaces.

## WordPress-style CMS

A CMS plugin can map:

| CMS object | ARP representation |
| --- | --- |
| Post | `article` resource |
| Page | `webpage` resource |
| Author | `person` or `organization` resource |
| Category | Topic relationship |
| Media item | Media resource |
| Search | Safe query action |

The plugin should avoid exposing draft, private, password-protected, or member-only content unless authorization is enforced.

## Shopify-style ecommerce platform

An ecommerce app can map:

| Store object | ARP representation |
| --- | --- |
| Product | Product resource |
| Variant | Variant resource |
| Collection | Catalog resource |
| Cart operation | Cart action |
| Checkout operation | Checkout action requiring authorization and confirmation |
| Order | Authorized order resource |

The most useful first deployment is usually product discovery plus cart actions. Payment, checkout, returns, and account actions need stricter governance.

## Enterprise SaaS

A SaaS platform can expose ARP by mapping existing API operations into actions and existing objects into resources.

Recommended rollout:

- Public documentation resources.
- Safe search or lookup actions.
- Authenticated account resources.
- Low-risk state-changing actions.
- High-risk actions with confirmation, audit, and receipts.

## Government services

Government sites can use ARP to describe services and forms without requiring agents to infer workflows from page layouts.

Useful resources include:

- Service descriptions.
- Eligibility requirements.
- Required documents.
- Office locations.
- Appointment slots.
- Form schemas.
- Submission actions.
- Receipts and case status resources.

Government deployments should treat accessibility, auditability, privacy, and retention requirements as first-order design constraints.

## Static sites

Static sites can publish ARP with generated JSON files.

Good candidates:

- Documentation.
- Research papers.
- Public datasets.
- Portfolios.
- Blogs.
- Public knowledge bases.

Static sites usually start with discovery, article resources, topic relationships, and safe search.

## Site owner controls

Platforms should give site owners controls for:

- Enabling ARP.
- Selecting which content types are exposed.
- Setting governance policy values.
- Controlling agent access to actions.
- Reviewing generated resources.
- Disabling risky actions.
- Viewing agent traffic and errors.

