---
type: Data Model
title: JWKS Response Format
description: JWKS Response Format is a JSON Web Key Set that contains the public keys
  used for verifying JSON Web Tokens (JWTs). It is generated and served by the Token
  Data Service to allow external consumers to validate JWT signatures.
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895304/Domino+Key+Rotation
tags:
- okf
- data_model
okf_schema: okf.concept.v1
identity:
  canonical_id: data_model.jwks-response
  concept_type: data_model
  display_name: JWKS Response Format
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:1ee144b508b2ab63e3113b5b00fb24001b1b4fe8da0b6c84cfa176eace1db9af
  last_updated_at: '2026-09-02T21:50:28.423361Z'
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
- type: generated_by
  target_canonical_id: service.token-data-service
- type: returned_by
  target_canonical_id: api_route.get-jwks
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:50:28.423361Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# JWKS Response Format

## Summary

JWKS Response Format is a JSON Web Key Set that contains the public keys used for verifying JSON Web Tokens (JWTs). It is generated and served by the Token Data Service to allow external consumers to validate JWT signatures.

## Details

The JWKS Response is a standardized JSON structure containing an array of JSON Web Keys (JWKs), each identified by a key ID (kid) and containing cryptographic material for JWT verification.

### Structure

The JWKS format includes the following components:

- **Key ID (kid)**: A unique identifier for each JWK, used to match signatures in JWTs with the appropriate verification key
- **Algorithm (alg)**: The cryptographic algorithm used for signing, typically RSA256
- **Public Key Material**: The public portion of the RSA256 key pair, used for JWT verification

### Generation and Caching

The Token Data Service generates JWKS by converting cached public keys into the JWKS format on application startup. The public keys are retrieved from Azure Key Vault and cached in memory, with each key tagged as either primary or secondary based on its age and position in the rotation schedule.

### Key Lifecycle in JWKS

According to the key rotation schedule:

- A new JWK is placed in the JWKS for 3 days before it becomes the primary signing key
- The primary signing JWK is used for 80 days
- After rotation, the old JWK remains in the JWKS for 7 additional days
- A JWK has a total lifespan of 90 days before being removed from the JWKS

### Serving and Consumption

When consumers request the JWKS, the Token Data Service directly serves the cached JWKS from memory, ensuring low-latency responses. This allows external systems to fetch the current set of public keys needed to verify JWTs signed by the service without direct access to the underlying key management infrastructure.

## Related Concepts

- **service.token-data-service** — Generates and caches the JWKS, serving it to consumers; manages the in-memory cache of public keys with daily refresh from Azure Key Vault
- **api_route.get-jwks** — The API endpoint that returns this response format to consumers requesting public keys for JWT verification

## Sources

- Confluence page: "Domino Key Rotation" (Space: BD, Page ID: 96895304)
