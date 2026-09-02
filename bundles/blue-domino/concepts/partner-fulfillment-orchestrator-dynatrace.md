---
type: Concept
title: Partner Fulfillment Orchestrator (Dynatrace)
description: A Dynatrace-based monitoring dashboard that provides observability and
  performance metrics for the `service.partner-fulfillment-orchestrator` service.
  The dashboard is hosted on the TGO745 Dynatrace managed instance and displays service
  hea
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- dashboard
okf_schema: okf.concept.v1
identity:
  canonical_id: dashboard.partner-fulfillment-orchestrator-dynatrace
  concept_type: dashboard
  display_name: Partner Fulfillment Orchestrator (Dynatrace)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:c9431a48671ac2e1f013e31a5e37f6bc056a0d71dfdcce25803f4a7f721eca63
  last_updated_at: '2026-09-02T21:49:36.073607Z'
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
  target_canonical_id: service.partner-fulfillment-orchestrator
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:49:36.073607Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Fulfillment Orchestrator (Dynatrace)

## Summary

A Dynatrace-based monitoring dashboard that provides observability and performance metrics for the `service.partner-fulfillment-orchestrator` service. The dashboard is hosted on the TGO745 Dynatrace managed instance and displays service health, transaction flows, and key performance indicators with a default 6-hour timeframe.

## Details

The Partner Fulfillment Orchestrator dashboard is integrated into the Dynatrace monitoring platform to track the operational health and behavior of the orchestrator service that manages partner fulfillment workflows.

### Access and Hosting

The dashboard is hosted on `external_system.tgo745-dynatrace-managed` and accessible through the Dynatrace UI. The dashboard filters service data to show metrics specific to the partner-fulfillment-orchestrator component, with configurable time windows (default -6h).

### Related Services and Dashboards

The monitoring infrastructure includes complementary dashboards for related components within the Partner Fulfillment system:
- Partner Fulfillment Domain service dashboard
- Partner Fulfillment Data service dashboard  
- Daily Reconciliation dashboard

### Infrastructure Context

The service is deployed in Kubernetes clusters across multiple environments:
- **Namespace**: partner-fulfillments
- **Dev**: aks-cx-shared-01-dev-centralus
- **Stage**: aks-cx-encust-01-stage-centralus
- **Production**: aks-cx-encust-01-prod-centralus

## Related Concepts

- `service.partner-fulfillment-orchestrator`
- `external_system.tgo745-dynatrace-managed`

## Sources

- Domino Dev Ops (Confluence page 96895597): Monitoring Links section containing Dynatrace dashboard references for the Partner Fulfillment Orchestrator service
