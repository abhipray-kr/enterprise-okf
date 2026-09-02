---
type: Service
title: Token Data Service (TDS)
description: 'The Token Data Service (service.token-data-service) is responsible for
  generating JWT tokens by signing them and serving public keys in JWKS (JSON Web
  Key Set) format. It manages an in-memory cache of RSA key pairs retrieved from Azure
  Key '
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895304/Domino+Key+Rotation
tags:
- okf
- service
okf_schema: okf.concept.v1
identity:
  canonical_id: service.token-data-service
  concept_type: service
  display_name: Token Data Service (TDS)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:a344453a5377e94c6f165725fb6bf79d940ccaaa087ed748089aab373bb86fdb
  last_updated_at: '2026-09-02T22:02:27.128053Z'
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
- type: depends_on
  target_canonical_id: external_system.azure-key-vault
- type: implements
  target_canonical_id: api_route.get-jwks
- type: implements
  target_canonical_id: api_route.sign-token
- type: manages
  target_canonical_id: data_model.key-cache
- type: uses
  target_canonical_id: data_model.rsa-key-pair
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T22:02:27.128053Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Token Data Service (TDS)

## Summary

The Token Data Service (service.token-data-service) is responsible for generating JWT tokens by signing them and serving public keys in JWKS (JSON Web Key Set) format. It manages an in-memory cache of RSA key pairs retrieved from Azure Key Vault, maintaining primary and secondary key designations based on key age and rotation schedules. TDS provides low-latency access to both signing capabilities and public key material for downstream consumers.

## Details

### Key Initialization and Caching

On application startup, TDS retrieves all available keys from external_system.azure-key-vault. It assigns primary and secondary roles to the keys based on their age, following a defined rotation schedule. TDS generates JWKS for the public keys and caches them along with private PEM keys in memory, tagging each as primary or secondary as needed.

The key rotation schedule maintains a 90-day lifecycle for each key pair:
- New keys remain in JWKS for three days before becoming the primary signing key
- Keys serve as the primary signing key for 80 days
- Old keys remain in JWKS for seven days after demotion
- Keys are deleted after 90 days from creation

### Cache Management

The in-memory cache of data_model.rsa-key-pair entries does not have a time-to-live (TTL) setting, as it will be refreshed daily. A separate goroutine refreshes the cache every 24 hours by:
- Fetching updated keys from Azure Key Vault
- Re-evaluating each key's primary or secondary status
- Updating the data_model.key-cache accordingly

This ensures that TDS always has current key material without requiring constant access to the external key vault.

### JWKS and Token Requests

TDS implements two core operations:

**JWKS Requests:** When downstream services (such as service.partner-fulfillment-orchestrator) request the JWKS via api_route.get-jwks, TDS directly serves the cached JWKS, ensuring low-latency responses.

**Token Requests:** For each token signing request via api_route.sign-token, TDS retrieves the primary private key from the cache and signs the request payload with it. This avoids the need for real-time key vault access during token generation.

## Related Concepts

- external_system.azure-key-vault — Key storage and lifecycle management
- service.partner-fulfillment-orchestrator — Primary consumer of JWKS and token signing
- api_route.get-jwks — JWKS retrieval endpoint implemented by TDS
- api_route.sign-token — Token signing endpoint implemented by TDS
- data_model.key-cache — In-memory cache managed by TDS
- data_model.rsa-key-pair — Key material format used by TDS

## Sources

- Confluence page "Domino Key Rotation" (96895304, space BD): Describes TDS responsibilities, caching strategy, and token/JWKS serving mechanisms
