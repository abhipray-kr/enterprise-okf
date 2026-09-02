---
type: External System
title: AKS Prod Cluster (aks-cx-encust-01-prod-centralus)
description: The AKS Prod Cluster (`aks-cx-encust-01-prod-centralus`) is the production
  Kubernetes cluster hosted in Azure Kubernetes Service (AKS) in the Central US region.
  It serves the partner-fulfillments namespace and hosts critical partner fulfill
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- external_system
okf_schema: okf.concept.v1
identity:
  canonical_id: external_system.aks-prod-cluster
  concept_type: external_system
  display_name: AKS Prod Cluster (aks-cx-encust-01-prod-centralus)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:b600a23ca4caa45461a69846ead7edc15f8dc5afe7fdc7b1f0ba21625dc8fedf
  last_updated_at: '2026-09-02T21:55:11.378181Z'
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
- type: hosts
  target_canonical_id: service.partner-fulfillment-data
- type: hosts
  target_canonical_id: service.partner-fulfillment-domain
- type: hosts
  target_canonical_id: service.partner-fulfillment-orchestrator
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:55:11.378181Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# AKS Prod Cluster (aks-cx-encust-01-prod-centralus)

## Summary

The AKS Prod Cluster (`aks-cx-encust-01-prod-centralus`) is the production Kubernetes cluster hosted in Azure Kubernetes Service (AKS) in the Central US region. It serves the partner-fulfillments namespace and hosts critical partner fulfillment services for Kroger's customer platform.

## Details

### Cluster Information

**Cluster Name:** aks-cx-encust-01-prod-centralus  
**Environment:** Production  
**Region:** Central US  
**Namespace:** partner-fulfillments

### Hosted Services

This cluster hosts the following services:

- `service.partner-fulfillment-data`
- `service.partner-fulfillment-domain`
- `service.partner-fulfillment-orchestrator`

### Monitoring and Observability

**Dynatrace Monitoring:**
- Comprehensive APM monitoring available for all hosted services
- Service dashboards available for performance tracking and analysis

**Datadog Monitoring:**
- Gateway logs accessible via `jcs000-apim-datadog-reader-6668` service account
- Service logs accessible via `jcs000-gad6392datadogreader` service account
- Enterprise Customer index (Domain 6392) available for filtering partner services
- Production Situational Awareness Dashboard available
- Signup Dashboard available

### Access and Management

**Rancher Management:**
- Production cluster accessible via Rancher dashboard at https://rancher-central.prod.kpsazc.dgtl.kroger.com/dashboard/c/c-tb2rw/explorer/service

**Gateway Routes:**
- Orchestrator: `/ui/partner-benefit/v1/eligibilities`, `/ui/partner-benefit/v1/fulfillments`, `/partner-benefit/v1/partner`
- Domain: `/partner-fulfillment/v1`, `/partner-fulfillment/v1/.well-known/jwks.json`

## Related Concepts

- `service.partner-fulfillment-data`
- `service.partner-fulfillment-domain`
- `service.partner-fulfillment-orchestrator`

## Sources

- Page 96895597 (Domino Dev Ops): K8 - Cluster Names, Service Links, Monitoring Links
