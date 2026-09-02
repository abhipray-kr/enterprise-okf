---
type: External System
title: AKS Dev Cluster (aks-cx-shared-01-dev-centralus)
description: The AKS Dev Cluster (aks-cx-shared-01-dev-centralus) is an Azure Kubernetes
  Service cluster deployed in the Central US region for development environments.
  It hosts partner fulfillment services within the partner-fulfillments Kubernetes
  nam
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- external_system
okf_schema: okf.concept.v1
identity:
  canonical_id: external_system.aks-dev-cluster
  concept_type: external_system
  display_name: AKS Dev Cluster (aks-cx-shared-01-dev-centralus)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:72a393ac948e57f326cd2f4a090e219fe64ab2e0aa3fc76859fe9c1db13048be
  last_updated_at: '2026-09-02T21:54:49.147349Z'
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
  generated_at: '2026-09-02T21:54:49.147349Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# AKS Dev Cluster (aks-cx-shared-01-dev-centralus)

## Summary

The AKS Dev Cluster (aks-cx-shared-01-dev-centralus) is an Azure Kubernetes Service cluster deployed in the Central US region for development environments. It hosts partner fulfillment services within the partner-fulfillments Kubernetes namespace and is part of a multi-environment Kubernetes infrastructure.

## Details

### Environment Configuration
- **Cluster Name**: aks-cx-shared-01-dev-centralus
- **Region**: Central US
- **Environment Tier**: Development
- **Kubernetes Namespace**: partner-fulfillments

### Multi-Environment Infrastructure
This cluster is part of a coordinated multi-environment deployment:
- **Development**: aks-cx-shared-01-dev-centralus
- **Staging**: aks-cx-encust-01-stage-centralus
- **Production**: aks-cx-encust-01-prod-centralus

### Hosted Services
The cluster hosts the following services within the partner-fulfillments namespace:
- service.partner-fulfillment-orchestrator
- service.partner-fulfillment-domain
- service.partner-fulfillment-data

### Observability and Monitoring
- **Dynatrace**: Service monitoring and performance analytics for all hosted services
- **Datadog**: Logs and metrics (Gateway Logs and Service Logs available with appropriate APM credentials)

### Cluster Management
- **Rancher (Non-PROD)**: Primary cluster management and service exploration interface
- **GitHub**: Code repositories for hosted services
- **ServiceNow**: Ticket tracking (APM0004876)

### API Gateway Routes
- **Orchestrator**: `/ui/partner-benefit/v1/eligibilities`, `/ui/partner-benefit/v1/fulfillments`, `/partner-benefit/v1/partner`
- **Domain**: `/partner-fulfillment/v1`, `/partner-fulfillment/v1/.well-known/jwks.json`

## Related Concepts

- service.partner-fulfillment-orchestrator
- service.partner-fulfillment-domain
- service.partner-fulfillment-data

## Sources

- Confluence: Domino Dev Ops (Space: BD, Page ID: 96895597)
