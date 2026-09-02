---
type: Architecture
title: Domino Key Rotation Architecture
description: 'The Domino Key Rotation Architecture describes a system for securely
  managing and rotating RSA cryptographic keys used for JWT token signing. The architecture
  involves two primary components: the Key Vault Manager, which generates and rotat'
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895304/Domino+Key+Rotation
tags:
- okf
- architecture
okf_schema: okf.concept.v1
identity:
  canonical_id: architecture.domino-key-rotation
  concept_type: architecture
  display_name: Domino Key Rotation Architecture
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:6a10e73caa9356d25de19152ee54b230e46c9f63fc7ee79885995ad85b9fd260
  last_updated_at: '2026-09-02T21:47:39.806815Z'
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
- type: describes
  target_canonical_id: external_system.azure-key-vault
- type: describes
  target_canonical_id: job.key-vault-manager
- type: describes
  target_canonical_id: service.token-data-service
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:47:39.806815Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Domino Key Rotation Architecture

## Summary

The Domino Key Rotation Architecture describes a system for securely managing and rotating RSA cryptographic keys used for JWT token signing. The architecture involves two primary components: the Key Vault Manager, which generates and rotates keys on a 90-day cycle, and the Token Data Service, which caches keys and uses them for token signing. Keys are stored in external_system.azure-key-vault and follow a staggered rotation schedule ensuring continuous availability with minimal overlap.

## Details

### Overview

The architecture ensures continuous availability of signing keys with a staggered rotation schedule. Keys are generated, rotated, and deleted according to a precise timeline that maintains overlap between old and new keys, allowing consumers to validate tokens signed with either key during the transition period.

### Key Vault Manager (job.key-vault-manager)

The Key Vault Manager is a Golang-based cron job that runs every 24 hours to manage the lifecycle of RSA key pairs in external_system.azure-key-vault. Its responsibilities include:

**Key Generation and Rotation:**
- Generates RSA256 public-private key pairs
- Stores both public and private PEM files as secrets in Azure Key Vault
- Follows a 90-day key lifecycle with staggered introduction and retirement

**Rotation Schedule:**
- Day 1: Introduces a new key pair
- Days 3–83: The new key serves as the primary signing key for 80 days
- Day 80: Introduces another new key pair, which remains inactive for 3 days
- Days 84–90: The original key remains in Azure Key Vault in an inactive state
- Day 91: Deletes the original key after reaching the end of its 90-day lifecycle

**Key Expiration and Age Tracking:**
- Each key has an associated creation date and 90-day expiration date
- KVM determines when to introduce new keys and delete expired ones based on age and expiration
- Does not manage primary or secondary status assignments—this is handled by service.token-data-service

### Token Data Service (service.token-data-service)

The Token Data Service is responsible for generating JWT tokens by signing them with keys from external_system.azure-key-vault and serving public keys in JWKS format. Its responsibilities include:

**Key Initialization and Caching:**
- On startup, retrieves all available keys from Azure Key Vault
- Assigns primary and secondary roles to keys based on their age
- Generates JWKS (JSON Web Key Set) for public keys
- Caches both JWKS and private PEM keys in memory, tagged as primary or secondary

**Cache Management:**
- In-memory cache has no TTL, as it is refreshed daily
- A separate goroutine refreshes the cache every 24 hours by:
  - Fetching updated keys from Azure Key Vault
  - Re-evaluating each key's primary or secondary status
  - Updating the cache with current key roles

**JWKS and Token Requests:**
- JWKS Requests: When service.partner-fulfillment-orchestrator requests JWKS, TDS serves the cached JWKS directly for low-latency responses
- Token Requests: For each token request, TDS retrieves the primary private key from the cache and signs the request payload with it

### Azure Key Vault (external_system.azure-key-vault)

Azure Key Vault serves as the central secure storage for all RSA key pairs. The Key Vault Manager creates and manages secrets representing the public and private PEM files. The Token Data Service retrieves keys from Azure Key Vault during startup and periodic cache refreshes.

### Integration with Partner Fulfillment Orchestrator

The service.partner-fulfillment-orchestrator consumes the JWKS and signed tokens produced by service.token-data-service. The architecture ensures that all keys available in the JWKS can validate tokens signed by the primary key, supporting a seamless key rotation process.

### Key Rotation Principles

**Overlap Strategy:**
- A new JWK is included in the JWKS for three days before becoming the primary signing key
- The old JWK remains in the JWKS for seven additional days after the new key becomes primary
- This overlap allows consumers to validate tokens signed with either key during the rotation period

**Partner Key Rotation Requirements:**
- A kid (key ID) must not be reused
- Any individual JWK identified by its kid claim must not live longer than 90 days
- A new JWK should not be used for signing JWTs until it has been confirmed that consumers have pulled the new JWK
- At a minimum, a new JWK should not be used for signing a JWT for at least one hour

**Verification:**
- Send a GET request to the entitlement API using a JWT signed by the new JWK
- Verify it does not return a 401 error response

### Service Principal Management

The service principal password rotates every 12 months. Thirty days before secret expiration, the password is automatically rotated, and an email notification is sent. The rotated secret must be manually set in Harness after rotation.

## Related Concepts

- service.partner-fulfillment-orchestrator
- external_system.azure-key-vault
- job.key-vault-manager
- service.token-data-service

## Sources

- Confluence page "Domino Key Rotation" (Space: BD, Page ID: 96895304)
