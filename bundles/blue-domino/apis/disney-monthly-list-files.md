---
type: API Endpoint
title: Disney Monthly Reconciliation List Files
description: The Disney Monthly Reconciliation List Files API endpoint (`/listFiles`)
  is a REST API route that lists all files currently available in the Disney SFTP
  environment. This endpoint is part of the monthly reconciliation process between
  Kroger
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.disney-monthly-list-files
  concept_type: api_route
  display_name: Disney Monthly Reconciliation List Files
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:c5659deec3f068206973251da64ad1fe67b2315be33ec5591cae66a2f6d8062d
  last_updated_at: '2026-09-02T21:45:03.861411Z'
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
  generated_at: '2026-09-02T21:45:03.861411Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Disney Monthly Reconciliation List Files

## Summary

The Disney Monthly Reconciliation List Files API endpoint (`/listFiles`) is a REST API route that lists all files currently available in the Disney SFTP environment. This endpoint is part of the monthly reconciliation process between Kroger and Disney, allowing operators to view what files are available for retrieval after the monthly batch job completion.

## Details

### Purpose

The `/listFiles` endpoint provides visibility into the Disney SFTP directory structure following monthly batch job execution. Once Disney completes the monthly reconciliation batch job, they send back a diff file containing differences between their system and Kroger's. This endpoint enables operators to discover and enumerate files available for processing.

### Endpoint Information

- **URL**: `https://disney-monthly-recon-prod.kpsazc.dgtl.kroger.com/listFiles`
- **HTTP Method**: GET
- **Protocol**: HTTPS

### Usage

The endpoint can be invoked using a curl command:

```
curl --location 'https://disney-monthly-recon-prod.kpsazc.dgtl.kroger.com/listFiles'
```

The response displays all files currently on the Disney SFTP environment, providing operators with necessary visibility to determine which files are available for retrieval or processing.

### Related Operations

This endpoint is typically used in conjunction with other reconciliation endpoints:

- **File Retrieval**: The `retrieveReconciliation` endpoint (`/retrieveReconciliation?fileName={nameOfFile}`) is used to retrieve specific files listed by this endpoint
- **Manual Processing**: Operators can manually run the reconciliation process via the `execute` endpoint (`/execute`)

### System Integration

Files retrieved through this endpoint are typically stored in the Kroger SFTP `/disneyTempData` directory (IP: 10.32.10.252, username: ECADMIN) for subsequent processing by reconciliation batch jobs.

## Related Concepts

- `external_system.disney-monthly-recon-prod-kpsazc-dgtl-kroger-com` — The external system exposing this API route
- `job.domino-monthly-reconciliation-batch` — The batch job that uses this endpoint to discover available reconciliation files

## Sources

- Confluence page: [Domino Batch Jobs](https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs) (Space: BD, Page ID: 96895502)
