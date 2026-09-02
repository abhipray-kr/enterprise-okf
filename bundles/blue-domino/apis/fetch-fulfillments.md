---
type: API Endpoint
title: Fetch Fulfillments
description: The Fetch Fulfillments endpoint (`/v1/fulfillments`) retrieves a list
  of fulfillments from the partner fulfillment service based on provided parameters.
  The endpoint supports optional retrieval of associated fulfillment codes and handles
  bo
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.fetch-fulfillments
  concept_type: api_route
  display_name: Fetch Fulfillments
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:b56c800f1c25bab751297d3e181fbbf6ef63e7a84b1c72f19ed2f96560422cba
  last_updated_at: '2026-09-02T21:45:54.408534Z'
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
  generated_at: '2026-09-02T21:45:54.408534Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Fetch Fulfillments

## Summary

The Fetch Fulfillments endpoint (`/v1/fulfillments`) retrieves a list of fulfillments from the partner fulfillment service based on provided parameters. The endpoint supports optional retrieval of associated fulfillment codes and handles both basic (59 flow) and non-basic (99 flow) fulfillment types, generating appropriate redirect URLs for each type.

## Details

### Endpoint Path
`/v1/fulfillments`

### Request Parameters
The endpoint accepts the following request parameters:
- `partner` - Partner identifier
- `fulfillment_type` - Type of fulfillment to filter by
- `customerId` - Customer identifier (optional for some operations)
- `offset` - Pagination offset
- `size` - Number of results to return
- `include` - Optional parameter to request additional data (supports `PartnerFulfillmentCodes`)

### Process Flow

#### Step 1: Setup Headers and Parameters
Prepare HTTP headers for the API call and define request parameters including partner, fulfillment_type, customerId, offset, and size.

#### Step 2: Call Partner Fulfillment Service
Send a request to the partner fulfillment service to fetch fulfillments based on the provided parameters.

#### Step 3: Handle Service Response
Unmarshal the service response into a PartnerFulfillmentResponse object and evaluate the response for errors.

#### Step 4: Conditional Fetch for Fulfillment Codes
If the `include` parameter contains `PartnerFulfillmentCodes`, proceed with the following sub-steps:

- **Distinguish Fulfillment Types**: Separate the fulfillment IDs into Basic (59 flow) and Non-Basic (99 flow) lists
- **Call Fulfillment Codes Service**: Request fulfillment codes without the customerId parameter
- **Process 59 Flow Logic**: For each basic fulfillment code retrieved, generate a redirect URL using the associated fulfillment and promo code
- **Process 99 Flow Logic**: For each non-basic fulfillment ID, generate a redirect URL without a promo code and store the fulfillment code details

#### Step 5: Return Results
Return the list of fulfillments and associated fulfillment codes.

### Error Handling
The endpoint captures and returns errors if any step of the fetching or processing fails. Error handling is applied at each stage of the request-response cycle.

## Related Concepts

- `event_topic.offer-interactions` — Event topic related to offer interactions
- `service.partner-fulfillment-data` — Data service that handles interactions with fulfillment database, codes, tokens, and external endpoints
- `service.partner-fulfillment-domain` — Domain service that handles fulfillment operations and external interactions

## Sources

**Primary Source**: Support Dashboard And Monitoring (Confluence Space: BD, Page ID: 96895288)
- Contains the complete technical flow description for the Fetch Fulfillments endpoint in the "Domain APIs Technical Flow Description" section
