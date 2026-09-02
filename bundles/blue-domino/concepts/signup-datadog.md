---
type: Concept
title: Signup (Datadog)
description: The Signup Datadog Dashboard is a monitoring dashboard hosted on Datadog
  that provides real-time visibility into the signup services within the Partner-Benefit-Fulfillment
  platform.
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- dashboard
okf_schema: okf.concept.v1
identity:
  canonical_id: dashboard.signup-datadog
  concept_type: dashboard
  display_name: Signup (Datadog)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:d8db5b6a6202b6bb27bf91071228d1360407c23371a4de86c383526ce58a3989
  last_updated_at: '2026-09-02T21:50:07.360418Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895597'
    page_title: Domino Dev Ops
    page_version: 1
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
    role: primary
relationships:
- type: hosted_on
  target_canonical_id: external_system.datadog-monitoring
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:50:07.360418Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Signup (Datadog)

## Summary

The Signup Datadog Dashboard is a monitoring dashboard hosted on Datadog that provides real-time visibility into the signup services within the Partner-Benefit-Fulfillment platform.

## Details

### Purpose
The dashboard is used for monitoring and situational awareness of signup service operations, particularly for the Enterprise Customer services (SPM ID: 6392).

### Access Requirements
Access to the Signup Dashboard requires Datadog reader credentials:
- Service logs reader: `jcs000-gad6392datadogreader`

These credentials provide visibility into service logs for the Enterprise Customer services.

### Deployment Environment
The services monitored by this dashboard are deployed across three Kubernetes clusters in the `partner-fulfillments` namespace:
- **Development**: aks-cx-shared-01-dev-centralus
- **Stage**: aks-cx-encust-01-stage-centralus
- **Production**: aks-cx-encust-01-prod-centralus

### Related Services Monitored
The dashboard provides monitoring for:
- Enterprise Customer services (SPM ID: 6392)
- Partner-Benefit-Fulfillment-API services (SPM ID: 11007)

## Related Concepts

- **external_system.datadog-monitoring** – The Datadog platform where this dashboard is hosted and maintained

## Sources

- Domino Dev Ops (page 96895597): Monitoring Links section – lists the Signup Datadog Dashboard as a quick link for monitoring signup services
