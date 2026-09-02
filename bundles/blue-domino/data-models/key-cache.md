---
type: Data Model
title: In-Memory Key Cache
description: The In-Memory Key Cache is a caching mechanism managed by the Token Data
  Service (TDS) that stores cryptographic keys retrieved from Azure Key Vault. The
  cache holds both public keys in JSON Web Key Set (JWKS) format and private PEM keys,
  e
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895304/Domino+Key+Rotation
tags:
- okf
- data_model
okf_schema: okf.concept.v1
identity:
  canonical_id: data_model.key-cache
  concept_type: data_model
  display_name: In-Memory Key Cache
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:42d75d640c45f6fb8ed8c09c3b94293d362c007ff47039ed4a95670fbcf1715d
  last_updated_at: '2026-09-02T21:50:46.671375Z'
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
- type: managed_by
  target_canonical_id: service.token-data-service
- type: sourced_from
  target_canonical_id: external_system.azure-key-vault
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:50:46.671375Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# In-Memory Key Cache

## Summary

The In-Memory Key Cache is a caching mechanism managed by the Token Data Service (TDS) that stores cryptographic keys retrieved from Azure Key Vault. The cache holds both public keys in JSON Web Key Set (JWKS) format and private PEM keys, each tagged with a primary or secondary role designation. The cache is refreshed every 24 hours to maintain synchronized key state with the source key vault system.

## Details

### Initialization and Structure

On application startup, TDS retrieves all available keys from Azure Key Vault and assigns primary and secondary roles to the keys based on their age, following a 90-day rotation schedule. The cache stores:

- JWKS (JSON Web Key Set) containing public keys for external consumption
- Private PEM keys used for JWT token signing
- Role designations (primary or secondary) for each key

### Cache Management

The in-memory cache operates without a time-to-live (TTL) setting, relying instead on a scheduled refresh mechanism. A separate goroutine refreshes the cache every 24 hours by:

1. Fetching updated keys from Azure Key Vault
2. Re-evaluating each key's primary or secondary status based on key age
3. Updating the cache with current key state

### Key Lifecycle Support

The cache maintains keys through a 90-day lifecycle, with keys transitioning through the following states:

- Days 1-2: New key inactive (pending primary status)
- Days 3-83: Key active as primary signing key (80-day active period)
- Days 80+: New key introduced while previous key remains cached
- Days 84-90: Previous key remains inactive in cache
- Day 91+: Expired keys removed from cache

### Operational Usage

**JWKS Requests:** When external systems request the JWKS, TDS directly serves the cached public keys, ensuring low-latency responses without accessing the external key vault.

**Token Requests:** For each token signing request, TDS retrieves the current primary private key from the cache and signs the request payload with it.

### Performance and Availability

By maintaining a refreshed in-memory copy of keys, the cache enables:

- Low-latency token generation and JWKS serving
- Reduced dependency on external Azure Key Vault access frequency
- Consistent availability of signing keys across the 24-hour refresh interval

## Related Concepts

- `service.token-data-service` — manages and utilizes the in-memory key cache
- `external_system.azure-key-vault` — source of all cached keys

## Sources

Primary source: Confluence page "Domino Key Rotation" (Space: BD, Page ID: 96895304)
