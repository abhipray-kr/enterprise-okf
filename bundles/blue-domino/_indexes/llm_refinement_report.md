# OKF Graph Refinement Audit

Applied edge additions: 11
Skipped edge additions: 0

## Applied edges

| From | Type | To | Confidence |
| --- | --- | --- | --- |
| 96894978:service.token-data-service | exposes | 96894978:api_route.sign-token | High (92%) |
| 96894978:service.token-data-service | exposes | 96894978:api_route.get-jwks | High (92%) |
| 96894978:service.partner-fulfillment-domain | exposes | 96894978:api_route.create-fulfillment | High (90%) |
| 96894978:service.partner-fulfillment-domain | exposes | 96894978:api_route.fetch-fulfillments | High (90%) |
| 96894978:service.partner-fulfillment-domain | exposes | 96894978:api_route.update-fulfillment | High (90%) |
| 96894978:deployment_process.process-domino-jwk-token-rotation | deploys | 96894978:service.domino | High (88%) |
| 96894978:deployment_process.process-partner-benefit-subscriber-recovery | deploys | 96894978:service.partner-benefit-subscriber | High (86%) |
| 96894978:deployment_process.process-domino-batch-jobs-release | deploys | 96894978:job.domino-monthly-reconciliation-batch | High (88%) |
| 96894978:deployment_process.process-domino-batch-jobs-release | deploys | 96894978:job.domino-nightly-daf-responder-batch | High (88%) |
| 96894978:disaster_recovery_plan.plan-partner-benefit-subscriber | documents | 96894978:deployment_process.process-partner-benefit-subscriber-recovery | High (85%) |
| 96894978:disaster_recovery_plan.plan-partner-fulfillment-services | related_to | 96894978:disaster_recovery_plan.plan-pfo-regional-failover | Medium (72%) |

## Skipped edges

No edges skipped.

## Raw LLM patch

```yaml
```yaml
schema_version: okf.graph_refinement_patch.v1
summary: "Proposed safe edge additions using only allowed relationship types, derived from existing documented relationships in the sample edges. All additions connect nodes already in the enterprise node ID list and use only standard relationship types."
edge_additions:
  - from: "96894978:service.token-data-service"
    type: exposes
    to: "96894978:api_route.sign-token"
    confidence: "High (92%)"
    reason: "Explicit 'implemented_by' relationship in sample edges establishes service implements this route; reverse relationship well-supported."
  - from: "96894978:service.token-data-service"
    type: exposes
    to: "96894978:api_route.get-jwks"
    confidence: "High (92%)"
    reason: "Explicit 'implemented_by' relationship in sample edges; service clearly exposes this endpoint."
  - from: "96894978:service.partner-fulfillment-domain"
    type: exposes
    to: "96894978:api_route.create-fulfillment"
    confidence: "High (90%)"
    reason: "Sample edges document 'exposes_api_route' relationship; standardizing to allowed type."
  - from: "96894978:service.partner-fulfillment-domain"
    type: exposes
    to: "96894978:api_route.fetch-fulfillments"
    confidence: "High (90%)"
    reason: "Sample edges document 'exposes_api_route' relationship; standardizing to allowed type."
  - from: "96894978:service.partner-fulfillment-domain"
    type: exposes
    to: "96894978:api_route.update-fulfillment"
    confidence: "High (90%)"
    reason: "Sample edges document 'exposes_api_route' relationship; standardizing to allowed type."
  - from: "96894978:deployment_process.process-domino-jwk-token-rotation"
    type: deploys
    to: "96894978:service.domino"
    confidence: "High (88%)"
    reason: "Sample edges document 'deploys_service' relationship; standardizing to allowed type."
  - from: "96894978:deployment_process.process-partner-benefit-subscriber-recovery"
    type: deploys
    to: "96894978:service.partner-benefit-subscriber"
    confidence: "High (86%)"
    reason: "Sample edges document 'deployment_procedure_for' relationship with clear deployment intent."
  - from: "96894978:deployment_process.process-domino-batch-jobs-release"
    type: deploys
    to: "96894978:job.domino-monthly-reconciliation-batch"
    confidence: "High (88%)"
    reason: "Sample edges document 'releases_to_production' relationship indicating deployment responsibility."
  - from: "96894978:deployment_process.process-domino-batch-jobs-release"
    type: deploys
    to: "96894978:job.domino-nightly-daf-responder-batch"
    confidence: "High (88%)"
    reason: "Sample edges document 'releases_to_production' relationship indicating deployment responsibility."
  - from: "96894978:disaster_recovery_plan.plan-partner-benefit-subscriber"
    type: documents
    to: "96894978:deployment_process.process-partner-benefit-subscriber-recovery"
    confidence: "High (85%)"
    reason: "Sample edges show 'includes_deployment_process' relationship; plan documents the recovery procedure."
  - from: "96894978:disaster_recovery_plan.plan-partner-fulfillment-services"
    type: related_to
    to: "96894978:disaster_recovery_plan.plan-pfo-regional-failover"
    confidence: "Medium (72%)"
    reason: "Both plans govern related partner-fulfillment infrastructure; regional failover complements service-level recovery."
```
```
