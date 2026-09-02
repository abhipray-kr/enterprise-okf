---
type: Deployment Process
title: Domino JWK Token Rotation Deployment
description: 'This deployment process enables automated JWK token rotation for the
  Domino service with zero downtime. It implements a two-phase approach: initial deployment
  with a shortened key lifecycle to allow Disney to cache new keys, followed by rev'
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895586/JWK+Token+Rotation+Plan+for+Domino+-+Production+Readiness
tags:
- okf
- deployment_process
okf_schema: okf.concept.v1
identity:
  canonical_id: deployment_process.process-domino-jwk-token-rotation
  concept_type: deployment_process
  display_name: Domino JWK Token Rotation Deployment
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:3d278f38d9d1594f4cdc607c368855ee3cb9e24e879caaf34d479665c5bb200a
  last_updated_at: '2026-09-02T21:51:50.552789Z'
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
- type: deploys_service
  target_canonical_id: service.domino
- type: implements_control
  target_canonical_id: security_control.control-domino-jwk-token-rotation-config
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:51:50.552789Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Domino JWK Token Rotation Deployment

## Summary

This deployment process enables automated JWK token rotation for the Domino service with zero downtime. It implements a two-phase approach: initial deployment with a shortened key lifecycle to allow Disney to cache new keys, followed by reversion to standard 90-day rotation intervals. The process migrates the Token Data Service from using static JWT/JWK files to dynamically fetching keys from Azure Key Vault, maintaining alignment with Disney's 90-day token rotation requirements.

## Details

### Overview

Disney rotates their JWK tokens every 90 days in production and requires the same rotation cadence from integrating services. Domino was initially deployed using a static JWT and JWK set. To meet ongoing security requirements, the following capabilities have been developed:

- Token Manager for automating token rotation
- Token uploads to Azure Key Vault based on defined schedules
- Token Data Service updated to fetch tokens from Azure secrets instead of static files

### Deployment Approach

The deployment is structured to ensure zero downtime by leveraging already-active production keys:

1. **Copy existing credentials**: Upload the existing JWT and JWK set to production Azure Key Vault
2. **Enable primary state**: Allow the uploaded JWT and JWK set to transition from Inactive/Secondary to Active/Primary state
3. **Deploy Token Data Service**: Deploy both the Token Manager and Token Data Service to production with shortened key lifecycle configurations
4. **Gradual key rotation**: After deployment, the Token Data Service begins retrieving keys from Azure Key Vault instead of static files. Since the uploaded keys match the currently-used static production keys, this transition causes no downtime
5. **Disney cache period**: The shortened initial key lifecycle provides Disney sufficient time to cache new keys before they are promoted to Primary state
6. **Configuration revert**: After the initial rotation cycle completes and Disney has cached the new key set, a second production deployment reverts configurations back to standard 90-day intervals

### Key Lifecycle Configuration

#### Initial Deployment Configuration (Short Cycle)

For the first production deployment, a shortened key lifecycle allows controlled transition:

- **INACTIVE_PERIOD**: 4 hours 30 minutes (duration new keys remain inactive after creation)
- **NEW_KEY_CREATION**: 5 days (interval for introducing new keys)
- **INACTIVE_KEY_STATE**: 10 hours 30 minutes (duration expired keys stay inactive before deletion)
- **PRIMARY_KEY_STATE**: 5 days 4 hours 30 minutes (time keys stay as primary)
- **KEY_LIFETIME**: 5 days 15 hours (total lifetime of a key)

#### Production Harness Configuration (Standard Cycle)

After the initial rotation cycle and Disney cache update, revert to standard configuration:

- **INACTIVE_PERIOD**: 3 days
- **NEW_KEY_CREATION**: 80 days
- **INACTIVE_KEY_STATE**: 7 days
- **PRIMARY_KEY_STATE**: 83 days
- **KEY_LIFETIME**: 90 days

### Deployment Timeline Example

Using the initial shortened (5-day, 15-hour) cycle:

| Date/Time | Action | Key Status |
|-----------|--------|-----------:|
| Wed, Jun 18, 4:00 AM | Manually upload prod key to Key Vault | Old prod key, age 0, only key in vault (inactive until 4:30 AM) |
| Wed, Jun 18, 8:30 AM | 4h 30m later: prod key eligible for primary | Prod key now eligible for primary (age: 4h 30m) |
| Thu, Jun 19, 4:00 AM | **Deployment 1**: Deploy both apps with shorter configs | Prod key is primary, age = 24h; smooth cutover, no downtime. Stays primary until Mon, Jun 23, 8:30 AM |
| Mon, Jun 23, 4:00 AM | KVM creates new key after 5d of prod key | Age of primary prod key is now 5d. New secondary key age 0 days (inactive for next 4h 30m) |
| Mon, Jun 23, 8:30 AM | New key completes 4h 30m inactive period | Original prod key becomes secondary/inactive. New key becomes primary (will stay until Sat, Jun 28, 8:30 AM) |
| Tue, Jun 24, 7:00 PM | Old prod key (secondary) is deleted | Only new key remains in vault |
| Thu, Jun 26, 5:00 AM onwards | **Deployment 2**: Revert configs back to default values | New key is now 3d 1h after creation; eligible for primary under default configs |

### Key Timing Considerations

- Key ages are calculated from the day and time of creation in Azure Key Vault
- Cron job execution timing in KVM and TDS must account for the configured lifecycle intervals
- The inactive period for new keys must complete before a key becomes eligible for primary state
- When configurations are reverted mid-cycle, only keys meeting the new configuration criteria become eligible for state transitions
- A single key remaining in the vault after secondary key deletion will serve as the sole primary until the next rotation cycle

### Critical Success Factors

- The uploaded keys must be identical to existing production keys to ensure zero-downtime transition
- Timing of deployments must align with key eligibility states to avoid disruption
- Disney's cache refresh must occur during the shortened lifecycle to ensure new keys are cached before long-term rotation begins
- Configuration revert deployment must occur when existing keys remain eligible for primary state under new configuration parameters

## Related Concepts

- **service.domino**: The service being updated with automated token rotation capability
- **security_control.control-domino-jwk-token-rotation-config**: The security control implemented by this deployment process

## Sources

**Primary Source**: Confluence page "🔐 JWK Token Rotation Plan for Domino - Production Readiness" (Page ID: 96895586, Space: BD)
