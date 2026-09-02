---
type: Service
title: Domino
description: Domino is a service that manages JWT and JWK token authentication in
  integration with external partners, specifically Disney. The service requires implementation
  of automated JWK token rotation on a 90-day cycle to align with partner securi
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895586/JWK+Token+Rotation+Plan+for+Domino+-+Production+Readiness
tags:
- okf
- service
okf_schema: okf.concept.v1
identity:
  canonical_id: service.domino
  concept_type: service
  display_name: Domino
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:42d9c932c613136d09e24db289a7da2aa896fedb877d09936660cb7489a643a1
  last_updated_at: '2026-09-02T22:00:32.251576Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895586'
    page_title: 🔐 JWK Token Rotation Plan for Domino - Production Readiness
    page_version: 16
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895586/JWK+Token+Rotation+Plan+for+Domino+-+Production+Readiness
    role: primary
relationships:
- type: deployed_via
  target_canonical_id: deployment_process.process-domino-jwk-token-rotation
- type: uses_security_control
  target_canonical_id: security_control.control-domino-jwk-token-rotation-config
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T22:00:32.251576Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Domino

## Summary

Domino is a service that manages JWT and JWK token authentication in integration with external partners, specifically Disney. The service requires implementation of automated JWK token rotation on a 90-day cycle to align with partner security requirements and production standards.

## Details

### Service Purpose
Domino handles token-based authentication through JWT and JWK token sets. It serves as an integration point with Disney and is expected to comply with their security requirements regarding token lifecycle management.

### Current Architecture
Domino's token management system consists of:
- Token Manager: Automates token rotation
- Token Data Service: Fetches tokens from Azure Key Vault instead of static files
- Azure Key Vault: Centralized storage for JWT and JWK sets

### Token Rotation Requirements
Disney rotates JWK tokens every 90 days in production. Domino's production deployment must support this rotation cycle with the following key lifecycle parameters:

- **INACTIVE_PERIOD**: 3 days (duration new keys remain inactive after creation)
- **NEW_KEY_CREATION**: 80 days (interval for introducing new keys)
- **INACTIVE_KEY_STATE**: 7 days (duration expired keys stay inactive before deletion)
- **PRIMARY_KEY_STATE**: 83 days (time keys stay as primary)
- **KEY_LIFETIME**: 90 days (total lifetime of a key)

### Deployment Approach
The deployment plan ensures zero-downtime token transition:
1. Copy existing JWT and JWK set to production Azure Key Vault
2. Allow uploaded tokens to transition from Inactive/Secondary to Active/Primary state
3. Deploy Token Data Service to retrieve keys from Azure Key Vault
4. Since uploaded keys match currently-used static production keys, no service interruption occurs
5. Token rotation begins on schedule, allowing downstream consumers (Disney) time to cache new keys before promotion to Primary state

### Production Deployment Phasing
Two-phase deployment uses temporarily shortened key lifecycle configurations:
- **Phase 1**: Deploy with shortened lifecycle (1-2 weeks) to verify token transitions
- **Phase 2**: Revert to standard 90-day lifecycle after Disney caches new key sets

## Related Concepts

- deployment_process.process-domino-jwk-token-rotation
- security_control.control-domino-jwk-token-rotation-config

## Sources

**Confluence Page:** 🔐 JWK Token Rotation Plan for Domino - Production Readiness (Page ID: 96895586, Space: BD)
- Overview and business requirements
- Deployment plan and timeline
- Key lifecycle configuration specifications
- Q&A on configuration strategy
- Detailed deployment timeline with key state transitions
