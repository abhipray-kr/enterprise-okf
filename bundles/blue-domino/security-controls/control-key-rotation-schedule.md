---
type: Security Control
title: Key Rotation Schedule
description: The Key Rotation Schedule is a security control that establishes a 90-day
  lifecycle for cryptographic keys used in JWT token signing and validation. This
  control ensures continuous availability of signing keys while maintaining security
  thr
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895304/Domino+Key+Rotation
tags:
- okf
- security_control
okf_schema: okf.concept.v1
identity:
  canonical_id: security_control.control-key-rotation-schedule
  concept_type: security_control
  display_name: Key Rotation Schedule
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:dc036d5447c1ff91ae25c3ce13d8f9cd07fa620e94f4530efc599bc258481ae6
  last_updated_at: '2026-09-02T22:00:14.340156Z'
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
- type: enforced_by
  target_canonical_id: job.key-vault-manager
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T22:00:14.340156Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Key Rotation Schedule

## Summary

The Key Rotation Schedule is a security control that establishes a 90-day lifecycle for cryptographic keys used in JWT token signing and validation. This control ensures continuous availability of signing keys while maintaining security through regular rotation, preventing key compromise and extending the lifetime of any single key.

## Details

### Rotation Lifecycle

The key rotation schedule operates on a 90-day cycle for each JSON Web Key (JWK):

- **Day 1**: A new JWK is introduced
- **Day 1-2**: The new JWK is added to the JWKS (JSON Web Key Set) for partner discovery
- **Day 3-83**: The new JWK becomes the primary signing key (80 days of active use)
- **Day 80**: Another new JWK is introduced, following the same pattern
- **Day 84-90**: The original JWK remains in the JWKS in an inactive state for backward compatibility
- **Day 91**: The original JWK is deleted after reaching the end of its 90-day lifecycle

### Key Characteristics

- No key identifier (kid) may be reused
- Any individual JWK must not live longer than 90 days
- New keys use an overlap strategy to ensure smooth transitions without service interruption
- A new JWK should not be used for signing until it has been confirmed that downstream partners have pulled the new key (minimum 1 hour recommended)

### Implementation Architecture

The control is enforced through two primary services:

**Token Data Service (TDS)** manages the in-memory cache:
- Retrieves all keys from Azure Key Vault on startup
- Assigns primary and secondary roles based on key age
- Generates and caches JWKS for public keys
- Refreshes the cache every 24 hours
- Serves cached JWKS with low latency
- Signs tokens using the primary private key

**Key Vault Manager (KVM)** handles key generation and rotation:
- Generates RSA256 public-private key pairs
- Stores both public and private PEM files as secrets in Azure Key Vault
- Executes every 24 hours as a scheduled cron job
- Tracks creation dates and 90-day expiration dates
- Automatically deletes expired keys after the rotation period
- Delegates primary/secondary role assignment to TDS based on key age

### Service Principal Password Rotation

Service principal passwords rotate every 12 months. Thirty days before expiration, the password is automatically rotated, requiring manual updates in Harness. Email notifications are sent upon rotation.

## Related Concepts

- `job.key-vault-manager` — The automated service enforcing this control through key generation, rotation, and deletion

## Sources

- Confluence page: Domino Key Rotation (Space: BD, Page ID: 96895304)
