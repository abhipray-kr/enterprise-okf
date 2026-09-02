---
type: Concept
title: Daily Reconciliation (Dynatrace)
description: A Dynatrace monitoring dashboard hosted on the TGO745 Dynatrace-managed
  instance that provides observability and performance metrics for the Daily Reconciliation
  job. The dashboard displays service overview data with a default six-hour time
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
tags:
- okf
- dashboard
okf_schema: okf.concept.v1
identity:
  canonical_id: dashboard.daily-reconciliation-dynatrace
  concept_type: dashboard
  display_name: Daily Reconciliation (Dynatrace)
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:5a8179232919c69a90e0901aa0abfa065a04dcf69d0f482e627915a0923a4d58
  last_updated_at: '2026-09-02T21:48:48.988264Z'
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
  target_canonical_id: job.daily-reconciliation
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:48:48.988264Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Daily Reconciliation (Dynatrace)

## Summary

A Dynatrace monitoring dashboard hosted on the TGO745 Dynatrace-managed instance that provides observability and performance metrics for the Daily Reconciliation job. The dashboard displays service overview data with a default six-hour time window and enables monitoring of job execution health and system behavior.

## Details

The Daily Reconciliation dashboard is accessible via the Dynatrace service overview interface (Service ID: SERVICE-7E49778AD9EF9AD0) on the organization's Dynatrace-managed environment. The dashboard captures real-time and historical performance data for the daily reconciliation process, supporting troubleshooting, performance analysis, and operational visibility into the job's execution patterns.

The dashboard integrates with the broader monitoring landscape documented in Domino Dev Ops, where it is referenced alongside other monitoring tools and dashboards for the partner fulfillment domain. The six-hour default time filter (gtf=-6h parameter) is configured to provide sufficient historical context for reconciliation job analysis while maintaining dashboard responsiveness.

## Related Concepts

- **external_system.tgo745-dynatrace-managed** — The Dynatrace-managed instance hosting this dashboard
- **job.daily-reconciliation** — The job being observed and monitored by this dashboard

## Sources

- Domino Dev Ops (Confluence page 96895597) — Monitoring Links section
