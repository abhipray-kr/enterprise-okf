---
type: Job
title: Domino Monthly Hulu Responder Batch Job
description: A scheduled batch job that processes monthly Hulu responder files through
  the Disney monthly reconciliation system. The job retrieves responder files from
  the Disney file system, executes them through the reconciliation process, and handles
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- job
okf_schema: okf.concept.v1
identity:
  canonical_id: job.domino-monthly-hulu-responder-batch
  concept_type: job
  display_name: Domino Monthly Hulu Responder Batch Job
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:3f70c79656e923449e95c487cf2518aa585d86b4ab08a046d74dcdb31e9cb944
  last_updated_at: '2026-09-02T21:57:09.814326Z'
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
- type: invokes
  target_canonical_id: api_route.disney-monthly-execute-responder
- type: retrieves_from
  target_canonical_id: api_route.disney-monthly-retrieve-hulu-responder
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:57:09.814326Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Domino Monthly Hulu Responder Batch Job

## Summary

A scheduled batch job that processes monthly Hulu responder files through the Disney monthly reconciliation system. The job retrieves responder files from the Disney file system, executes them through the reconciliation process, and handles the resulting reconciliation data.

## Details

### Functionality

The monthly Hulu responder batch job processes reconciliation files between Kroger and Disney systems. Responder files are stored in the `ResponderFile` directory on the Disney monthly recon central system and must be executed to process the monthly reconciliation data.

### File Processing

The job handles the following operations:

**Execution**: Run a responder file against the external_system.disney-monthly-recon-central-prod-kpsazc-dgtl-kroger-com system using the endpoint pattern `executeResponder?fileName={nameOfFile}`.

**File Retrieval**: Retrieve Hulu responder files from the Disney file system and place them in the `disneyTempData` folder on the Kroger SFTP environment (IP: 10.32.10.252, username: ECADMIN) using the endpoint pattern `retrieveHuluResponder?fileName={nameOfFile}`.

**Directory Listing**: View available files in the monthly directory structure using the `getDirectoryStructure` endpoint to identify responder files ready for processing.

**Diff File Management**: After execution, Disney returns reconciliation diff files containing differences between systems. These files are also retrieved from the Disney SFTP environment for review and reconciliation.

### Manual Execution

The job can be triggered manually through curl requests to the Disney monthly recon central system. File names typically follow the pattern `kroger_{datestring}.csv` for responder files.

### Infrastructure

- External system: external_system.disney-monthly-recon-central-prod-kpsazc-dgtl-kroger-com
- Processing method: Environment-based cron schedule
- File storage: Disney monthly SFTP, Kroger SFTP (`/disneyTempData` directory)
- Monitoring: Available in Datadog and Dynatrace

### Deployment History

Initial release: 10-7-2024

Recent updates include logic to update redeemed dates in offers and integration with audit endpoints for reconciliation (11-13-2024).

## Related Concepts

- api_route.disney-monthly-execute-responder
- api_route.disney-monthly-retrieve-hulu-responder
- external_system.disney-monthly-recon-central-prod-kpsazc-dgtl-kroger-com

## Sources

- Domino Batch Jobs (BD space, page 96895502)
