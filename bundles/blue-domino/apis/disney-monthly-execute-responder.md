---
type: API Endpoint
title: Disney Monthly Reconciliation Execute Hulu Responder
description: The Execute Hulu Responder endpoint processes monthly Hulu responder
  files from the Disney monthly reconciliation system. This API route accepts a responder
  file name as a parameter and executes the file against the Disney monthly reconcili
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.disney-monthly-execute-responder
  concept_type: api_route
  display_name: Disney Monthly Reconciliation Execute Hulu Responder
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:01ae7fa37a4fabd807140ce9557093387464ee47a08000d3be8a0a3d6dcbbf5d
  last_updated_at: '2026-09-02T21:44:45.696166Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895502'
    page_title: Domino Batch Jobs
    page_version: 5
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
    role: primary
relationships:
- type: exposed_by
  target_canonical_id: external_system.disney-monthly-recon-central-prod-kpsazc-dgtl-kroger-com
- type: used_by
  target_canonical_id: job.domino-monthly-hulu-responder-batch
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:44:45.696166Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Disney Monthly Reconciliation Execute Hulu Responder

## Summary

The Execute Hulu Responder endpoint processes monthly Hulu responder files from the Disney monthly reconciliation system. This API route accepts a responder file name as a parameter and executes the file against the Disney monthly reconciliation central system, enabling manual or batch-triggered processing of monthly reconciliation data.

## Details

### Purpose
Executes responder files stored in the ResponderFile directory on the Disney monthly reconciliation system. The responder files contain reconciliation data that needs to be processed monthly as part of the Disney-Kroger reconciliation workflow.

### Endpoint
**HTTP Endpoint:** `/executeResponder`

**Full URL:** `https://disney-monthly-recon-central-prod.kpsazc.dgtl.kroger.com/executeResponder`

### Parameters
- **fileName** (required): The name of the responder file to execute from the ResponderFile directory

### Usage
```
curl --location 'https://disney-monthly-recon-central-prod.kpsazc.dgtl.kroger.com/executeResponder?fileName={nameOfFile}'
```

### Context
Responder files are initially placed in the ResponderFile directory (as listed by the getDirectoryStructure endpoint). The executeResponder endpoint allows operators to trigger processing of these files for monthly reconciliation purposes. File names follow patterns such as `kroger_202410.csv`.

### Exposure
Exposed by external_system.disney-monthly-recon-central-prod-kpsazc-dgtl-kroger-com

## Related Concepts

- `external_system.disney-monthly-recon-central-prod-kpsazc-dgtl-kroger-com` — The external system that exposes this endpoint
- `job.domino-monthly-hulu-responder-batch` — The batch job that uses this endpoint

## Sources

- [Domino Batch Jobs](https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs) — Page 96895502, Confluence space BD
