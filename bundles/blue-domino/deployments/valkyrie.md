---
type: Deployment Process
title: valkyrie
description: valkyrie
resource: null
tags:
- okf
- deployment_process
okf_schema: okf.concept.v1
identity:
  canonical_id: deployment_process.valkyrie
  concept_type: deployment_process
  display_name: valkyrie
  domain: unknown
  lifecycle_status: active
version:
  content_version: 1
  source_fingerprint: remediation:deployment_process.valkyrie
  last_updated_at: '2026-09-03T07:06:20.896600Z'
aliases: []
provenance:
  source_documents:
  - platform: github
    space_key: null
    page_id: code_comprehension/deployment_ground_truth.json:facts[service_name=valkyrie]
    page_title: valkyrie
    page_version: null
    url: null
    role: derived
relationships: []
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: remediation
  generated_at: '2026-09-03T07:06:20.896600Z'
quality:
  standardization: pending
  coherence: pending
  comprehensiveness: pending
---

## Service Name
valkyrie

## Harness Configuration
- `.harness/Services/Caspian/valkyrie.yaml`

## Helm Value Overrides
- `ValueYAMLOverrides/valkyrie/valuesoverrideValues.yaml`
- `ValueYAMLOverrides/valkyrie/valuesoverridethanosValuesValkyrie.yaml`

## Kubernetes Templates
- `ValueYAMLOverrides/valkyrie/templates/deployment.yaml`
- `ValueYAMLOverrides/valkyrie/templates/service.yaml`
- `ValueYAMLOverrides/valkyrie/templates/ingress.yaml`
