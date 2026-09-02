---
type: Deployment Process
title: Partner Benefit Subscriber Recovery Deployment
description: This deployment process describes the recovery procedure for the `service.partner-benefit-subscriber`
  service when the primary US Central region experiences an outage. The process involves
  manually deploying the service to the East-US 2 sec
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895323/partner-benefit-subscriber
tags:
- okf
- deployment_process
okf_schema: okf.concept.v1
identity:
  canonical_id: deployment_process.process-partner-benefit-subscriber-recovery
  concept_type: deployment_process
  display_name: Partner Benefit Subscriber Recovery Deployment
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:f21d75aec5b053da0a89458d825c04ed2f150745dd0316f5319309c09b11aefb
  last_updated_at: '2026-09-02T21:52:19.304243Z'
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
- type: deployment_procedure_for
  target_canonical_id: service.partner-benefit-subscriber
- type: part_of_disaster_recovery_plan
  target_canonical_id: disaster_recovery_plan.plan-partner-benefit-subscriber
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:52:19.304243Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Benefit Subscriber Recovery Deployment

## Summary

This deployment process describes the recovery procedure for the `service.partner-benefit-subscriber` service when the primary US Central region experiences an outage. The process involves manually deploying the service to the East-US 2 secondary region and restoring message consumption from Kafka topics with appropriate partition offsets.

## Details

### Purpose

The purpose of this deployment process is to provide a structured approach for recovering the partner-benefit-subscriber service after a regional disruption, ensuring business continuity for the Kroger Company.

### Scope

This plan covers partner-benefit-subscriber utilized by the Kroger Company including Infrastructure as a Service (IaaS) and Platform as a Service (PaaS).

### Recovery Time Objectives

- **RTO (Recovery Time Objective):** 4 hours for critical applications
- **RPO (Recovery Point Objective):** 5 minutes for critical applications

### Prerequisites

Before initiating recovery, the following prerequisites must be verified:

1. Verify that the Offer service is running successfully via Grafana
2. Verify that events are being published to Desp for the Offer service topic `offer_interactions`
3. Obtain the latest Docker image tag to be deployed from the GitHub latest release
4. Identify the timestamp when the Central region applications went down using alert data
5. Fetch partition and offset information manually from Desp Central for the `offer_interactions` topic (all partitions 0-15)

### Recovery Steps

1. **Update Configuration:** Update production configs in Harness based on East-US 2 region specifications from `repository.partner-benefit-subscriber` (`/blob/develop/configs/.prod.env`)

2. **Deploy via Harness Pipeline:** Trigger deployment from the Harness pipeline with the following parameters:
   - Service: partner-benefit-subscriber
   - Primary Artifact: dockerProdPartnerBenefitSubscriber
   - Environment: aks-cx-encust-01-prod-eastus2-encust-prod
   - Infrastructure: aks-cx-encust-01-prod-eastus2-encust-prod
   - Namespace: partner-fulfillments

3. **Verify Pod Deployment:** Check that pods are running on Rancher East under the partner-fulfillments namespace

4. **Configure Kafka Offsets:** Update the KAFKA_OFFSETS environment configuration on Rancher with partition and offset values retrieved from Desp for the `offer_interactions` topic. Values must be formatted as JSON with all partitions (0-15) enclosed in single quotes

5. **Verify Metrics:** Monitor Grafana dashboards to confirm service metrics are being collected from the East region

6. **Check Logs:** Verify Datadog logs for any errors from the service

7. **Smoke Testing:** Perform production smoke testing to verify that changes are reflected correctly

### Failover Configuration

| Environment | Type | Traffic | Functionality |
|---|---|---|---|
| US Central | Primary | Default | Full |
| US East 2 | Secondary | Failover | Full |

### Risk Assessment

The application is deployed across only one region (US Central). If US Central becomes unavailable, there is no operational risk to business continuity once the service is recovered in the East-US 2 region, though boost membership cancellations that occur during the outage will need to be reprocessed manually.

### Recovery Team

| Role | Responsibility |
|---|---|
| IT Application Owner | sunil.bapat@kroger.com |
| Business Owner | heather.alvey@kroger.com |
| Support Team/Escalation | venkata.penmesa@kroger.com, santoshreddy.boppidi@kroger.com, goutam.kundu@kroger.com |

### Testing Schedule

Disaster recovery testing is conducted once every 6 months.

### Test Scenario

**Scenario:** Shutdown of Primary Region

1. Shutdown the primary Azure region (Central-US)
2. Verify that all prerequisites are met
3. Deploy partner-benefit-subscriber to the secondary region (East-US 2)
4. Verify Grafana metrics to confirm apps are consuming events from the East region
5. Perform smoke testing on production and verify changes are reflected

## Related Concepts

- `service.partner-benefit-subscriber` — The service being recovered
- `repository.partner-benefit-subscriber` — The repository containing deployment configuration
- `disaster_recovery_plan.plan-partner-benefit-subscriber` — The overall disaster recovery plan

## Sources

- Confluence page 96895323 (partner-benefit-subscriber disaster recovery documentation)
