---
type: Security Control
title: Domino JWK Token Rotation Configuration
description: This security control defines the configuration parameters and deployment
  strategy for automated JWK token rotation in the Domino service. The configuration
  implements a 90-day token lifecycle to align with Disney's token rotation requireme
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895586/JWK+Token+Rotation+Plan+for+Domino+-+Production+Readiness
tags:
- okf
- security_control
okf_schema: okf.concept.v1
identity:
  canonical_id: security_control.control-domino-jwk-token-rotation-config
  concept_type: security_control
  display_name: Domino JWK Token Rotation Configuration
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:c6e9ced473611fe889aadbe4960f5ade78bfaf905a4a970788c7b185d7e9d22f
  last_updated_at: '2026-09-02T21:59:54.738910Z'
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
- type: controls
  target_canonical_id: service.domino
- type: implemented_by
  target_canonical_id: deployment_process.process-domino-jwk-token-rotation
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:59:54.738910Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Domino JWK Token Rotation Configuration

## Summary

This security control defines the configuration parameters and deployment strategy for automated JWK token rotation in the Domino service. The configuration implements a 90-day token lifecycle to align with Disney's token rotation requirements. It includes settings for key inactivity periods, creation intervals, and promotion schedules that govern how JSON Web Keys are generated, activated, and retired in production.

## Details

### Overview

Disney requires JWK tokens to be rotated every 90 days in production. Domino previously used a static JWT and JWK set during initial deployment. To meet this requirement, a Token Manager was developed to automate token rotation, with token uploads to Azure Key Vault based on a defined schedule and the Token Data Service updated to fetch tokens from Azure secrets instead of static files.

### Production Configuration Parameters

The standard production configuration uses a 90-day key lifecycle with the following parameters:

| Parameter | Value | Purpose |
|-----------|-------|---------| 
| INACTIVE_PERIOD | 3 days | Duration new keys remain inactive after creation |
| NEW_KEY_CREATION | 80 days | Interval for introducing new keys |
| INACTIVE_KEY_STATE | 7 days | Duration expired keys stay inactive before deletion |
| PRIMARY_KEY_STATE | 83 days | Time keys stay as primary |
| KEY_LIFETIME | 90 days | Total lifetime of a key |
| NEW_KEY_STATE | 3 days | Inactivity period for new keys (same as INACTIVE_PERIOD) |

### Deployment Strategy

The production deployment follows these steps to ensure zero downtime:

1. Copy existing JWT and JWK set and upload to Azure Key Vault
2. Allow uploaded JWT and JWK set to transition to Active/Primary state from Inactive/Secondary
3. Deploy Token Data Service to production once keys are in Active/Primary state
4. Service begins retrieving keys from Azure Key Vault instead of static set
5. Token rotation starts based on the configured schedule

Since the uploaded keys are identical to currently used static production keys, this deployment causes no downtime.

### Key Lifecycle Mechanics

Key ages are calculated from the day and time of creation. When a new key is created, it remains in an inactive state for the configured INACTIVE_PERIOD before becoming eligible for primary use. As keys age and reach their PRIMARY_KEY_STATE duration, they transition to secondary status and become inactive. After reaching INACTIVE_KEY_STATE duration, secondary keys are deleted from the vault.

### Initial Production Configuration Adjustment

For the initial production deployment, the key lifecycle may be temporarily shortened (e.g., 1–2 weeks) since the Token Manager uses the existing production key set. This shorter lifecycle allows for a rapid transition period while Disney caches the new keys. Once caching is complete, configuration parameters revert to standard 90-day lifecycle settings.

### Cron Job Execution

The key rotation process is managed by scheduled cron jobs in KVM and TDS environments. The cron schedule must align with the configured NEW_KEY_CREATION interval to ensure new keys are generated at the appropriate times throughout the key lifecycle.

## Related Concepts

- [service.domino](service.domino) — The service controlled by this security configuration
- [deployment_process.process-domino-jwk-token-rotation](deployment_process.process-domino-jwk-token-rotation) — The deployment process that implements this configuration

## Sources

- [🔐 JWK Token Rotation Plan for Domino - Production Readiness](https://kroger.atlassian.net/wiki/spaces/BD/pages/96895586/JWK+Token+Rotation+Plan+for+Domino+-+Production+Readiness) — Primary source page containing production configuration parameters, deployment strategy, and key lifecycle timeline details
