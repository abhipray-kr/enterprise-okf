---
type: Job
title: Domino Monthly Reconciliation Batch Job
description: The Domino Monthly Reconciliation Batch Job is an automated process that
  generates monthly reconciliation files based on production data and exchanges them
  with the Disney system. The job creates files, sends them to Disney's SFTP environme
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- job
okf_schema: okf.concept.v1
identity:
  canonical_id: job.domino-monthly-reconciliation-batch
  concept_type: job
  display_name: Domino Monthly Reconciliation Batch Job
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:b524313f8ecb012a8f05806bdc3489d571a0af4941df2811c395065705dca2c7
  last_updated_at: '2026-09-02T21:57:29.811901Z'
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
  target_canonical_id: api_route.disney-monthly-execute-reconciliation
- type: uses
  target_canonical_id: api_route.disney-monthly-list-files
- type: uses
  target_canonical_id: api_route.disney-monthly-retrieve-reconciliation
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:57:29.811901Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Domino Monthly Reconciliation Batch Job

## Summary

The Domino Monthly Reconciliation Batch Job is an automated process that generates monthly reconciliation files based on production data and exchanges them with the Disney system. The job creates files, sends them to Disney's SFTP environment, and retrieves reconciliation diff files that identify discrepancies between Kroger's and Disney's systems.

## Details

### Execution

The monthly reconciliation batch job can be executed manually by running:

```
curl --location 'https://disney-monthly-recon-prod.kpsazc.dgtl.kroger.com/execute'
```

This command creates a file based on the current timestamp and sends it to Disney's SFTP environment. The job processes production data flowing through the domain service to generate the reconciliation file.

### File Retrieval

Once the monthly batch job completes, Disney sends a diff file containing the differences between their system and Kroger's system.

Available files on Disney's system can be listed using:

```
curl --location 'https://disney-monthly-recon-prod.kpsazc.dgtl.kroger.com/listFiles'
```

To retrieve a specific reconciliation file from the SFTP location:

```
curl --location 'https://disney-monthly-recon-prod.kpsazc.dgtl.kroger.com/retrieveReconciliation?fileName={nameOfFile}'
```

### SFTP Configuration

Retrieved files are sent to the Kroger SFTP environment at:
- **IP Address:** 10.32.10.252
- **Directory:** /disneyTempData
- **Username:** ECADMIN

### Deployment History

| Date | Status | Notes |
|------|--------|-------|
| 10-7-2024 | Initial Release | Monthly Batch Job released |
| 10-31-2024 | Cron Enabled | Cron schedule enabled for monthly reconciliation |
| 11-13-2024 | Enhancement | Added logic to update redeemed dates in offers and updated reconciliation job to use Audit endpoint |

## Related Concepts

- api_route.disney-monthly-execute-reconciliation
- api_route.disney-monthly-list-files
- api_route.disney-monthly-retrieve-reconciliation
- external_system.disney-monthly-recon-prod-kpsazc-dgtl-kroger-com

## Sources

- Confluence: [Domino Batch Jobs](https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs) (Page 96895502, Space BD)
