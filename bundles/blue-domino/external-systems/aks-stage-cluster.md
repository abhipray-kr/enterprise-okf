---
type: External System
title: AKS Stage Cluster (aks-cx-encust-01-stage-centralus)
description: The AKS Stage Cluster (aks-cx-encust-01-stage-centralus) is an Azure
  Kubernetes Service cluster in the centralus region that hosts the partner-fulfillments
  namespace for staging environments. It serves as the primary deployment target for
  t
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- external_system
okf_schema: okf.concept.v1
identity:
  canonical_id: external_system.aks-stage-cluster
  concept_type: external_system
  display_name: AKS Stage Cluster (aks-cx-encust-01-stage-centralus)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:d060eb9e7bfa89486ddf7a23c67e02d2de3300b526b93b5f00c973b2b621ee02
  last_updated_at: '2026-09-02T21:55:27.974524Z'
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
  generated_at: '2026-09-02T21:55:27.974524Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# AKS Stage Cluster (aks-cx-encust-01-stage-centralus)

## Summary

The AKS Stage Cluster (aks-cx-encust-01-stage-centralus) is an Azure Kubernetes Service cluster in the centralus region that hosts the partner-fulfillments namespace for staging environments. It serves as the primary deployment target for the partner fulfillment service ecosystem in non-production.

## Details

### Cluster Information

- **Full Name**: aks-cx-encust-01-stage-centralus
- **Namespace**: partner-fulfillments
- **Environment**: Stage
- **Region**: centralus
- **Platform**: Azure Kubernetes Service (AKS)

### Hosted Services

This cluster hosts the following services:

- [service.partner-fulfillment-orchestrator](service.partner-fulfillment-orchestrator)
- [service.partner-fulfillment-domain](service.partner-fulfillment-domain)
- [service.partner-fulfillment-data](service.partner-fulfillment-data)

### Monitoring and Observability

**Dynatrace**: Integrated monitoring available for hosted services including partner-fulfillment-orchestrator, partner-fulfillment-domain, and partner-fulfillment-data.

**Datadog**: Service logs available with appropriate access permissions (APT required for visibility to gateway logs and service logs).

### Access and Management

**Rancher (Non-PROD)**: Available at https://rancher-central.nonprod.kpsazc.dgtl.kroger.com/dashboard/c/c-xf6cx/explorer/service for cluster and service management.

**GitHub Repositories**:
- https://github.com/krogertechnology/partner-fulfillment-api
- https://github.com/krogertechnology/partner-fulfillment-orchestrator

### Service Routes

The cluster hosts services accessible through the following gateway routes:
- Orchestrator: `/ui/partner-benefit/v1/eligibilities`, `/ui/partner-benefit/v1/fulfillments`, `/partner-benefit/v1/partner`
- Domain: `/partner-fulfillment/v1`, `/partner-fulfillment/v1/.well-known/jwks.json`

## Related Concepts

- [service.partner-fulfillment-orchestrator](service.partner-fulfillment-orchestrator)
- [service.partner-fulfillment-domain](service.partner-fulfillment-domain)
- [service.partner-fulfillment-data](service.partner-fulfillment-data)

## Sources

- Domino Dev Ops (Confluence, page 96895597, BD space): K8 cluster names, monitoring links, service links, and cluster configuration details
