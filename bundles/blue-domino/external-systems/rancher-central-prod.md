---
type: External System
title: Rancher Central (Prod)
description: Rancher Central (Prod) is the production Kubernetes cluster management
  and orchestration platform used for deploying and managing containerized services.
  It provides a centralized dashboard for monitoring, deployment management, and service
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- external_system
okf_schema: okf.concept.v1
identity:
  canonical_id: external_system.rancher-central-prod
  concept_type: external_system
  display_name: Rancher Central (Prod)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:7950b21e69dc2d2d057c34d63d4548b7f4203ea1c491bb71539c346d2e8ee9da
  last_updated_at: '2026-09-02T21:56:37.702365Z'
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
  target_canonical_id: external_system.aks-prod-cluster
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:56:37.702365Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Rancher Central (Prod)

## Summary

Rancher Central (Prod) is the production Kubernetes cluster management and orchestration platform used for deploying and managing containerized services. It provides a centralized dashboard for monitoring, deployment management, and service exploration across production Kubernetes environments.

## Details

### Purpose

Rancher Central (Prod) serves as the deployment management system for production workloads in the Kroger infrastructure. It provides visibility and control over services running in production Kubernetes clusters.

### Access

The production Rancher instance is accessible at:
- Dashboard: https://rancher-central.prod.kpsazc.dgtl.kroger.com/dashboard/c/c-tb2rw/explorer/service

### Kubernetes Integration

The system manages production Kubernetes clusters including:
- aks-cx-encust-01-prod-centralus (Azure Kubernetes Service cluster for production workloads)

Services are deployed with namespace `partner-fulfillments` for relevant applications.

## Related Concepts

- `external_system.aks-prod-cluster` — The production Azure Kubernetes Service cluster managed by Rancher Central (Prod)

## Sources

- **Domino Dev Ops** — Confluence page (96895597, BD space) detailing service infrastructure links and Kubernetes cluster configurations including Rancher deployment management systems
