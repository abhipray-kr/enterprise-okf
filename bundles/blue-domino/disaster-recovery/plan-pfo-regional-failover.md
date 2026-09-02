---
type: Disaster Recovery Plan
title: Partner Fulfillment Orchestrator Regional Failover Recovery Plan
description: This Cloud Disaster Recovery Plan provides a structured approach for
  recovering data and applications hosted in cloud environments after a disruption,
  ensuring business continuity for the Kroger Company. The plan specifically covers
  `servic
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895308/partner-fulfillment-orchestrator
tags:
- okf
- disaster_recovery_plan
okf_schema: okf.concept.v1
identity:
  canonical_id: disaster_recovery_plan.plan-pfo-regional-failover
  concept_type: disaster_recovery_plan
  display_name: Partner Fulfillment Orchestrator Regional Failover Recovery Plan
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:433cb03116ac36f721efb4ec44f2f7b427e89a43bd0a27a0c0de9ce3d1febb23
  last_updated_at: '2026-09-02T21:54:04.802485Z'
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
- type: applies_to
  target_canonical_id: service.partner-fulfillment-orchestrator
- type: includes_architecture
  target_canonical_id: architecture.pfo-regional-deployment
- type: includes_procedure
  target_canonical_id: runbook.pfo-region-failover-procedure
- type: validated_by
  target_canonical_id: test_plan.pfo-regional-failover-testing
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:54:04.802485Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Fulfillment Orchestrator Regional Failover Recovery Plan

## Summary

This Cloud Disaster Recovery Plan provides a structured approach for recovering data and applications hosted in cloud environments after a disruption, ensuring business continuity for the Kroger Company. The plan specifically covers `service.partner-fulfillment-orchestrator` deployed across two Azure regions: US Central (primary) and US East 2 (secondary). The plan establishes recovery procedures with a Recovery Time Objective (RTO) of 4 hours for critical applications and a Recovery Point Objective (RPO) of 1 hour.

## Details

### Purpose and Objectives

The purpose of this plan is to provide a structured approach for recovering data and applications hosted in cloud environments after a disruption, ensuring business continuity for the Kroger Company.

Key objectives include:
- Minimize disruption to cloud-hosted services
- Ensure timely recovery of data and applications
- Maintain business continuity and operational resilience

### Scope

This plan covers `service.partner-fulfillment-orchestrator` utilized by the Kroger Company including Infrastructure as a Service (IaaS) and Platform as a Service (PaaS). This plan does not cover Software as a Service (SaaS) offerings as they are the responsibility of the vendor.

### Risk Assessment

The application is resilient to cloud failure on one region. `service.partner-fulfillment-orchestrator` is deployed across two regions: US Central and US East 2. Manual intervention is required to change the API gateway redirection URL from Central to East in case of regional failure.

### Failover Configuration

| Environment | Type | Traffic | Functionality |
|-------------|------|---------|---|
| US Central | Primary | Default | Full |
| US East 2 | Secondary | Manual configuration required to redirect to east region | Full |

When `service.partner-fulfillment-orchestrator` is having downtime in the central region and is up in the east region, traffic must be manually configured to redirect to the east region through the API Gateway.

### Recovery Process

Recovery requires manual intervention and communication with the API Program Team when a region goes down. If US Central is down, the API Gateway URL must be manually changed to the east region.

Recovery steps:
1. Configure API gateway URL to east region (requires API Program team coordination)
2. Check for pods running on Rancher East under partner-fulfillments namespace
3. Verify Grafana metrics
4. Verify Datadog logs for any errors from the service
5. Perform smoke testing on production environment (QA team responsibility)

**Recovery Time Objective (RTO):** 4 hours for critical applications

**Recovery Point Objective (RPO):** 1 hour for critical applications

### Pre-Deployment Checks

Before restoring the application, the following prerequisites must be verified:

1. Note down current pod configuration in the central region to enable revert after recovery. Verify pod stability in east region and configure pod count matching central. Ensure all pods are running with no restarts.
2. Verify dependent services are operational: Loyalty service, Profile-alias service, Offers service, and Internal Services (partner-fulfillment-data and partner-fulfillment-domain).
3. Verify the correct application tag on the east region from the latest GitHub release.
4. Configure the URL in API Gateway to redirect to east region (requires API Program team coordination).

Post-deployment checks include sanity testing of all endpoints and monitoring Grafana and Datadog logs for a period after deployment.

### Test Plan

Disaster recovery testing simulates the shutdown of one region:

1. Shutdown the primary Azure region (Central-US)
2. Verify all pre-requisites mentioned in the document are met
3. Change the URL to east URL in API Gateway
4. Verify Grafana to confirm the application can serve requests
5. Perform smoke testing on production to verify changes are reflected
6. Change the URL in API Gateway back to Central
7. Verify Grafana confirms requests are sent to central again

**Test Schedule:** Once every 6 months

### Communication Plan

**Employees:**
- Team Email: EnterpriseCustomer-BlueArmy@kroger.com
- Infrastructure Group: APP-DIG-SVC-Partner-Fulfillment

**Management Team:**
- Sunil P Bapat (IT Application Owner): sunil.bapat@kroger.com
- Nathan Subler: nathan.subler@kroger.com

### Resiliency Recovery Team

| Name | Role | Contact Details |
|------|------|---|
| Sunil P Bapat | IT Application Owner | sunil.bapat@kroger.com |
| Heather Alvey | Business Owner | heather.alvey@kroger.com |
| Venketa S Penmetsa / Santosh Boppidi / Goutam Kundu | Support Team/Escalation Point | venkata.penmetsa@kroger.com santoshreddy.boppidi@kroger.com goutam.kundu@kroger.com |

### Service Identifier

**APM Number:** APM0004876

## Related Concepts

- `service.partner-fulfillment-orchestrator` — The Partner Fulfillment Orchestrator service covered by this recovery plan
- `team.resiliency-recovery` — The team responsible for implementing and maintaining this disaster recovery plan
- `architecture.pfo-regional-deployment` — The multi-region deployment architecture of the Partner Fulfillment Orchestrator
- `runbook.pfo-region-failover-procedure` — Detailed operational procedures for executing the regional failover
- `test_plan.pfo-regional-failover-testing` — Testing plan that validates this disaster recovery capability

## Sources

- Confluence page 96895308: Partner Fulfillment Orchestrator Disaster Recovery Plan documentation (v26)
