---
type: Concept
title: Production Situational Awareness (Datadog)
description: The Production Situational Awareness dashboard is a Datadog monitoring
  dashboard that provides real-time visibility into the production environment for
  partner fulfillment services. It enables teams to monitor service health, performance
  me
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- dashboard
okf_schema: okf.concept.v1
identity:
  canonical_id: dashboard.production-awareness-datadog
  concept_type: dashboard
  display_name: Production Situational Awareness (Datadog)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:252935e55b1cc80bb2c8d047cbf41faca0dbb6b68eeab4e0947a18f13388d7ec
  last_updated_at: '2026-09-02T21:49:51.880807Z'
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
  generated_at: '2026-09-02T21:49:51.880807Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Production Situational Awareness (Datadog)

## Summary

The Production Situational Awareness dashboard is a Datadog monitoring dashboard that provides real-time visibility into the production environment for partner fulfillment services. It enables teams to monitor service health, performance metrics, and operational status across the partner fulfillment platform.

## Details

### Purpose

This dashboard serves as the primary monitoring interface for production situational awareness, allowing teams to quickly assess the health and performance of production systems. It provides consolidated visibility into key metrics and service status indicators.

### Access Requirements

Access to the Production Situational Awareness dashboard requires appropriate Datadog permissions. The following service accounts are needed for visibility:

- Gateway Logs: jcs000-apim-datadog-reader-6668
- Service Logs: jcs000-gad6392datadogreader

Note: APT (Access Provisioning Tool) access may be required for full visibility into gateway and service logs.

### Monitored Services

The dashboard covers the following partner fulfillment services:

- Partner Fulfillment Orchestrator
- Partner Fulfillment Domain
- Partner Fulfillment Data
- Enterprise Customer services
- Daily Reconciliation processes

### Infrastructure Context

The services are deployed across Kubernetes clusters in the following environments:

- **Development**: aks-cx-shared-01-dev-centralus
- **Staging**: aks-cx-encust-01-stage-centralus
- **Production**: aks-cx-encust-01-prod-centralus

All services run in the `partner-fulfillments` namespace.

### Related Resources

This dashboard complements other monitoring tools in the environment, including Dynatrace monitoring for service performance tracking and operational observability.

## Related Concepts

- `external_system.datadog-monitoring`

## Sources

- Confluence: Domino Dev Ops (page 96895597, space BD) — Monitoring Links section
