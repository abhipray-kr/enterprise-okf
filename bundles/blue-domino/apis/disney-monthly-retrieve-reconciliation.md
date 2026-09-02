---
type: API Endpoint
title: Disney Monthly Reconciliation Retrieve File
description: API route that retrieves specific reconciliation diff files from the
  Disney monthly reconciliation system. This endpoint is part of the Disney monthly
  reconciliation integration and allows retrieving files that contain differences
  between K
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.disney-monthly-retrieve-reconciliation
  concept_type: api_route
  display_name: Disney Monthly Reconciliation Retrieve File
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:3a21efdfc12c54b37bb0f1784628ad40e44115aeb6d393bba110dec4c59ddc59
  last_updated_at: '2026-09-02T21:45:39.008655Z'
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
  generated_at: '2026-09-02T21:45:39.008655Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Disney Monthly Reconciliation Retrieve File

## Summary

API route that retrieves specific reconciliation diff files from the Disney monthly reconciliation system. This endpoint is part of the Disney monthly reconciliation integration and allows retrieving files that contain differences between Kroger's and Disney's systems after monthly batch processing.

## Details

### Endpoint Location

The retrieve reconciliation file endpoint is hosted on the Disney monthly reconciliation external system at `https://disney-monthly-recon-prod.kpsazc.dgtl.kroger.com/retrieveReconciliation`.

### Request Format

The endpoint accepts a query parameter to specify which file to retrieve:

```
GET /retrieveReconciliation?fileName={nameOfFile}
```

The `fileName` parameter should contain the name of the reconciliation file to retrieve from the Disney SFTP environment.

### Usage Context

Once the monthly batch job completes, Disney sends a diff file containing the differences between their system and Kroger's system. This API route enables retrieval of specific diff files from Disney's SFTP location.

### Related Operations

Before using this endpoint, you can list available files on the Disney monthly file system using the `/getDirectoryStructure` endpoint to identify which file names are available for retrieval.

Files retrieved through this endpoint are placed in the Kroger SFTP location at `/disneyTempData` directory (IP: 10.32.10.252, username: ECADMIN).

## Related Concepts

- `external_system.disney-monthly-recon-prod-kpsazc-dgtl-kroger-com` — The external system that exposes this API route
- `job.domino-monthly-reconciliation-batch` — The batch job that uses this API route to retrieve reconciliation results

## Sources

- Domino Batch Jobs (Confluence, page 96895502, space BD) — Documents the retrieval workflow for reconciliation diff files from Disney
