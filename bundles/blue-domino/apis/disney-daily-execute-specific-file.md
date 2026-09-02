---
type: API Endpoint
title: Disney Daily Reconciliation Execute Specific File
description: 'API endpoint exposed by the Disney Daily Reconciliation system that
  enables manual execution of specific reconciliation files from the SFTP directory.
  This parameterized endpoint supports recovery scenarios and targeted reprocessing
  of DAF '
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.disney-daily-execute-specific-file
  concept_type: api_route
  display_name: Disney Daily Reconciliation Execute Specific File
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:87f72edb59a110351c539548573c2e62a294fc16a6fed911c77b8f438f1c07f7
  last_updated_at: '2026-09-02T21:44:11.146123Z'
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
  target_canonical_id: external_system.disney-daily-recon-central-prod-kpsazc-dgtl-kroger-com
- type: used_by
  target_canonical_id: job.domino-nightly-daf-responder-batch
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:44:11.146123Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Disney Daily Reconciliation Execute Specific File

## Summary

API endpoint exposed by the Disney Daily Reconciliation system that enables manual execution of specific reconciliation files from the SFTP directory. This parameterized endpoint supports recovery scenarios and targeted reprocessing of DAF responder files by specifying a file name as a query parameter.

## Details

### Endpoint

The `/execute` endpoint is available on the `external_system.disney-daily-recon-central-prod-kpsazc-dgtl-kroger-com` system and accepts an optional `fileName` query parameter.

**Base endpoint:**
```
https://disney-daily-recon-central-prod.kpsazc.dgtl.kroger.com/execute
```

### Parameterized Usage

When a specific file needs to be reprocessed, the endpoint can be invoked with a `fileName` parameter:

```
https://disney-daily-recon-central-prod.kpsazc.dgtl.kroger.com/execute?fileName=kroger_daf_responder_20241003000000_UTC.csv
```

### Purpose and Usage

This endpoint allows operators to:
- Manually trigger reconciliation for a previously processed file
- Reprocess specific dated reconciliation files from the SFTP directory
- Recover from processing failures for individual files without waiting for the scheduled nightly batch

Files in the SFTP directory follow the naming pattern: `kroger_daf_responder_{YYYYMMDDHHMMSS}_UTC.csv`

### Invocation

The endpoint is invoked via HTTP GET using curl or similar tools:

```
curl --location 'https://disney-daily-recon-central-prod.kpsazc.dgtl.kroger.com/execute?fileName=kroger_daf_responder_20241003000000_UTC.csv'
```

### Important Notes

**Security Warning:** This is a production endpoint. Care should be taken when invoking this curl command.

## Related Concepts

- **external_system.disney-daily-recon-central-prod-kpsazc-dgtl-kroger-com** — The system that exposes this endpoint
- **job.domino-nightly-daf-responder-batch** — The nightly batch job that uses this endpoint for reconciliation processing

## Sources

- Confluence page: "Domino Batch Jobs" (space BD, page ID 96895502)
  - Section: "Manually Running a Specific File"
  - Section: "Manually Running the Process"
