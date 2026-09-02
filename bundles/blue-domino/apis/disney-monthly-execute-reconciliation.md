---
type: API Endpoint
title: Disney Monthly Reconciliation Execute
description: The Disney Monthly Reconciliation Execute endpoint is an API route that
  triggers the execution of the monthly reconciliation process. It creates a reconciliation
  file based on current production data and sends it to Disney's SFTP environmen
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.disney-monthly-execute-reconciliation
  concept_type: api_route
  display_name: Disney Monthly Reconciliation Execute
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:df924133ebc5b90ba61f294fd82b334c045dd01483f97e14e1340f1227804b09
  last_updated_at: '2026-09-02T21:44:27.483409Z'
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
  target_canonical_id: external_system.disney-monthly-recon-prod-kpsazc-dgtl-kroger-com
- type: used_by
  target_canonical_id: job.domino-monthly-reconciliation-batch
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:44:27.483409Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Disney Monthly Reconciliation Execute

## Summary

The Disney Monthly Reconciliation Execute endpoint is an API route that triggers the execution of the monthly reconciliation process. It creates a reconciliation file based on current production data and sends it to Disney's SFTP environment.

## Details

**Endpoint URL:**
```
https://disney-monthly-recon-prod.kpsazc.dgtl.kroger.com/execute
```

**Purpose:**
This endpoint allows manual execution of the monthly reconciliation batch job. When invoked, it generates a file based on the current production data flowing through the domain service and automatically sends this file to Disney's SFTP environment.

**How to Use:**
The endpoint can be called using curl:
```
curl --location 'https://disney-monthly-recon-prod.kpsazc.dgtl.kroger.com/execute'
```

**Related Operations:**
- **List Files:** Retrieved via `listFiles` endpoint to view all files on Disney's SFTP
- **Retrieve Reconciliation:** Retrieved via `retrieveReconciliation` endpoint with a specific filename parameter
- **File Destination:** Diff files are sent to the Kroger SFTP environment at IP 10.32.10.252 in the `/disneyTempData` directory (username: ECADMIN)

**Deployment and Scheduling:**
This endpoint is part of the monthly batch job, which was initially released on 10-7-2024. As of 11-13-2024, the reconciliation job was updated to use the Audit endpoint and to add logic for updating redeemed dates in offers.

## Related Concepts

- external_system.disney-monthly-recon-prod-kpsazc-dgtl-kroger-com (exposed by this system)
- job.domino-monthly-reconciliation-batch (used by this job)

## Sources

- [Domino Batch Jobs](https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs) - Confluence page documenting batch job operations, including endpoint specifications and manual execution instructions
