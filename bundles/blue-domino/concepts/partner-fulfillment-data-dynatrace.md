---
type: Concept
title: Partner Fulfillment Data (Dynatrace)
description: This dashboard provides monitoring and observability for the partner-fulfillment-data
  service within the Dynatrace managed environment. It enables visualization of service
  health, performance metrics, and system behavior for the Partner Ful
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- dashboard
okf_schema: okf.concept.v1
identity:
  canonical_id: dashboard.partner-fulfillment-data-dynatrace
  concept_type: dashboard
  display_name: Partner Fulfillment Data (Dynatrace)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:3ed8011ffd20b8331a9ba06c0059be8439ad5155e6194b8039488d512400d635
  last_updated_at: '2026-09-02T21:49:05.292511Z'
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
  target_canonical_id: service.partner-fulfillment-data
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:49:05.292511Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Fulfillment Data (Dynatrace)

## Summary

This dashboard provides monitoring and observability for the partner-fulfillment-data service within the Dynatrace managed environment. It enables visualization of service health, performance metrics, and system behavior for the Partner Fulfillment data processing component.

## Details

The dashboard is hosted on the Dynatrace managed platform (tgo745 instance) and offers real-time monitoring capabilities for the partner-fulfillment-data service. The monitoring view includes configurable time-range filtering and service-specific metrics aggregation.

The dashboard supports:
- Service performance monitoring with configurable historical time windows
- Filtering capabilities for focused analysis on non-database service components
- Integration with the broader partner fulfillment platform monitoring suite alongside the orchestrator and domain services

Access to this monitoring resource requires appropriate permissions within the Dynatrace environment.

## Related Concepts

- [[external_system.tgo745-dynatrace-managed]] — The hosting platform for this dashboard
- [[service.partner-fulfillment-data]] — The service observed by this dashboard

## Sources

- Domino Dev Ops (Confluence, space: BD, page ID: 96895597) — Monitoring Links section documents the Dynatrace dashboard access for partner fulfillment services
