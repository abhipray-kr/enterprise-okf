---
type: API Endpoint
title: Create Fulfillment
description: The Create Fulfillment endpoint (`POST /v1/fulfillments`) initiates a
  new fulfillment record in the partner fulfillment system. It handles the complete
  workflow for creating fulfillments, including type assessment, promo code association,
  a
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.create-fulfillment
  concept_type: api_route
  display_name: Create Fulfillment
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:4c76e0e56608fba5a09159ffb0002b9addbd182714e0463bdf969bbb14b139fe
  last_updated_at: '2026-09-02T21:43:32.448585Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895288'
    page_title: Support Dashboard And Monotoring
    page_version: 11
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
    role: primary
relationships:
- type: consumes_event_topic
  target_canonical_id: event.topic-offer-interactions
- type: depends_on
  target_canonical_id: external_system.disney
- type: used_by
  target_canonical_id: service.partner-fulfillment-data
- type: used_by
  target_canonical_id: service.partner-fulfillment-domain
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:43:32.448585Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Create Fulfillment

## Summary

The Create Fulfillment endpoint (`POST /v1/fulfillments`) initiates a new fulfillment record in the partner fulfillment system. It handles the complete workflow for creating fulfillments, including type assessment, promo code association, and entitlement generation based on fulfillment classification (Basic vs. Non-Basic flows).

## Details

### Endpoint Path
```
POST /v1/fulfillments
```

### Process Flow

The endpoint executes the following steps:

1. **Create Partner Fulfillment**
   - Initiates a new fulfillment record in the underlying data service
   - Validates the fulfillment creation request

2. **Check Fulfillment Type**
   - Assesses the fulfillment type to determine processing path:
     - **Basic (59 flow)**: Proceeds to promo code association
     - **Non-Basic (99 flow)**: Follows alternative fulfillment handling

3. **Associate with Promo Code (Basic Fulfillment)**
   - Retrieves and updates promo codes linked to the fulfillment
   - Generates a redirect URL with the associated promo code

4. **Handle Non-Basic Fulfillment**
   - Generates an entitlement token
   - Creates an entitlement record
   - Generates a redirect URL for the fulfillment without a promo code

5. **Return Results**
   - Returns the created fulfillment record with the fulfillment code and redirect URL
   - Returns error response if any step fails

### Error Handling

The endpoint includes robust error handling:
- Immediate return of errors if fulfillment creation fails
- Capture and return of errors during promo code association
- Capture and return of errors during non-basic fulfillment handling
- Comprehensive error response with appropriate HTTP status codes

### Integration Points

- **service.partner-fulfillment-data**: Underlying data service that persists fulfillment records, codes, tokens, and Disney endpoint interactions
- **service.partner-fulfillment-domain**: Domain service that handles fulfillment business logic and external interactions
- **external_system.disney**: Disney partner system for entitlement creation and subscription activation

### Dependencies

The endpoint depends on successful interaction with external_system.disney for entitlement operations in non-basic fulfillment flows.

## Related Concepts

- [[service.partner-fulfillment-data]] — Data service that handles fulfillment persistence and Disney interactions
- [[service.partner-fulfillment-domain]] — Domain service managing fulfillment business logic
- [[external_system.disney]] — External Disney partner system for entitlements
- [[event.topic-offer-interactions]] — Event topic consumed by fulfillment orchestration

## Sources

**Provenance:**
- Source page: Confluence Space BD, page 96895288 "Support Dashboard And Monotoring" (primary)
  - Section: "Domain APIs Technical Flow Description"

**Quality:** Pending

**Last updated:** Unknown
