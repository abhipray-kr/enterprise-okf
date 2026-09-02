---
type: Service
title: Partner Fulfillment Orchestrator
description: The Partner Fulfillment Orchestrator is a core microservice that orchestrates
  fulfillment operations between the Offers domain and the Partner Fulfillment domain.
  It provides APIs for creating, updating, and fetching fulfillment records, ha
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
tags:
- okf
- service
okf_schema: okf.concept.v1
identity:
  canonical_id: service.partner-fulfillment-orchestrator
  concept_type: service
  display_name: Partner Fulfillment Orchestrator
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:767b9631375d175967daccf8da34c6c23258f094d19417c8fca1cd44c075c24e
  last_updated_at: '2026-09-02T22:02:02.283542Z'
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
    page_id: '96895308'
    page_title: partner-fulfillment-orchestrator
    page_version: 26
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895308/partner-fulfillment-orchestrator
    role: primary
relationships:
- type: coordinates_with
  target_canonical_id: service.partner-fulfillment-domain
- type: has_disaster_recovery_plan
  target_canonical_id: disaster_recovery_plan.plan-pfo-regional-failover
- type: implemented_by
  target_canonical_id: repository.partner-fulfillment-orchestrator
- type: monitored_by
  target_canonical_id: dashboard.partner-fulfillment-orchestrator-monitoring
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T22:02:02.283542Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Partner Fulfillment Orchestrator

## Summary

The Partner Fulfillment Orchestrator is a core microservice that orchestrates fulfillment operations between the Offers domain and the Partner Fulfillment domain. It provides APIs for creating, updating, and fetching fulfillment records, handling both basic (59 flow) and non-basic (99 flow) fulfillment types with support for promo code association, entitlement token generation, and redirect URL management.

## Details

### Purpose and Scope

This service operates within the Kroger Company's Infrastructure as a Service (IaaS) and Platform as a Service (PaaS) environments. It acts as a coordination point between external offer management systems and internal partner fulfillment operations, managing the end-to-end fulfillment lifecycle.

### API Operations

The service exposes three primary fulfillment operations:

**CreateFulfillment** (`POST /v1/fulfillments`)
- Initiates a new fulfillment record in the underlying data service
- Differentiates between fulfillment types and routes accordingly
- For basic fulfillments (59 flow): associates with promo codes and generates redirect URLs
- For non-basic fulfillments (99 flow): generates entitlement tokens and creates entitlement records
- Includes comprehensive error handling with immediate error returns on failures

**Update/Patch Fulfillment** (`PATCH /v1/fulfillments/{id}`)
- Retrieves and validates existing fulfillment records
- Updates fulfillment details against business rule validation
- Manages associated promo code updates or deletions
- Persists changes to the underlying data service with error capture

**Fetch Fulfillments** (`GET /v1/fulfillments`)
- Retrieves fulfillments based on partner, fulfillment type, customer ID, and pagination parameters
- Conditionally fetches associated fulfillment codes based on include parameters
- Generates redirect URLs for both basic (59 flow) and non-basic (99 flow) fulfillments
- Returns unified fulfillment and code lists with comprehensive error handling

### Deployment and Infrastructure

The service is deployed across two Azure regions for resilience:
- **Primary**: US Central
- **Secondary**: US East 2

Regional deployment details:
- **Grafana monitoring**: Central, East regions
- **DynaTrace APM**: Both production clusters in Prod and East, Central/East in lower environments
- **Rancher orchestration**: Central and East regions
- **Harness deployment**: Service workflows across Central and East environments

### Disaster Recovery

The service maintains a comprehensive disaster recovery plan with the following characteristics:

- **Recovery Time Objective (RTO)**: 4 hours for critical applications
- **Recovery Point Objective (RPO)**: 1 hour for critical applications
- **Test Schedule**: Once every 6 months

In the event of a primary region failure, manual intervention is required to reconfigure the API Gateway to redirect traffic to the secondary region. Recovery procedures include:
1. Configuring API Gateway URL to the East region
2. Verifying pod stability and count via Rancher
3. Monitoring metrics via Grafana
4. Reviewing service logs via Datadog
5. Performing smoke testing on production

### Dependencies and Integrations

The service depends on several related systems:

- **Partner Fulfillment Data Service**: Underlying data persistence layer
- **Partner Fulfillment Domain Service**: Fulfillment business logic coordination
- **Loyalty Service**: Customer loyalty data and operations
- **Profile Alias Service**: Customer profile identification
- **Offers Service**: Promotional offer management
- **Disney Integration**: External partner fulfillment and entitlement management

### APM Tracking

**APM Number**: APM0004876

### Operational Contacts

| Role | Contact |
|------|---------| 
| IT Application Owner | Sunil P Bapat (sunil.bapat@kroger.com) |
| Business Owner | Heather Alvey (heather.alvey@kroger.com) |
| Support Team Lead | Venketa S Penmetsa (venkata.penmetsa@kroger.com) |
| Support Escalation | Santosh Boppidi (santoshreddy.boppidi@kroger.com), Goutam Kundu (goutam.kundu@kroger.com) |

## Related Concepts

- [[service.partner-fulfillment-domain]] — Coordinates fulfillment domain operations
- [[repository.partner-fulfillment-orchestrator]] — Implementation repository for this service
- [[dashboard.partner-fulfillment-orchestrator-monitoring]] — Monitoring and observability dashboard
- [[disaster_recovery_plan.plan-pfo-regional-failover]] — Regional failover and recovery procedures

## Sources

- Confluence: Support Dashboard And Monitoring (Page 96895288)
- Confluence: partner-fulfillment-orchestrator (Page 96895308)
