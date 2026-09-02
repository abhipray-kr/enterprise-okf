---
type: API Endpoint
title: Get JWKS Endpoint
description: The Get JWKS Endpoint is an API route provided by service.token-data-service
  that serves a JSON Web Key Set (JWKS) containing the public keys used for JWT token
  verification. This endpoint delivers cached public keys in JWKS format with low
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895304/Domino+Key+Rotation
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.get-jwks
  concept_type: api_route
  display_name: Get JWKS Endpoint
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:fe2f7b4a22d9e4da4a5f708ed7723de80de0722ffeaf66e747c58a32a990b2d3
  last_updated_at: '2026-09-02T21:46:12.446562Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895304'
    page_title: Domino Key Rotation
    page_version: 6
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895304/Domino+Key+Rotation
    role: primary
relationships:
- type: consumed_by
  target_canonical_id: service.partner-fulfillment-orchestrator
- type: implemented_by
  target_canonical_id: service.token-data-service
- type: returns
  target_canonical_id: data_model.jwks-response
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:46:12.446562Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Get JWKS Endpoint

## Summary

The Get JWKS Endpoint is an API route provided by service.token-data-service that serves a JSON Web Key Set (JWKS) containing the public keys used for JWT token verification. This endpoint delivers cached public keys in JWKS format with low-latency responses to requesting services, particularly service.partner-fulfillment-orchestrator.

## Details

### Purpose

The JWKS endpoint enables external services to retrieve the public keys necessary for validating JWT tokens signed by service.token-data-service. This is a critical component of the token verification workflow in the Domino key rotation system.

### Request Handling

When service.partner-fulfillment-orchestrator requests the JWKS, service.token-data-service directly serves the cached JWKS, ensuring low-latency responses. The endpoint does not require any parameters and returns the complete set of currently valid public keys.

### Response Format

The endpoint returns a data_model.jwks-response containing a JSON Web Key Set. This includes:

- Primary public keys currently used for token signing
- Secondary public keys maintained during key rotation transitions
- Each key identified by its `kid` (key identifier) claim
- Key metadata necessary for JWT validation

### Key Caching and Refresh

service.token-data-service maintains an in-memory cache of JWKS that is refreshed every 24 hours. The cache includes:

- Public keys derived from RSA key pairs stored in Azure Key Vault
- Primary and secondary role designations assigned based on key age
- No time-to-live (TTL) setting on individual cache entries; instead, the entire cache is refreshed daily

### Key Rotation Context

The JWKS endpoint supports a 90-day key rotation schedule:

- New keys appear in the JWKS for 3 days before becoming the primary signing key
- The primary signing key is active for 80 days
- Previous keys remain in the JWKS for 7 additional days after being superseded
- Keys are removed from the JWKS after 90 days total

### Performance Characteristics

The endpoint provides low-latency responses by serving pre-generated and cached JWKS rather than dynamically constructing the response on each request. This design ensures consistent performance even under high request volume.

## Related Concepts

- service.partner-fulfillment-orchestrator — consumes this endpoint to retrieve public keys
- service.token-data-service — implements and serves this endpoint
- data_model.jwks-response — the response format returned by this endpoint

## Sources

- Domino Key Rotation (Confluence page 96895304, BD space) — describes the Token Data Service responsibilities including JWKS caching and serving behavior
