# ARBench Results

Generated: 2026-07-04 18:08 UTC

## Summary Table

| Task | Agent | Tokens | Requests | Latency (s) | Receipts | Success |
|------|-------|-------:|---------:|------------:|---------:|---------|
| product_lookup | Browser | 76,020 | 3 | 0.0 | 0 | Y |
| product_lookup | **ARP** | **2,281** | **5** | **0.0** | 1 | Y |
| hotel_compare | Browser | 85,868 | 4 | 0.0 | 0 | Y |
| hotel_compare | **ARP** | **2,554** | **6** | **0.0** | 0 | Y |
| gov_form | Browser | 103,591 | 6 | 0.0 | 0 | Y |
| gov_form | **ARP** | **2,184** | **5** | **0.0** | 1 | Y |
| saas_downgrade | Browser | 94,430 | 6 | 0.0 | 0 | Y |
| saas_downgrade | **ARP** | **2,640** | **5** | **0.0** | 1 | Y |
| docs_lookup | Browser | 83,721 | 4 | 0.0 | 0 | Y |
| docs_lookup | **ARP** | **1,610** | **4** | **0.0** | 0 | Y |

## Efficiency Gains (ARP vs Browser)

| Task | Token Reduction | Request Reduction | Latency Reduction |
|------|----------------:|------------------:|------------------:|
| product_lookup | 97.0% | -66.7% | 38.4% |
| hotel_compare | 97.0% | -50.0% | 52.4% |
| gov_form | 97.9% | 16.7% | 58.9% |
| saas_downgrade | 97.2% | 16.7% | 59.1% |
| docs_lookup | 98.1% | 0.0% | 66.5% |

---

> **Note:** These are empirically measured estimates (via tiktoken against realistic HTML fixtures) for benchmark design purposes.
> Real-world results will vary by site, agent, and network conditions.

## Step-by-Step Traces

### product_lookup

**Browser Agent Steps:**
  1. [http_request] Navigate to product page (2,020 tokens)
  2. [js_execute] Wait for JS to render variant selector and price
  3. [screenshot] Screenshot product page for visual understanding (15,000 tokens)
  4. [llm_inference] Parse DOM: find product name, price, availability, variants (12,000 tokens)
  5. [llm_inference] Infer: select variant -> click 'Add to Cart' button (8,000 tokens)
  6. [http_request] Navigate to blue/medium variant page (8,000 tokens)
  7. [js_execute] Wait for variant page to render
  8. [llm_inference] Confirm variant selection and availability (12,000 tokens)
  9. [form_fill] Fill quantity=1, click 'Add to Cart' (3,000 tokens)
  10. [http_request] Submit form
  11. [js_execute] Wait for cart update response
  12. [llm_inference] Verify cart updated (parse cart count / confirmation toast) (12,000 tokens)
  13. [llm_inference] Confirm item appears in cart (4,000 tokens)

**ARP Agent Steps:**
  1. [http_request] GET /.well-known/arp.json (400 tokens)
  2. [http_request] GET /.well-known/arp-policy.json (200 tokens)
  3. [http_request] GET /products/rain-jacket.agent.json (581 tokens)
  4. [http_request] Follow hasVariant → /products/rain-jacket-blue-m.agent.json (600 tokens)
  5. [action_plan] Plan inputs for action 'cart.addItem' from declared schema (200 tokens)
  6. [http_request] POST cart.addItem {variantId: rain-jacket-blue-m, quantity: 1}
  7. [receipt_verify] Verify signed receipt (300 tokens)

### hotel_compare

