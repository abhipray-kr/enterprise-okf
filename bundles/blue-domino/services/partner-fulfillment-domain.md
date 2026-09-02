---
type: Service
title: partner-fulfillment-domain
description: service.partner-fulfillment-domain is a core service that handles fulfillment
  operations and external partner interactions within the Partner Fulfillment domain.
  It manages fulfillment records, coordinates with external partners like Disney
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
tags:
- okf
- service
okf_schema: okf.concept.v1
identity:
  canonical_id: service.partner-fulfillment-domain
  concept_type: service
  display_name: partner-fulfillment-domain
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:1973ff8f20311391542c40f34ac19ed7a5d007a53630431d55a0080303e4e365
  last_updated_at: '2026-09-02T22:01:39.762475Z'
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
- type: exposes
  target_canonical_id: api_route.partner-fulfillment-jwks
- type: exposes
  target_canonical_id: api_route.partner-fulfillment-v1
- type: exposes_api_route
  target_canonical_id: api_route.create-fulfillment
- type: exposes_api_route
  target_canonical_id: api_route.fetch-fulfillments
- type: exposes_api_route
  target_canonical_id: api_route.update-fulfillment
- type: has_disaster_recovery_plan
  target_canonical_id: disaster_recovery_plan.plan-partner-fulfillment-services
- type: implemented_by
  target_canonical_id: repository.partner-fulfillment-domain
- type: monitored_by
  target_canonical_id: dashboard.partner-fulfillment-domain-dynatrace
- type: monitored_by
  target_canonical_id: dashboard.partner-fulfillment-domain-monitoring
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T22:01:39.762475Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# partner-fulfillment-domain

## Summary

service.partner-fulfillment-domain is a core service that handles fulfillment operations and external partner interactions within the Partner Fulfillment domain. It manages fulfillment records, coordinates with external partners like Disney, and provides APIs for creating, updating, and retrieving fulfillment data. The service operates in a distributed deployment across multiple Azure regions with comprehensive monitoring and disaster recovery capabilities.

## Details

### Purpose and Scope

service.partner-fulfillment-domain handles partner fulfillment operations and manages interactions with external systems. It processes two fulfillment flow types: Basic fulfillments (59 flow) that work with promo codes, and Non-Basic fulfillments (99 flow) that use entitlement tokens.

### API Capabilities

The service exposes three primary API routes:

- api_route.create-fulfillment (`POST /v1/fulfillments`) — Creates new fulfillment records, handles promo code association for basic fulfillments, and generates entitlement tokens for non-basic fulfillments
- api_route.fetch-fulfillments (`GET /v1/fulfillments`) — Retrieves fulfillment records with optional fulfillment codes, supporting filtering by partner, fulfillment type, customer ID, and pagination
- api_route.update-fulfillment (`PATCH /v1/fulfillments/{id}`) — Updates existing fulfillment records and associated promo codes

Additionally, api_route.partner-fulfillment-jwks provides JWKS endpoint for token validation.

### Deployment and Infrastructure

service.partner-fulfillment-domain is deployed to external_system.aks-prod-cluster across two Azure regions: US Central (primary) and US East 2 (secondary). The service runs in the partner-fulfillments Kubernetes namespace with regional variations in pod configuration and monitoring scope.

### Monitoring and Observability

The service is monitored through multiple platforms:
- DynaTrace at external_system.tgo745-dynatrace-managed-com for performance monitoring
- Grafana dashboards for metrics visualization across Central and East regions
- Datadog for service logs and situational awareness dashboards
- Rancher for Kubernetes cluster management
- Harness for service workflows and deployment orchestration

### Disaster Recovery

service.partner-fulfillment-domain is covered under disaster_recovery_plan.plan-partner-fulfillment-services with manual failover procedures. The service has a Recovery Time Objective (RTO) of 4 hours and Recovery Point Objective (RPO) of 1 hour for critical operations. Failover requires manual API Gateway URL reconfiguration to redirect traffic from Central to East region.

### External Dependencies

The service interacts with Disney endpoints and maintains profile alias integrations. It depends on associated data services for codes, tokens, and Disney partner data.

## Related Concepts

- repository.partner-fulfillment-domain — Implementation repository
- api_route.create-fulfillment — Fulfillment creation endpoint
- api_route.fetch-fulfillments — Fulfillment retrieval endpoint
- api_route.update-fulfillment — Fulfillment update endpoint
- api_route.partner-fulfillment-jwks — JWKS token endpoint
- api_route.partner-fulfillment-v1 — Primary API gateway route
- external_system.aks-prod-cluster — Production deployment infrastructure
- external_system.tgo745-dynatrace-managed-com — Monitoring platform
- disaster_recovery_plan.plan-partner-fulfillment-services — Disaster recovery procedures
- dashboard.partner-fulfillment-domain-dynatrace — DynaTrace monitoring dashboard
- dashboard.partner-fulfillment-domain-monitoring — Grafana monitoring dashboard

## Sources

- Confluence page 96895288: Support Dashboard And Monitoring
- Confluence page 96895306: partner-fulfillment-api Disaster Recovery Plan
- Confluence page 96895597: Domino Dev Ops
