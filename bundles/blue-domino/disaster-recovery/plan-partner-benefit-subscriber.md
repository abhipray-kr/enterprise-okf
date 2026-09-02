---
type: Disaster Recovery Plan
title: Partner Benefit Subscriber Disaster Recovery Plan
description: This Cloud Disaster Recovery Plan provides a structured approach for
  recovering the partner-benefit-subscriber service hosted in cloud environments after
  a disruption, ensuring business continuity for the Kroger Company. The plan covers
  Inf
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895323/partner-benefit-subscriber
tags:
- okf
- disaster_recovery_plan
okf_schema: okf.concept.v1
identity:
  canonical_id: disaster_recovery_plan.plan-partner-benefit-subscriber
  concept_type: disaster_recovery_plan
  display_name: Partner Benefit Subscriber Disaster Recovery Plan
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:8cd9700df8793fc3c182bae35e4a8d1a7d7f065f079e592c6224a81d22103ef9
  last_updated_at: '2026-09-02T21:52:42.593103Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895323'
    page_title: partner-benefit-subscriber
    page_version: 36
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895323/partner-benefit-subscriber
    role: primary
relationships:
- type: includes_deployment_process
  target_canonical_id: deployment_process.process-partner-benefit-subscriber-recovery
- type: includes_test_plan
  target_canonical_id: test_plan.partner-benefit-subscriber-failover
- type: owned_by
  target_canonical_id: team.resiliency-recovery
- type: recovery_plan_for
  target_canonical_id: service.partner-benefit-subscriber
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:52:42.593103Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: failed
---

# Partner Benefit Subscriber Disaster Recovery Plan

## Summary

This Cloud Disaster Recovery Plan provides a structured approach for recovering the partner-benefit-subscriber service hosted in cloud environments after a disruption, ensuring business continuity for the Kroger Company. The plan covers Infrastructure as a Service (IaaS) and Platform as a Service (PaaS) offerings managed by the Resiliency Recovery team, with a Recovery Time Objective (RTO) of 4 hours and Recovery Point Objective (RPO) of 5 minutes for this critical application.

## Details

### Purpose

The purpose of this Cloud Disaster Recovery Plan is to provide a structured approach for recovering data and applications hosted in cloud environments after a disruption, ensuring business continuity for the Kroger Company.

### Objectives

- Minimize disruption to cloud-hosted services
- Ensure timely recovery of data and applications
- Maintain business continuity and operational resilience

### Scope

This plan covers partner-benefit-subscriber service (service.partner-benefit-subscriber) utilized by the Kroger Company including Infrastructure as a Service (IaaS) and Platform as a Service (PaaS). This plan does not cover Software as a Service (SaaS) offerings as they are the responsibility of the vendor.

### Risk Assessment

The application is deployed across only one region (US Central). It is not resilient if US Central is down, which will not have any business impact but the cancellations of boost membership will not be processed. Cancellations must be identified and re-run manually in case of regional failure.

### Failover Configuration

| Environment | Type | Traffic | Functionality |
|---|---|---|---|
| US Central | Primary | Default | Full |
| US East 2 | Secondary | - | Recovery destination |

**Recovery Time Objective (RTO):** 4 hours  
**Recovery Point Objective (RPO):** 5 minutes

### Prerequisites

Before initiating recovery procedures, verify the following prerequisites:

1. Offer service is running successfully (check via Grafana link) - Contact: Rafiq Ruknudeen
2. Events are being published on DESP for Offer service with filter: Env: Customer - eastus2 - Prod, Topic: offer_interactions, Events: BenefitExpired - Contact: Chandler Wenninger
3. Latest deployment tag is available from GitHub release page
4. Timestamp of application downtime in Central region has been identified from alerts
5. Partition and Offset values for offer_interactions topic (all partitions 0-15) have been retrieved from DESP for Central region

### Recovery Process