**Browser Agent Steps:**
  1. [http_request] Navigate to hotel search results (2,717 tokens)
  2. [js_execute] Wait for search results to render (lazy-loaded)
  3. [screenshot] Screenshot search results for visual parsing (15,000 tokens)
  4. [llm_inference] Parse search results: identify top 3 hotels, extract names and prices (12,000 tokens)
  5. [http_request] Navigate to hotel 1 detail page (2,717 tokens)
  6. [js_execute] Wait for hotel 1 page JS (availability calendar, etc.)
  7. [llm_inference] Extract hotel 1 details: amenities, room types, cancellation policy (12,000 tokens)
  8. [http_request] Navigate to hotel 2 detail page (2,717 tokens)
  9. [js_execute] Wait for hotel 2 page JS (availability calendar, etc.)
  10. [llm_inference] Extract hotel 2 details: amenities, room types, cancellation policy (12,000 tokens)
  11. [http_request] Navigate to hotel 3 detail page (2,717 tokens)
  12. [js_execute] Wait for hotel 3 page JS (availability calendar, etc.)
  13. [llm_inference] Extract hotel 3 details: amenities, room types, cancellation policy (12,000 tokens)
  14. [llm_inference] Compare 3 hotels across price, amenities, cancellation â€” select best option (8,000 tokens)
  15. [llm_inference] Verify comparison data is complete and consistent (4,000 tokens)

**ARP Agent Steps:**
  1. [http_request] GET /.well-known/arp.json (400 tokens)
  2. [http_request] GET /.well-known/arp-policy.json (200 tokens)
  3. [action_plan] Plan inputs for action 'hotels.search' from declared schema (200 tokens)
  4. [http_request] POST hotels.search {checkin: 2026-08-01, checkout: 2026-08-05, guests: 2}
  5. [http_request] GET hotel 1 resource (518 tokens)
  6. [http_request] GET hotel 2 resource (518 tokens)
  7. [http_request] GET hotel 3 resource (518 tokens)
  8. [action_plan] Plan inputs for action 'hotel.checkAvailability' from declared schema (200 tokens)

### gov_form

**Browser Agent Steps:**
  1. [http_request] Navigate to permit application portal (1,591 tokens)
  2. [js_execute] Wait for portal login check and form to load
  3. [screenshot] Screenshot application form for visual analysis (15,000 tokens)
  4. [llm_inference] Parse form: identify fields, labels, required markers, help text (12,000 tokens)
  5. [llm_inference] Determine field order, conditional fields, and required attachments (8,000 tokens)
  6. [form_fill] Fill form section: Property Information (3,000 tokens)
  7. [http_request] Submit form
  8. [js_execute] Wait for validation and next-step navigation (Property Information)
  9. [llm_inference] Verify Property Information section accepted, check for errors (12,000 tokens)
  10. [form_fill] Fill form section: Work Description (3,000 tokens)
  11. [http_request] Submit form
  12. [js_execute] Wait for validation and next-step navigation (Work Description)
  13. [llm_inference] Verify Work Description section accepted, check for errors (12,000 tokens)
  14. [form_fill] Fill form section: Contractor Details (3,000 tokens)
  15. [http_request] Submit form
  16. [js_execute] Wait for validation and next-step navigation (Contractor Details)
  17. [llm_inference] Verify Contractor Details section accepted, check for errors (12,000 tokens)
  18. [form_fill] Fill form section: Fee Calculation (3,000 tokens)
  19. [http_request] Submit form
  20. [js_execute] Wait for validation and next-step navigation (Fee Calculation)
  21. [llm_inference] Verify Fee Calculation section accepted, check for errors (12,000 tokens)
  22. [form_fill] Submit draft (save for later) (3,000 tokens)
  23. [http_request] Submit form
  24. [llm_inference] Verify draft saved and confirmation number shown (4,000 tokens)

**ARP Agent Steps:**
  1. [http_request] GET /.well-known/arp.json (400 tokens)
  2. [http_request] GET /.well-known/arp-policy.json (200 tokens)
  3. [http_request] GET permit resource (fields, eligibility, required docs) (484 tokens)
  4. [http_request] Follow hasRequiredDocument → /docs/building-requirements.agent.json (600 tokens)
  5. [action_plan] Plan inputs for action 'permit.saveDraft' from declared schema (200 tokens)
  6. [http_request] POST permit.saveDraft {description, estimatedCost, attachments}
  7. [receipt_verify] Verify receipt (300 tokens)

