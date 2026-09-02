---
type: Disaster Recovery Plan
title: Partner Fulfillment API Regional Failover Recovery Plan
description: This disaster recovery plan outlines the structured approach for recovering
  the Partner Fulfillment API (service.partner-fulfillment-api) after a regional outage.
  The plan covers a multi-region deployment spanning US Central (primary) and U
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895306/partner-fulfillment-api
tags:
- okf
- disaster_recovery_plan
okf_schema: okf.concept.v1
identity:
  canonical_id: disaster_recovery_plan.plan-pfa-regional-failover
  concept_type: disaster_recovery_plan
  display_name: Partner Fulfillment API Regional Failover Recovery Plan
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:3f1ae141fb738e62e0f931d41340e819f1394b5620cd7b1f2a4a62b4ce8a3908
  last_updated_at: '2026-09-02T21:53:38.216583Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895306'
    page_title: partner-fulfillment-api
    page_version: 25
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895306/partner-fulfillment-api
    role: primary
relationships:
- type: applies_to
  target_canonical_id: service.partner-fulfillment-api
- type: includes_procedure
  target_canonical_id: runbook.pfa-region-failover-procedure
- type: validated_by
  target_canonical_id: test_plan.pfa-regional-failover-testing
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:53:38.216583Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Fulfillment API Regional Failover Recovery Plan

## Summary

This disaster recovery plan outlines the structured approach for recovering the Partner Fulfillment API (service.partner-fulfillment-api) after a regional outage. The plan covers a multi-region deployment spanning US Central (primary) and US East 2 (secondary), with manual failover procedures to ensure business continuity. The recovery targets a Recovery Time Objective (RTO) of 4 hours and Recovery Point Objective (RPO) of 1 hour for this critical application.

## Details

### Purpose

The purpose of this Cloud Disaster Recovery Plan is to provide a structured approach for recovering data and applications hosted in cloud environments after a disruption, ensuring business continuity for the Kroger company.

### Scope

This plan covers partner-fulfillment-domain and partner-fulfillment-data services utilized by the Kroger company including Infrastructure as a Service (IaaS) and Platform as a Service (PaaS). This plan does not cover Software as a Service (SaaS) offerings as they are the responsibility of the vendor.

### Objectives

- Minimize disruption to cloud-hosted services
- Ensure timely recovery of data and applications
- Maintain business continuity and operational resilience

### Risk Assessment

The application is resilient to cloud failure in one region. The service is deployed across two regions: US Central and US East 2. Manual intervention is required to change the API gateway redirection URL from Central to East during a regional failure.

### Cloud Recovery Requirements

#### Resiliency Infrastructure

The application maintains redundancy across two Azure regions:
- **US Central**: Primary environment with full functionality and default traffic
- **US East 2**: Secondary environment with full functionality; receives traffic only after manual API Gateway reconfiguration

### Failover Configuration

| Environment | Type | Traffic | Functionality |
|---|---|---|---|
| US Central | Primary | Default | Full |
| US East 2 | Secondary | Manual redirection required | Full |

### Recovery Process

**RTO and RPO Targets:**
- Critical applications RTO: 4 hours
- Critical applications RPO: 1 hour

**Recovery Steps:**

1. Configure API gateway URL to redirect traffic to the east region (requires coordination with API Program Team)
2. Verify pods running on Rancher East under partner-fulfillments namespace
3. Verify Grafana metrics to confirm service health
4. Verify Datadog logs for errors from the service
5. Perform smoke testing on production to validate recovery

### Pre-Deployment Checks

| Step | Task | Resources | Contact |
|---|---|---|---|
| 1 | Verify pod stability in east region and configure pod count to match central. Ensure all pods are running with no restarts. Document current configuration for revert procedures. | Rancher, Grafana (Domain and Data) | chikkala.durga@kroger.com, ritesh.harihar@kroger.com, ravi.yadav@kroger.com |
| 2 | Verify dependent services are operational; contact respective POCs if unavailable (Disney service) | NA | venkata.penmetsa@kroger.com, santoshreddy.boppidi@kroger.com |
| 3 | Verify image tag on east region from GitHub latest release | GitHub Latest Release | Unknown |
| 4 | Configure URL in API Gateway to redirect traffic to east region | Microsoft Teams (API Program General) | API Program Team |

### Post-Deployment Checks

- Perform sanity testing to verify all endpoints are functioning correctly via Swagger
- Monitor Grafana and Datadog logs for a period after deployment
- Consult Support Dashboard for any operational issues

### Resiliency Recovery Team

| Name | Role | Contact |
|---|---|---|
| Sunil P Bapat | IT Application Owner | sunil.bapat@kroger.com |
| Heather Alvey | Business Owner | heather.alvey@kroger.com |
| Venketa S Penmetsa | Support Team/Escalation Point | venkata.penmetsa@kroger.com |
| Santosh Boppidi | Support Team/Escalation Point | santoshreddy.boppidi@kroger.com |
| Goutam Kundu | Support Team/Escalation Point | goutam.kundu@kroger.com |

### Communication Plan

**Employees:**
- Team Email: EnterpriseCustomer-BlueArmy@kroger.com
- Infrastructure Group: APP-DIG-SVC-Partner-Fulfillment

**Management Escalation:**
- Sunil P Bapat (sunil.bapat@kroger.com)
- Nathan Subler (nathan.subler@kroger.com)

### Testing Plan

**Test Schedule:** Once every 6 months

**Test Scenario:** Shutdown of one region

| Step | Activity |
|---|---|
| 1 | Shutdown the primary Azure region (Central-US) |
| 2 | Verify all pre-requisites documented in this plan are met |
| 3 | Change URL to east region in API Gateway |
| 4 | Verify Grafana metrics show the application serving requests |
| 5 | Perform smoke testing on production and verify changes are reflected |
| 6 | If successful, change API Gateway URL back to Central |
| 7 | Verify Grafana shows requests routing back to central region |

### Supporting Information

- **APM Number:** APM0004876
- **Services Covered:** partner-fulfillment-domain, partner-fulfillment-data

## Related Concepts

- **service.partner-fulfillment-api** — The primary service subject to this recovery plan
- **runbook.pfa-region-failover-procedure** — Operational procedures for executing regional failover
- **test_plan.pfa-regional-failover-testing** — Validation plan for disaster recovery testing

## Sources

- **Confluence Page 96895306** (Space: BD) — Partner Fulfillment API disaster recovery documentation containing complete plan details, procedures, team contacts, and testing schedules
