---
type: API Endpoint
title: Update Fulfillment
description: 'The Update Fulfillment endpoint (`/v1/fulfillments/{id}`) allows modification
  of an existing fulfillment record in the partner fulfillment system. This endpoint
  validates the update request, modifies fulfillment details, handles associated '
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.update-fulfillment
  concept_type: api_route
  display_name: Update Fulfillment
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:5e3106de0ffcbeda6a90905515233bdb35abd162c793481e0f30f942d1aecbda
  last_updated_at: '2026-09-02T21:46:45.766970Z'
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
- type: used_by
  target_canonical_id: service.partner-fulfillment-data
- type: used_by
  target_canonical_id: service.partner-fulfillment-domain
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:46:45.766970Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Update Fulfillment

## Summary

The Update Fulfillment endpoint (`/v1/fulfillments/{id}`) allows modification of an existing fulfillment record in the partner fulfillment system. This endpoint validates the update request, modifies fulfillment details, handles associated promo code updates, and persists the changes to the underlying data service.

## Details

### Endpoint Path
`PATCH /v1/fulfillments/{id}`

### Process Flow

The update operation follows a structured six-step process:

**1. Retrieve Current Fulfillment Record**
- Fetch the existing fulfillment record based on the provided identifier
- The record must exist in the underlying data service

**2. Validate Update Request**
- Ensure the fulfillment record exists before proceeding
- Validate any new data against established business rules
- Check the validity of all update parameters

**3. Update Fulfillment Details**
- Modify the necessary fields in the fulfillment record based on the update request
- Apply changes according to the validated request data

**4. Handle Promo Code Updates (If Applicable)**
- When applicable, update associated promo codes for the fulfillment
- Retrieve the current promo codes linked to the fulfillment
- Apply necessary updates or deletions to promo code associations

**5. Save Changes**
- Commit the changes to the underlying data service
- Ensure all updates are properly saved and reflected in the database

**6. Return Results**
- Return the updated fulfillment record on success
- Return an appropriate error response if any step fails

### Error Handling

The endpoint includes comprehensive error handling at multiple stages:

- **Immediate return of errors** if the retrieval of the fulfillment record fails
- **Capture and return of errors** during validation, updating details, or saving changes
- All errors follow standard error code conventions

## Related Concepts

- [[service.partner-fulfillment-data]] — Data service that handles interactions with fulfillment database, promo codes, tokens, and external endpoints
- [[service.partner-fulfillment-domain]] — Domain service that orchestrates fulfillment business logic and external interactions

## Sources

- Support Dashboard And Monitoring (Confluence, page 96895288): Domain APIs Technical Flow Description, containing full technical specifications for the Update/Patch Fulfillment endpoint
