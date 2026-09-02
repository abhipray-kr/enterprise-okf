---
type: Architecture
title: Partner Fulfillment Orchestrator Regional Deployment Architecture
description: The Partner Fulfillment Orchestrator implements a two-region active-passive
  deployment architecture across US Central and US East 2 Azure regions. The architecture
  provides resilience and disaster recovery capability for the Kroger Company'
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895308/partner-fulfillment-orchestrator
tags:
- okf
- architecture
okf_schema: okf.concept.v1
identity:
  canonical_id: architecture.pfo-regional-deployment
  concept_type: architecture
  display_name: Partner Fulfillment Orchestrator Regional Deployment Architecture
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:84cd8600b3d3de284374333134e62dd3f382c3ab9eb34aadeef28fdefed7df4e
  last_updated_at: '2026-09-02T21:48:10.235999Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895308'
    page_title: partner-fulfillment-orchestrator
    page_version: 26
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895308/partner-fulfillment-orchestrator
    role: primary
relationships:
- type: describes_infrastructure_for
  target_canonical_id: disaster_recovery_plan.plan-pfo-regional-failover
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:48:10.235999Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Fulfillment Orchestrator Regional Deployment Architecture

## Summary

The Partner Fulfillment Orchestrator implements a two-region active-passive deployment architecture across US Central and US East 2 Azure regions. The architecture provides resilience and disaster recovery capability for the Kroger Company's fulfillment operations, with manual failover procedures and a documented recovery strategy. US Central serves as the primary region while US East 2 acts as a secondary region for failover scenarios.

## Details

### Deployment Configuration

The service.partner-fulfillment-orchestrator is deployed across two Azure regions with identical functionality in both:

- **Primary Region**: US Central (Default traffic destination)
- **Secondary Region**: US East 2 (Failover destination)
- **APM Number**: APM0004876

Both regions maintain full functionality, though traffic is routed by default to the primary region through the API Gateway.

### Failover Mechanism

Failover is initiated through manual API Gateway URL reconfiguration when the primary US Central region becomes unavailable. The failover process redirects traffic to US East 2, which maintains complete application functionality.

### Pre-Deployment Requirements

Before activating the secondary region, the following checks must be completed:

- Pod configuration and stability verification across both regions via Rancher
- Dependent service validation including Loyalty service, Profile-alias service, Offers service, and internal services (partner-fulfillment-data and partner-fulfillment-domain)
- Container image tag verification from GitHub latest release
- API Gateway configuration coordination with the API Program team

### Recovery Objectives

- **Recovery Time Objective (RTO)**: 4 hours for critical applications
- **Recovery Point Objective (RPO)**: 1 hour for critical applications

### Recovery Process

When US Central is unavailable:

1. Configure API Gateway URL to redirect to east region
2. Verify pod health on Rancher East under partner-fulfillments namespace
3. Monitor metrics in Grafana
4. Review Datadog logs for service errors
5. Execute smoke testing in production environment

### Testing Schedule

The architecture undergoes failover testing once every 6 months.

### Recovery Team Structure

Recovery operations are coordinated by a dedicated team with the following responsibilities:

- **IT Application Owner**: Sunil P Bapat
- **Business Owner**: Heather Alvey
- **Support/Escalation Team**: Venketa S Penmetsa, Santosh Boppidi, Goutam Kundu

### Communication

- Team Distribution: EnterpriseCustomer-BlueArmy@kroger.com
- Infrastructure Group: APP-DIG-SVC-Partner-Fulfillment
- Management Escalation: Sunil P Bapat, Nathan Subler

## Related Concepts

- **service.partner-fulfillment-orchestrator** — The fulfillment orchestration service deployed using this architecture
- **disaster_recovery_plan.plan-pfo-regional-failover** — The detailed disaster recovery plan that this architecture supports

## Sources

Page ID: 96895308 | Space: BD | Title: partner-fulfillment-orchestrator | Version: 26
