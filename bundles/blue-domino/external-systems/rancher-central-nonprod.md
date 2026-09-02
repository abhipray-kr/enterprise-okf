---
type: External System
title: Rancher Central (Non-Prod)
description: Rancher Central (Non-Prod) is a Kubernetes cluster management system
  used in the non-production environment for deploying and managing containerized
  services. It provides centralized control and visibility for development and staging
  cluste
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- external_system
okf_schema: okf.concept.v1
identity:
  canonical_id: external_system.rancher-central-nonprod
  concept_type: external_system
  display_name: Rancher Central (Non-Prod)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:41d5115560318caf3c96d8af8413adc11eb7c0d419f32155232f8ada3c053972
  last_updated_at: '2026-09-02T21:56:17.077390Z'
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
- type: manages
  target_canonical_id: external_system.aks-dev-cluster
- type: manages
  target_canonical_id: external_system.aks-stage-cluster
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:56:17.077390Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Rancher Central (Non-Prod)

## Summary

Rancher Central (Non-Prod) is a Kubernetes cluster management system used in the non-production environment for deploying and managing containerized services. It provides centralized control and visibility for development and staging clusters through a web-based dashboard.

## Details

### Purpose
Rancher Central (Non-Prod) serves as the deployment management platform for non-production Kubernetes environments, enabling service deployment, configuration, and monitoring across development and staging infrastructure.

### Managed Clusters
The system manages the following Azure Kubernetes Service (AKS) clusters:
- Development Cluster: aks-cx-shared-01-dev-centralus
- Staging Cluster: aks-cx-encust-01-stage-centralus

### Configuration
- **Namespace**: partner-fulfillments
- **Dashboard URL**: https://rancher-central.nonprod.kpsazc.dgtl.kroger.com/dashboard/c/c-xf6cx/explorer/service
- **Region**: Central US

## Related Concepts

- [[external_system.aks-dev-cluster]] — Development Kubernetes cluster managed by this Rancher instance
- [[external_system.aks-stage-cluster]] — Staging Kubernetes cluster managed by this Rancher instance

## Sources

Domino Dev Ops (Confluence, page 96895597) — Lists Rancher Central (Non-Prod) in Service Links section as the deployment management system for non-production environments, with dashboard access URL and cluster configuration details.