The recovery process requires manual intervention and communication with Azure when the region goes down. If US Central is down, partner-benefit-subscriber must be manually deployed to the East region following these steps:

1. **Update Production Configuration:** Update/add prod configs to Harness as per East region requirements (reference: https://github.com/krogertechnology/partner-benefit-subscriber/blob/develop/configs/.prod.env)

2. **Trigger Harness Deployment:** 
   - Access Harness pipeline and click "Run Pipeline" button
   - Select Service: partner-benefit-subscriber
   - Select Primary Artifact: dockerProdPartnerBenefitSubscriber (auto-selected)
   - Specify the deployment tag
   - Specify Environment: aks-cx-encust-01-prod-eastus2-encust-prod
   - Specify Infrastructure: aks-cx-encust-01-prod-eastus2-encust-prod
   - Set Namespace: partner-fulfillments
   - Click "Run Pipeline" button and verify successful deployment

3. **Verify Pod Deployment:** Confirm pods are running on Rancher East under partner-fulfillments namespace

4. **Update Kafka Offsets:** Update KAFKA_OFFSETS environment configuration on Rancher for all applications with Partition and Offset values retrieved from DESP. This must include values for all partitions (0-15) for the offer_interactions topic. Values should be enclosed in single quotes and formatted as JSON: `'[{"topic":"offer_interactions","partition":0,"offset":41455276},{"topic":"offer_interactions","partition":1,"offset":41468224}]'`

5. **Verify Metrics:** Check Grafana for application metrics and health status

6. **Verify Logs:** Check Datadog logs for any errors from the service

7. **Perform Smoke Testing:** Execute smoke testing on production (QA team responsibility) - Contact: VARALAKSHMI MK

### Failover Scenario (Central Region Down, East Region Up)

When partner-benefit-subscriber experiences downtime in the Central region while the East region is operational:

- **Topic:** Offer Interactions
- **Event Type:** BenefitExpired event consumed by partner-benefit-subscriber
- **Recovery Focus:** Ensure the service can consume events from the East region without data loss

### Test Plan

**Test Scenario:** Shutdown of One Region

| Step | Action |
|---|---|
| Step 1 | Shutdown the primary Azure region (Central-US) |
| Step 2 | Verify if all the Pre-requisites mentioned in the document are met |
| Step 3 | Deploy the partner-benefit-subscriber to the secondary region (East-US 2) |
| Step 4 | Verify Grafana that applications are able to consume events from East |
| Step 5 | Perform Smoke testing on Prod and verify if the changes are getting reflected |

**Test Schedule:** Once every 6 months

### Communication Plan

**Employees:**
- Team Email: EnterpriseCustomer-BlueArmy@kroger.com
- Infrastructure Group Name: APP-DIG-SVC-Partner-Fulfillment

**Management Team:**
- Sunil P Bapat (sunil.bapat@kroger.com)
- Nathan Subler (nathan.subler@kroger.com)

### Resiliency Recovery Team

| Name | Role | Contact Details |
|---|---|---|
| Sunil P Bapat | IT Application Owner | sunil.bapat@kroger.com |
| Heather Alvey | Business Owner | heather.alvey@kroger.com |
| Venketa S Penmetsa / Santosh Boppidi / Goutam Kundu | Support Team/Escalation Point | venkata.penmesa@kroger.com santoshreddy.boppidi@kroger.com goutam.kundu@kroger.com |
| Heather Alvey | Business Team/Escalation Group | heather.alvey@kroger.com |

### Additional Information

- **APM Number:** APM0004876
- **Cloud Recovery Requirements:** Resiliency Infrastructure

## Related Concepts

- service.partner-benefit-subscriber
- team.resiliency-recovery
- deployment_process.process-partner-benefit-subscriber-recovery
- test_plan.partner-benefit-subscriber-failover
- event_topic.offer-interactions

## Sources

Source documentation retrieved from Confluence page: partner-benefit-subscriber (Page ID: 96895323, Space: BD)
