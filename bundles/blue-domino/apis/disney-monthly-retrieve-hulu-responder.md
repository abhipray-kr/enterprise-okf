---
type: API Endpoint
title: Disney Monthly Reconciliation Retrieve Hulu Responder
description: The Disney Monthly Reconciliation Retrieve Hulu Responder is an API endpoint
  that retrieves processed Hulu responder files from the Disney monthly reconciliation
  system and transfers them to the Kroger SFTP environment for further processin
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.disney-monthly-retrieve-hulu-responder
  concept_type: api_route
  display_name: Disney Monthly Reconciliation Retrieve Hulu Responder
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:78011f9655a01a1c0def3418f7121ef09fcfd7ab9d97666d9d268af4cbbe3234
  last_updated_at: '2026-09-02T21:45:21.520876Z'
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
  generated_at: '2026-09-02T21:45:21.520876Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Disney Monthly Reconciliation Retrieve Hulu Responder

## Summary

The Disney Monthly Reconciliation Retrieve Hulu Responder is an API endpoint that retrieves processed Hulu responder files from the Disney monthly reconciliation system and transfers them to the Kroger SFTP environment for further processing. It is exposed by the Disney monthly reconciliation central system and used by the monthly Hulu responder batch job.

## Details

### Endpoint

The endpoint is accessed via HTTP GET request to:

```
https://disney-monthly-recon-central-prod.kpsazc.dgtl.kroger.com/retrieveHuluResponder?fileName={nameOfFile}
```

### Parameters

- `fileName` (required): The name of the Hulu responder file to retrieve from the Disney file system.

### Functionality

This endpoint retrieves a specified Hulu responder file from the Disney monthly reconciliation file structure. When executed, it places the requested file in the Kroger SFTP location within the `disneyTempData` folder for subsequent batch processing.

### Access Credentials

Files are delivered to the Kroger SFTP environment with the following details:
- IP Address: 10.32.10.252
- Username: ECADMIN
- Target Directory: `/disneyTempData`

### Related Operations

The Disney monthly reconciliation system also provides:
- **getDirectoryStructure**: Endpoint to view available files in the monthly directory
- **executeResponder**: Endpoint to execute a Hulu responder file for processing
- **retrieveReconciliation**: Endpoint to retrieve reconciliation diff files

## Related Concepts

- `external_system.disney-monthly-recon-central-prod-kpsazc-dgtl-kroger-com` — The external system that exposes this API route
- `job.domino-monthly-hulu-responder-batch` — The batch job that uses this API route

## Sources

- **Confluence Page:** Domino Batch Jobs (Page ID: 96895502, Space: BD) — Documents retrieval of processed Hulu responder files from Disney to Kroger SFTP
