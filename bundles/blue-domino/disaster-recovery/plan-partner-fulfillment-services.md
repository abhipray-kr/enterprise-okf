---
type: Disaster Recovery Plan
title: Partner Fulfillment Services Disaster Recovery Plan
description: This disaster recovery plan provides a structured approach for recovering
  the partner-fulfillment-domain and partner-fulfillment-data services after a disruption
  in the cloud environment. The plan ensures business continuity through multi-r
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895306/partner-fulfillment-api
tags:
- okf
- disaster_recovery_plan
okf_schema: okf.concept.v1
identity:
  canonical_id: disaster_recovery_plan.plan-partner-fulfillment-services
  concept_type: disaster_recovery_plan
  display_name: Partner Fulfillment Services Disaster Recovery Plan
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:c8129005ebc2e33082137a5a349b8aa1848154383ba6357227af16dffc6ed6f5
  last_updated_at: '2026-09-02T21:53:11.899909Z'
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
- type: owned_by
  target_canonical_id: team.resiliency-recovery
- type: recovery_plan_for
  target_canonical_id: service.partner-fulfillment-data
- type: recovery_plan_for
  target_canonical_id: service.partner-fulfillment-domain
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:53:11.899909Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Fulfillment Services Disaster Recovery Plan

## Summary

This disaster recovery plan provides a structured approach for recovering the partner-fulfillment-domain and partner-fulfillment-data services after a disruption in the cloud environment. The plan ensures business continuity through multi-region deployment across US Central (primary) and US East 2 (secondary), with manual failover procedures and defined recovery objectives.

## Details

### Purpose

The purpose of this Cloud Disaster Recovery Plan is to provide a structured approach for recovering data and applications hosted in cloud environments after a disruption, ensuring business continuity for the Kroger company.

### Scope

This plan covers partner-fulfillment-domain and partner-fulfillment-data services utilized by the Kroger company including Infrastructure as a Service (IaaS) and Platform as a Service (PaaS). This plan does not cover Software as a Service (SaaS) offerings as they are the responsibility of the vendor.

### Objectives

- Minimize disruption to cloud-hosted services
- Ensure timely recovery of data and applications
- Maintain business continuity and operational resilience

### Services Covered

- service.partner-fulfillment-domain
- service.partner-fulfillment-data

### Risk Assessment

The application is resilient to cloud failure in one region, as it is deployed across two regions: US Central and US East 2. Manual intervention is required to change the API Gateway redirection URL from Central to East.

### Recovery Objectives

**Recovery Time Objective (RTO):** 4 hours for critical applications

**Recovery Point Objective (RPO):** 1 hour for critical applications

### Resiliency Recovery Team

| Name | Role | Contact Details |
|------|------|--------------------|
| Sunil P Bapat | IT Application Owner | sunil.bapat@kroger.com |
| Heather Alvey | Business Owner | heather.alvey@kroger.com |
| Venketa S Penmetsa / Santosh Boppidi / Goutam Kundu | Support Team/Escalation Point | venkata.penmetsa@kroger.com santoshreddy.boppidi@kroger.com goutam.kundu@kroger.com |
| Heather Alvey | Business Team/Escalation Group | heather.alvey@kroger.com |

### Communication Plan

**Employees:**
- Team Email: EnterpriseCustomer-BlueArmy@kroger.com
- Infra Group Name: APP-DIG-SVC-Partner-Fulfillment

**Management Team:**
- Sunil P Bapat - sunil.bapat@kroger.com
- Nathan Subler - nathan.subler@kroger.com

### Recovery Process

Manual intervention and communication with API Program Team will be required when the region goes down. If the US Central is down, the API Gateway URL must be manually changed to the east region.

| Step | Action | Reference |
|------|--------|-----------|
| 1 | Configure API Gateway URL to east region | API Program - Microsoft Teams |
| 2 | Verify pods running on Rancher East under partner-fulfillments namespace | Rancher East Prod |
| 3 | Verify Grafana for metrics | Grafana Links (Domain and Data) |
| 4 | Verify Datadog logs for errors from the service | Datadog |
| 5 | Perform smoke testing on production | Contact: VARALAKSHMI MK |

### Failover Configuration

| Environment | Type | Traffic | Functionality |
|-------------|------|---------|------------------|
| US Central | Primary | Default | Full |
| US East 2 | Secondary | Manual redirection to east region | Full |

### Pre-Deployment Checks

| Step | Action | Reference | Contact |
|------|--------|-----------|---------|
| 1 | Verify pod stability in east region; configure pod count to match central; ensure all pods running with no restarts; note current configuration for revert | Rancher Link, Grafana Links | chikkala.durga@kroger.com, ritesh.harihar@kroger.com, ravi.yadav@kroger.com |
| 2 | Verify dependent services are alive; contact respective POCs (Disney service) | NA | Disney: venkata.penmetsa@kroger.com, santoshreddy.boppidi@kroger.com |
| 3 | Verify tag on east region; obtain tag from GitHub latest release | GitHub Latest Release | Unknown |
| 4 | Configure URL in API Gateway to redirect to east region | Contact API Program team | API Program - Microsoft Teams |

**Post-Deployment Checks:**
- Perform sanity testing to verify all endpoints are functioning correctly
- Monitor Grafana and Datadog logs for extended period after deployment

### Testing

**Test Schedule:** Once every 6 months

**Test Scenario: Shutdown of One Region**

1. Shutdown the primary Azure region (Central-US)
2. Verify all pre-requisites mentioned in the document are met
3. Change the URL to east URL in API-Gateway
4. Verify Grafana to confirm the application can serve requests
5. Perform smoke testing on production and verify changes are reflected
6. If everything is working, change the API Gateway URL back to Central
7. Verify requests are sent to central region via Grafana

### Additional Information

**APM Number:** APM0004876

## Related Concepts

- service.partner-fulfillment-domain
- service.partner-fulfillment-data
- team.resiliency-recovery

## Sources

- Confluence page 96895306: partner-fulfillment-api disaster recovery plan documentation
