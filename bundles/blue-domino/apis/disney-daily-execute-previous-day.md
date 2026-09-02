---
type: API Endpoint
title: Disney Daily Reconciliation Execute Previous Day
description: The `/execute` endpoint on the Disney Daily Reconciliation Central system
  processes the reconciliation file marked with the previous day's date. This endpoint
  is part of the nightly batch reconciliation process and can be called manually vi
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- api_route
okf_schema: okf.concept.v1
identity:
  canonical_id: api_route.disney-daily-execute-previous-day
  concept_type: api_route
  display_name: Disney Daily Reconciliation Execute Previous Day
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:5aafb95de13440810b4635266a7cad31c1f213da3e0e7df183c13a267f74abfd
  last_updated_at: '2026-09-02T21:43:51.829954Z'
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
  generated_at: '2026-09-02T21:43:51.829954Z'
quality:
  standardization: passed
  coherence: passed
  comprehensiveness: passed
---

# Disney Daily Reconciliation Execute Previous Day

## Summary

The `/execute` endpoint on the Disney Daily Reconciliation Central system processes the reconciliation file marked with the previous day's date. This endpoint is part of the nightly batch reconciliation process and can be called manually via HTTP GET to trigger processing of the most recent available file without specifying a file name.

## Details

### Endpoint Information

**URL:** `https://disney-daily-recon-central-prod.kpsazc.dgtl.kroger.com/execute`

**Method:** GET

**Purpose:** Processes the reconciliation file for the previous day in the SFTP directory. The file processed will have a name following the pattern `kroger_daf_responder_YYYYMMDDHHMMSS_UTC.csv`, where the date represents the previous day.

### Usage

The endpoint can be called in two modes:

1. **Standard execution (previous day):** Calling the endpoint without parameters will automatically process the file marked with the previous day's date.

2. **Specific file execution:** An optional `fileName` query parameter can be provided to process a specific file: `https://disney-daily-recon-central-prod.kpsazc.dgtl.kroger.com/execute?fileName=kroger_daf_responder_20241003000000_UTC.csv`

### Operational Context

The endpoint is primarily invoked by the nightly batch reconciliation job, which runs daily at 6 AM EST in production via an environment-based cron schedule. Manual execution of this endpoint should be performed with caution as this is a production URL. Execution logs are available in DataDog and performance metrics can be observed in Dynatrace.

### Historical Deployments

This endpoint was initially released on 9-24-2024 as part of the Nightly Batch Job. Subsequent updates include:
- 10-10-2024: Fix for filename issue where manually specified filenames would override the nightly job
- 11-13-2024: Addition of logic to update redeemed date in offers

## Related Concepts

- `external_system.disney-daily-recon-central-prod-kpsazc-dgtl-kroger-com` — The external system that exposes this endpoint
- `job.domino-nightly-daf-responder-batch` — The batch job that uses this endpoint for nightly execution

## Sources

- Confluence page 96895502: [Domino Batch Jobs](https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs)