### saas_downgrade

**Browser Agent Steps:**
  1. [http_request] Navigate to SaaS dashboard (1,886 tokens)
  2. [js_execute] Wait for SPA data load
  3. [screenshot] Screenshot dashboard to find settings/billing (15,000 tokens)
  4. [llm_inference] Parse dashboard DOM: locate user menu -> billing settings (12,000 tokens)
  5. [http_request] Click profile avatar dropdown (1,886 tokens)
  6. [http_request] Navigate to billing/subscription page (1,886 tokens)
  7. [js_execute] Wait for billing page JS (current plan, payment info)
  8. [screenshot] Screenshot billing page to understand current plan (15,000 tokens)
  9. [llm_inference] Parse billing page: find current plan and 'Change Plan' button (12,000 tokens)
  10. [http_request] Click 'Change Plan' (1,886 tokens)
  11. [js_execute] Wait for plans modal to open
  12. [llm_inference] Locate 'Free' tier button (12,000 tokens)
  13. [http_request] Click 'Downgrade to Free' (1,886 tokens)
  14. [js_execute] Wait for confirmation step to load
  15. [llm_inference] Parse confirmation page: verify plan name, effective date, lost features (12,000 tokens)
  16. [form_fill] Confirm downgrade (click 'Confirm' button) (3,000 tokens)
  17. [http_request] Submit form
  18. [js_execute] Wait for downgrade to process
  19. [llm_inference] Verify plan changed to Starter in billing page (4,000 tokens)

**ARP Agent Steps:**
  1. [http_request] GET /.well-known/arp.json (400 tokens)
  2. [http_request] GET /.well-known/arp-policy.json (200 tokens)
  3. [http_request] GET subscription resource (790 tokens)
  4. [http_request] Follow hasAvailablePlan → /plans.agent.json (600 tokens)
  5. [action_plan] Plan inputs for action 'subscription.downgrade' from declared schema (200 tokens)
  6. [human_confirmation] Request human confirmation: downgrade is financial + state-changing action (150 tokens)
  7. [http_request] POST subscription.downgrade {targetPlanId: starter, effectiveImmediately: false}
  8. [receipt_verify] Verify signed receipt (300 tokens)

### docs_lookup

**Browser Agent Steps:**
  1. [http_request] Navigate to documentation home (1,907 tokens)
  2. [js_execute] Wait for docs site JS framework to load
  3. [screenshot] Screenshot docs home to find search interface (15,000 tokens)
  4. [llm_inference] Locate search box in docs navigation (12,000 tokens)
  5. [form_fill] Type query: 'How do I configure rate limiting?' and submit (3,000 tokens)
  6. [http_request] Submit form
  7. [js_execute] Wait for search results to render
  8. [llm_inference] Parse search results: identify most relevant page(s) (12,000 tokens)
  9. [http_request] Navigate to candidate doc page 1 (1,907 tokens)
  10. [js_execute] Wait for doc page 1 to load
  11. [llm_inference] Parse doc page 1: extract rate limiting configuration steps (12,000 tokens)
  12. [http_request] Navigate to candidate doc page 2 (1,907 tokens)
  13. [js_execute] Wait for doc page 2 to load
  14. [llm_inference] Parse doc page 2: extract rate limiting configuration steps (12,000 tokens)
  15. [llm_inference] Synthesize answer from both pages, determine canonical source (8,000 tokens)
  16. [llm_inference] Verify answer is complete and source URL is correct (4,000 tokens)

**ARP Agent Steps:**
  1. [http_request] GET /.well-known/arp.json (400 tokens)
  2. [http_request] GET /.well-known/arp-policy.json (200 tokens)
  3. [action_plan] Plan inputs for action 'docs.search' from declared schema (200 tokens)
  4. [http_request] POST docs.search {query: 'How do I configure rate limiting?'}
  5. [http_request] GET /guides/rate-limiting.agent.json — structured answer + source (610 tokens)
  6. [answer_extraction] Extract answer from resource.properties.content and resource.url for citation (200 tokens)
