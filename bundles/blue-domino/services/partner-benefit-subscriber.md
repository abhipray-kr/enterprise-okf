---
type: Service
title: Partner Benefit Subscriber
description: Partner Benefit Subscriber is a microservice that subscribes to events
  from the offer service to handle expired benefits and subsequently cancels related
  fulfillments in the partner-fulfillment domain. The service is part of the Partner
  Ful
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
tags:
- okf
- service
okf_schema: okf.concept.v1
identity:
  canonical_id: service.partner-benefit-subscriber
  concept_type: service
  display_name: Partner Benefit Subscriber
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:2e19d43bc95633b22c26890a3fb28b3daab738534eae99fa6265d2d925ac561b
  last_updated_at: '2026-09-02T22:00:50.147975Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895288'
    page_title: Support Dashboard And Monotoring
    page_version: 11
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
    role: primary
  - platform: confluence
    space_key: BD
    page_id: '96895323'
    page_title: partner-benefit-subscriber
    page_version: 36
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895323/partner-benefit-subscriber
    role: primary
relationships:
- type: consumes_event_topic
  target_canonical_id: event.topic-offer-interactions
- type: depends_on
  target_canonical_id: external_system.disney
- type: has_disaster_recovery_plan
  target_canonical_id: disaster_recovery_plan.plan-partner-benefit-subscriber
- type: implemented_by
  target_canonical_id: repository.partner-benefit-subscriber
- type: modifies_state_in
  target_canonical_id: service.partner-fulfillment-domain
- type: monitored_by
  target_canonical_id: dashboard.partner-benefit-subscriber-monitoring
- type: owned_by
  target_canonical_id: team.resiliency-recovery
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T22:00:50.147975Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Benefit Subscriber

## Summary

Partner Benefit Subscriber is a microservice that subscribes to events from the offer service to handle expired benefits and subsequently cancels related fulfillments in the partner-fulfillment domain. The service is part of the Partner Fulfillment ecosystem and is operated by the Resiliency Recovery team for the Kroger Company. It processes BenefitExpired events from the offer_interactions event topic to ensure timely cancellation of boost memberships when benefits expire.

## Details

### Purpose and Functionality

The service subscribes to events from event_topic.offer-interactions for expired benefits and automatically cancels the corresponding fulfillment records in service.partner-fulfillment-domain. This automated workflow ensures that expired benefits are properly handled without manual intervention.

### Event Consumption

The service consumes BenefitExpired events from event_topic.offer-interactions with the following characteristics:
- Topic: offer_interactions
- Partition Count: 16 partitions (0-15)
- Event Type: BenefitExpired
- Event Source: Handled by offer service publishing to event_topic.offer-interactions

### Deployment and Infrastructure

- **Implementations**: The service is implemented by repository.partner-benefit-subscriber
- **Primary Deployment Region**: US Central (single region deployment)
- **Monitoring Tools**: Grafana, DynaTrace, Rancher, Harness (all deployed to Central region)
- **Namespaces**: partner-fulfillments
- **APM Number**: APM0004876

### Dependencies

- **Event Topic**: Consumes events from event_topic.offer-interactions to trigger cancellations
- **External System**: Depends on external_system.disney for partner interactions
- **Related Services**: Modifies state in service.partner-fulfillment-domain when processing cancellations

### Disaster Recovery

- **RTO (Recovery Time Objective)**: 4 hours for critical applications
- **RPO (Recovery Point Objective)**: 5 minutes for critical applications
- **Test Schedule**: Once every 6 months
- **Current Risk**: Application is deployed across only one region (US Central). If US Central becomes unavailable, boost membership cancellations cannot be processed and must be re-run manually. The service has a documented disaster_recovery_plan.plan-partner-benefit-subscriber for failover to the East-US 2 region.

### Operations and Support

- **Owning Team**: team.resiliency-recovery
- **IT Application Owner**: Sunil P Bapat (sunil.bapat@kroger.com)
- **Business Owner**: Heather Alvey (heather.alvey@kroger.com)
- **Support Team**: Venketa S Penmetsa, Santosh Boppidi, Goutam Kundu
- **Team Email**: EnterpriseCustomer-BlueArmy@kroger.com
- **Infrastructure Group**: APP-DIG-SVC-Partner-Fulfillment
- **Monitoring**: dashboard.partner-benefit-subscriber-monitoring

### Recovery Procedures

In the event of a region failure, manual intervention is required to:
1. Update production configurations in Harness for the East-US 2 region
2. Deploy the service from the latest available Docker image tag
3. Verify pod deployment in Rancher under partner-fulfillments namespace
4. Update KAFKA_OFFSETS environment configuration with partition and offset values from the Desp event system
5. Verify metrics in Grafana
6. Verify logs in Datadog
7. Perform smoke testing on production

## Related Concepts

- event_topic.offer-interactions
- external_system.disney
- repository.partner-benefit-subscriber
- service.partner-fulfillment-domain
- team.resiliency-recovery
- disaster_recovery_plan.plan-partner-benefit-subscriber
- dashboard.partner-benefit-subscriber-monitoring

## Sources

- Confluence: Support Dashboard And Monotoring (Page 96895288)
- Confluence: partner-benefit-subscriber (Page 96895323)
