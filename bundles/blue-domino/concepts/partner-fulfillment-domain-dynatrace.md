---
type: Concept
title: Partner Fulfillment Domain (Dynatrace)
description: This Dynatrace dashboard provides monitoring and observability for the
  Partner Fulfillment Domain service. It is hosted on the tgo745 Dynatrace-managed
  instance and tracks performance metrics, service health, and operational data for
  the pa
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- dashboard
okf_schema: okf.concept.v1
identity:
  canonical_id: dashboard.partner-fulfillment-domain-dynatrace
  concept_type: dashboard
  display_name: Partner Fulfillment Domain (Dynatrace)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:275d9a0518c1d083bf6d1da701560a16111a33c262b42f159d049656a0681be6
  last_updated_at: '2026-09-02T21:49:21.393684Z'
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
  target_canonical_id: external_system.tgo745-dynatrace-managed
- type: observes
  target_canonical_id: service.partner-fulfillment-domain
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:49:21.393684Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Fulfillment Domain (Dynatrace)

## Summary

This Dynatrace dashboard provides monitoring and observability for the Partner Fulfillment Domain service. It is hosted on the tgo745 Dynatrace-managed instance and tracks performance metrics, service health, and operational data for the partner-fulfillment-domain service component.

## Details

### Access

The dashboard is available at the following Dynatrace instance:
- **Instance**: tgo745.dynatrace-managed.com
- **Environment ID**: 7571065c-f052-471e-a3d7-f99d529548bb

### Service Monitoring

This dashboard observes the `partner-fulfillment-domain` service with monitoring parameters:
- Default time window: last 6 hours (gtf=-6h)
- Service filter: NON_DATABASE_SERVICE-NAME=partner-fulfillment-domain
- Sorting: by name in ascending order

### Purpose

The dashboard provides visibility into:
- Service performance and health metrics
- Request processing for the Partner Fulfillment domain component
- Key operational endpoints including:
  - `/partner-fulfillment/v1` (primary domain endpoint)
  - `/partner-fulfillment/v1/.well-known/jwks.json` (key management)

### Infrastructure Context

- **Kubernetes Namespace**: partner-fulfillments
- **Production Cluster**: aks-cx-encust-01-prod-centralus
- **Stage Cluster**: aks-cx-encust-01-stage-centralus
- **Development Cluster**: aks-cx-shared-01-dev-centralus

## Related Concepts

- external_system.tgo745-dynatrace-managed
- service.partner-fulfillment-domain

## Sources

- Domino Dev Ops (Confluence page ID: 96895597, space key: BD)
