---
type: Test Plan
title: Partner Benefit Subscriber Failover Test Plan
description: This test plan validates the disaster recovery procedure for the Partner
  Benefit Subscriber service when its primary Azure region (Central-US) becomes unavailable.
  The test exercises failover to the secondary region (East-US 2) and verifies
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895323/partner-benefit-subscriber
tags:
- okf
- test_plan
okf_schema: okf.concept.v1
identity:
  canonical_id: test_plan.partner-benefit-subscriber-failover
  concept_type: test_plan
  display_name: Partner Benefit Subscriber Failover Test Plan
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:7e025c02363e633f158069705ec113c4e71c720172717ca605ac734b9b0da9b6
  last_updated_at: '2026-09-02T22:02:44.500239Z'
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
- type: validates
  target_canonical_id: deployment_process.process-partner-benefit-subscriber-recovery
- type: validates
  target_canonical_id: disaster_recovery_plan.plan-partner-benefit-subscriber
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T22:02:44.500239Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Partner Benefit Subscriber Failover Test Plan

## Summary

This test plan validates the disaster recovery procedure for the Partner Benefit Subscriber service when its primary Azure region (Central-US) becomes unavailable. The test exercises failover to the secondary region (East-US 2) and verifies that the service can continue consuming and processing events during regional outage scenarios. The plan targets a Recovery Time Objective (RTO) of 4 hours and Recovery Point Objective (RPO) of 5 minutes for this critical application.

## Details

### Purpose

The purpose of this test plan is to provide a structured approach for validating the recovery procedures of the Partner Benefit Subscriber application hosted in cloud environments after a regional disruption, ensuring business continuity for the Kroger Company.

### Scope

This test plan covers the Partner Benefit Subscriber service utilized by the Kroger Company including Infrastructure as a Service (IaaS) and Platform as a Service (PaaS) deployments. This plan does not cover Software as a Service (SaaS) offerings as they are the responsibility of the vendor.

### Test Objectives

- Validate the failover mechanism from Central-US to East-US 2 region
- Verify service availability and event consumption during failover
- Confirm proper deployment and configuration in secondary region
- Ensure data consistency through Kafka offset management

### Test Scenario: Shutdown of One Region

The primary test scenario exercises regional failure recovery:

1. Shut down the primary Azure region (Central-US)
2. Verify all prerequisites mentioned in the supporting disaster recovery plan are met
3. Deploy the Partner Benefit Subscriber to the secondary region (East-US 2)
4. Verify via Grafana that applications can consume events from the East-US 2 region
5. Perform smoke testing on production and verify that changes are getting reflected

### Recovery Process

Manual intervention and communication with Azure will be required when the region goes down. The recovery procedure includes:

1. Update production configurations in Harness per East-US 2 specifications
2. Trigger deployment from the Harness pipeline using the latest container image tag, specifying Environment as `aks-cx-encust-01-prod-eastus2-encust-prod` and Namespace as `partner-fulfillments`
3. Verify successful pod deployment on Rancher East under the `partner-fulfillments` namespace
4. Update KAFKA_OFFSETS environment configuration on Rancher with Partition and Offset values retrieved from Desp for the `offer_interactions` topic (all partitions 0-15), formatted as JSON
5. Verify metrics in Grafana to confirm service health
6. Review Datadog logs for any service errors
7. Perform smoke testing on production (QA team responsibility)

### Pre-requisites for Failover

Before initiating failover, verify:

1. Offer service is running successfully (verify via Grafana)
2. Events are being published on Desp for the Offer service with topic `offer_interactions` and event `BenefitExpired`
3. Latest deployment tag is available from GitHub releases
4. Identify the timestamp of Central region downtime from alerts, then fetch Partition and Offset values from Desp for all partitions (0-15) of the `offer_interactions` topic

### Failover Configuration

| Environment | Type | Traffic | Functionality |
|-------------|------|---------|-----------------|
| US Central | Primary | Default | Full |
| US East 2 | Secondary | Active during failover | Full |

### Recovery Objectives

- **Recovery Time Objective (RTO)**: 4 hours for critical applications
- **Recovery Point Objective (RPO)**: 5 minutes for critical applications

### Test Schedule

Testing frequency: Once every 6 months

### Risk Assessment

The Partner Benefit Subscriber application is currently deployed across only the US Central region. Regional failure results in no automatic failover capability. Impact of Central-US downtime: Benefit cancellations will not be processed and must be manually identified and re-run.

## Related Concepts

- service.partner-benefit-subscriber
- disaster_recovery_plan.partner-benefit-subscriber
- deployment_process.process-partner-benefit-subscriber-recovery

## Sources

- Confluence page 96895323: partner-benefit-subscriber disaster recovery and failover documentation
