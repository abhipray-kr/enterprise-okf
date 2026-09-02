---
type: Service
title: partner-fulfillment-data
description: partner-fulfillment-data is a data service within the partner fulfillment
  ecosystem that handles interactions with partner fulfillment databases, codes, tokens,
  and Disney endpoints. It provides backend data operations supporting the fulfil
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
tags:
- okf
- service
okf_schema: okf.concept.v1
identity:
  canonical_id: service.partner-fulfillment-data
  concept_type: service
  display_name: partner-fulfillment-data
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:bf202d1ef1aaaaf2d866e6e02f3fe3e54bd33acc9d8cd9fbf0cacb6c05837860
  last_updated_at: '2026-09-02T22:01:16.508528Z'
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
    page_id: '96895306'
    page_title: partner-fulfillment-api
    page_version: 25
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895306/partner-fulfillment-api
    role: primary
  - platform: confluence
    space_key: BD
    page_id: '96895597'
    page_title: Domino Dev Ops
    page_version: 1
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895597/Domino+Dev+Ops
    role: primary
relationships:
- type: deployed_to
  target_canonical_id: external_system.aks-prod-cluster
- type: has_disaster_recovery_plan
  target_canonical_id: disaster_recovery_plan.plan-partner-fulfillment-services
- type: implemented_by
  target_canonical_id: repository.partner-fulfillment-data
- type: monitored_by
  target_canonical_id: dashboard.partner-fulfillment-data-dynatrace
- type: monitored_by
  target_canonical_id: dashboard.partner-fulfillment-data-monitoring
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T22:01:16.508528Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# partner-fulfillment-data

## Summary

partner-fulfillment-data is a data service within the partner fulfillment ecosystem that handles interactions with partner fulfillment databases, codes, tokens, and Disney endpoints. It provides backend data operations supporting the fulfillment fulfillment domain and serves as a critical component for managing fulfillment records and associated resources.

## Details

### Purpose and Scope

The service manages data layer operations for the partner fulfillment system, specifically handling:
- Fulfillment record management in the underlying database
- Promo code associations and updates
- Token generation and entitlement management
- Disney endpoint interactions
- Profile alias and customer identification

### Core Capabilities

The service exposes three primary API operations:
- **Create Fulfillment** (`api_route.create-fulfillment`): Initiates new fulfillment records with support for both basic (59-flow) and non-basic (99-flow) fulfillment types, including promo code association and entitlement token generation.
- **Fetch Fulfillments** (`api_route.fetch-fulfillments`): Retrieves fulfillment records with optional code inclusion and redirect URL generation based on fulfillment type.
- **Update Fulfillment** (`api_route.update-fulfillment`): Modifies existing fulfillment records and associated promo codes with comprehensive validation and error handling.

### Deployment and Infrastructure

The service is deployed across multiple Azure Kubernetes Service (AKS) clusters in the US Central and US East 2 regions under the `partner-fulfillments` namespace. Multi-region deployment provides resilience with manual failover capabilities to the East region via API Gateway URL configuration.

### Monitoring and Observability

The service is monitored through multiple observability platforms:
- **DynaTrace** (`external_system.tgo745-dynatrace-managed-com`): Primary application performance monitoring across both cluster regions
- **Grafana**: Metrics dashboards for Central and East deployments
- **Rancher**: Container orchestration and pod management visibility
- **Harness**: Service workflow and deployment tracking

### Disaster Recovery

The service is included in the organization's cloud disaster recovery planning with:
- Recovery Time Objective (RTO): 4 hours for critical applications
- Recovery Point Objective (RPO): 1 hour
- Manual regional failover procedures documented in the disaster recovery plan

## Related Concepts

- api_route.create-fulfillment
- api_route.fetch-fulfillments
- api_route.update-fulfillment
- repository.partner-fulfillment-data
- external_system.tgo745-dynatrace-managed-com
- external_system.aks-prod-cluster
- disaster_recovery_plan.plan-partner-fulfillment-services
- dashboard.partner-fulfillment-data-dynatrace
- dashboard.partner-fulfillment-data-monitoring

## Sources

- Confluence page 96895288: "Support Dashboard And Monitoring" - Application Resources table describing service functionality and monitoring configuration
- Confluence page 96895306: "partner-fulfillment-api" - Disaster recovery plan covering the service with RTO/RPO objectives and failover procedures
- Confluence page 96895597: "Domino Dev Ops" - Operations documentation including monitoring links and cluster configuration details
