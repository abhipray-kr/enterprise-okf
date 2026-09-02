---
type: Job
title: Domino Nightly DAF Responder Batch Job
description: The Domino Nightly DAF Responder Batch Job is a scheduled batch process
  that automatically executes daily at 6am EST in production. It processes DAF (Disney
  Affiliate File) responder data files that are marked with the previous day's date
  a
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- job
okf_schema: okf.concept.v1
identity:
  canonical_id: job.domino-nightly-daf-responder-batch
  concept_type: job
  display_name: Domino Nightly DAF Responder Batch Job
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:64299ca7ca8684569734879d8231468e0f8432edbe1624e6e10c4c021be3dc20
  last_updated_at: '2026-09-02T21:57:48.213414Z'
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
- type: can_invoke
  target_canonical_id: api_route.disney-daily-execute-specific-file
- type: invokes
  target_canonical_id: api_route.disney-daily-execute-previous-day
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:57:48.213414Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Domino Nightly DAF Responder Batch Job

## Summary

The Domino Nightly DAF Responder Batch Job is a scheduled batch process that automatically executes daily at 6am EST in production. It processes DAF (Disney Affiliate File) responder data files that are marked with the previous day's date and interacts with the Disney Daily Recon Central system to handle reconciliation and responder file processing.

## Details

### Schedule and Execution

The job is configured with an environment-based cron that runs daily at 6am EST in the production environment. The nightly batch job code is deployed and managed as a containerized service.

### File Processing

The job processes CSV files with the naming convention `kroger_daf_responder_YYYYMMDDHHMMSS_UTC.csv`. These files are staged in an SFTP directory, and the job automatically processes the file marked with the previous day's date without requiring manual intervention.

### Manual Invocation

The job can be manually triggered in two ways:

**Process Previous Day's File:**
Execute a curl request to process the file automatically selected for the previous day:
```
curl --location 'https://disney-daily-recon-central-prod.kpsazc.dgtl.kroger.com/execute'
```

**Process Specific File:**
Execute a curl request with a specific file name parameter to rerun a particular file:
```
curl --location 'https://disney-daily-recon-central-prod.kpsazc.dgtl.kroger.com/execute?fileName=kroger_daf_responder_20241003000000_UTC.csv'
```

### Observability

Logs for the nightly batch execution can be viewed in DataDog. Application performance and execution metrics are available in Dynatrace for monitoring and troubleshooting.

### Notable Changes

The job received a critical fix on 10-10-2024 to resolve a filename override issue where manually specified filenames would override the automatic nightly job execution. On 11-13-2024, logic was added to update the redeemed date in offers during processing.

## Related Concepts

- external_system.disney-daily-recon-central-prod-kpsazc-dgtl-kroger-com
- api_route.disney-daily-execute-previous-day
- api_route.disney-daily-execute-specific-file

## Sources

- [Domino Batch Jobs](https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs) (page 96895502)
