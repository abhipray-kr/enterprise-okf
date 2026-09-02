---
type: External System
title: Azure Key Vault
description: Azure Key Vault is the central secure storage system for cryptographic
  key material used in token generation and JWT signing operations. It maintains RSA256
  public-private key pairs throughout their lifecycle, supporting a 90-day key rotati
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895304/Domino+Key+Rotation
tags:
- okf
- external_system
okf_schema: okf.concept.v1
identity:
  canonical_id: external_system.azure-key-vault
  concept_type: external_system
  display_name: Azure Key Vault
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:95e3d0abdee7b91c7a03945580220e57568df4b9e9a6a57eb44fc227213736c4
  last_updated_at: '2026-09-02T21:55:43.350763Z'
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
  target_canonical_id: job.key-vault-manager
- type: queried_by
  target_canonical_id: service.token-data-service
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:55:43.350763Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Azure Key Vault

## Summary

Azure Key Vault is the central secure storage system for cryptographic key material used in token generation and JWT signing operations. It maintains RSA256 public-private key pairs throughout their lifecycle, supporting a 90-day key rotation schedule managed by independent background processes.

## Details

### Role in Key Management

Azure Key Vault serves as the primary repository for RSA256 public-private key pairs used for JWT token signing. Both the public and private PEM files are stored as secrets within the vault, enabling secure key storage and lifecycle management.

### Key Initialization and Caching

When the Token Data Service (TDS) initializes, it retrieves all available keys from Azure Key Vault and caches them in memory with their roles designated as primary or secondary based on key age. This approach minimizes vault queries during operation while maintaining secure key access patterns.

### Key Lifecycle Management

Each key stored in Azure Key Vault is associated with:
- A creation date used to calculate key age
- A 90-day expiration window

The Key Vault Manager (KVM) monitors key age and expiration status every 24 hours, generating new key pairs when needed and removing expired keys according to the rotation schedule.

### Key Rotation Schedule

The vault maintains keys following this lifecycle:
- Day 1: New key pair is introduced
- Days 3-83: Key serves as the primary signing key (80-day active period)
- Day 80: Subsequent new key pair is introduced and remains inactive for 3 days
- Days 84-90: Original key remains in vault in inactive state
- Day 91: Expired key is deleted from the vault

Keys remain live for a maximum of 90 days, and a new key is placed in the JWKS (JSON Web Key Set) for three days before becoming the primary signing key.

### Service Principal Management

The service principal password used to access Azure Key Vault rotates automatically every 12 months. Thirty days before expiration, an automated rotation occurs with email notification.

## Related Concepts

- [[job.key-vault-manager]] — The independent scheduled service responsible for generating, rotating, and deleting RSA key pairs in Azure Key Vault
- [[service.token-data-service]] — The service that retrieves keys from Azure Key Vault on startup and maintains an in-memory cache for JWT signing operations

## Sources

- Domino Key Rotation (Confluence page 96895304, Space: BD) — Primary documentation covering Azure Key Vault's role in storing cryptographic keys, the Key Vault Manager's rotation schedule, and the Token Data Service's key initialization and caching mechanisms
