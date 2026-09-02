---
type: API Endpoint
title: Sign Token Endpoint
description: 'The Sign Token Endpoint is an API route provided by the Token Data Service
  (TDS) that generates signed JWT tokens for use by partner systems. For each token
  request, the endpoint retrieves the primary private key from TDS''s in-memory cache '
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895304/Domino+Key+Rotation
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.sign-token
  concept_type: api_route
  display_name: Sign Token Endpoint
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:99a5823ebc0461e04404aa33f63f88ba58b6f75fb62ea18c66078d4c0f403025
  last_updated_at: '2026-09-02T21:46:29.771947Z'
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
- type: implemented_by
  target_canonical_id: service.token-data-service
- type: uses
  target_canonical_id: data_model.rsa-key-pair
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:46:29.771947Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Sign Token Endpoint

## Summary

The Sign Token Endpoint is an API route provided by the Token Data Service (TDS) that generates signed JWT tokens for use by partner systems. For each token request, the endpoint retrieves the primary private key from TDS's in-memory cache and signs the request payload with it using RSA256 cryptographic signing.

## Details

### Purpose

The Sign Token Endpoint handles token generation requests from partner systems such as the Partner Fulfillment Orchestrator. It enables secure JWT token creation by leveraging cryptographic signing with actively managed RSA key pairs.

### Operation

When a token request is received, the endpoint:

1. Retrieves the primary private key from the in-memory cache maintained by TDS
2. Signs the request payload using the primary key
3. Returns the signed JWT token to the requesting client

The endpoint does not manage key rotation directly; instead, it relies on the cache maintained by TDS, which is populated with keys retrieved from Azure Key Vault and tagged as primary or secondary based on their age.

### Key Rotation Integration

The endpoint operates within the context of the key rotation schedule managed by the Key Vault Manager (KVM) and TDS. Key rotation follows a 90-day lifecycle:

- New keys are introduced and remain inactive for 3 days
- Keys become active as the primary signing key for 80 days
- When a new key takes over, the previous key remains in the system for 7 additional days for verification purposes
- Keys are automatically deleted after 90 days

Partner systems must validate that new signing keys have been pulled before being used for signing operations, typically by sending a test request and confirming it does not return a 401 error.

### Security Considerations

- Service principal passwords associated with the endpoint are rotated every 12 months
- 30 days before password expiration, the system automatically rotates the password and sends email notification
- Manual configuration updates in Harness may be required after password rotation

## Related Concepts

- `service.token-data-service` — Implements this endpoint and manages token signing operations
- `data_model.rsa-key-pair` — The cryptographic key material used for signing operations

## Sources

- Page: "Domino Key Rotation" (Confluence Space BD, Page ID 96895304)
  - Sections: Token Data Service (TDS), Key Vault Manager (KVM), Key rotation schedule
