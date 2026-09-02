---
type: Deployment Process
title: Domino Batch Jobs Deployment and Release Process
description: The Domino Batch Jobs Deployment and Release Process manages the deployment,
  scheduling, and operational execution of batch jobs that handle Disney reconciliation
  and responder file processing. The process includes automated cron-based exec
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895502/Domino+Batch+Jobs
tags:
- okf
- deployment_process
okf_schema: okf.concept.v1
identity:
  canonical_id: deployment_process.process-domino-batch-jobs-release
  concept_type: deployment_process
  display_name: Domino Batch Jobs Deployment and Release Process
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:30e61de8b090b8ccf7ae55bc731d61239fa906030ca7937ad11dea4385c15286
  last_updated_at: '2026-09-02T21:51:24.317667Z'
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
- type: releases_to_production
  target_canonical_id: job.domino-monthly-reconciliation-batch
- type: releases_to_production
  target_canonical_id: job.domino-nightly-daf-responder-batch
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:51:24.317667Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Domino Batch Jobs Deployment and Release Process

## Summary

The Domino Batch Jobs Deployment and Release Process manages the deployment, scheduling, and operational execution of batch jobs that handle Disney reconciliation and responder file processing. The process includes automated cron-based execution, manual execution capabilities via curl endpoints, and file management across SFTP environments.

## Details

### Batch Jobs in Production

The deployment process releases and maintains three primary batch jobs:

- **job.domino-monthly-reconciliation-batch**: Monthly reconciliation job that generates reconciliation files based on production data flowing through the domain service
- **job.domino-nightly-daf-responder-batch**: Nightly batch job executing daily at 6am EST via environment-based cron, processing DAF responder files  
- **job.domino-monthly-hulu-responder-batch**: Monthly batch job processing Hulu responder files from Disney systems

### Automated Execution

Jobs are triggered via environment-based cron schedules:
- **Nightly DAF Responder Job**: Scheduled to run daily at 6am EST in production
- **Monthly Jobs**: Execute on their defined schedules for monthly reconciliation and responder processing

### Manual Execution

The process supports manual execution using curl commands:

- **Nightly DAF Responder**: Execute manual processing for a specific historical file by date
- **Monthly Reconciliation**: Manually trigger file generation based on current production data
- **Monthly Hulu Responder**: Manually execute responder file processing with specified filename parameter

### File Management and SFTP Operations

#### Kroger SFTP Environment
- **IP Address**: 10.32.10.252
- **Upload Directory**: /disneyTempData Directory
- **Access**: Restricted to authorized users

#### File Operations
- **List Available Files**: Query Disney SFTP directory structure to view available reconciliation and responder files
- **Retrieve Reconciliation Files**: Download diff files from Disney SFTP after monthly reconciliation completion
- **Retrieve Responder Files**: Fetch Hulu responder files from Disney systems
- **Execute File Processing**: Trigger processing of retrieved files on Kroger systems

### Monitoring and Observability

- **DataDog**: Logs available for tracking nightly batch job execution
- **Dynatrace**: Performance and execution data available for monitoring batch jobs

### Release Timeline

| Date | Application | Notes |
|------|-------------|-------|
| 9-24-2024 | Nightly Batch Job | Initial release |
| 10-7-2024 | Monthly Batch Job | Initial release |
| 10-10-2024 | Nightly Batch Job | Fix for filename issue when filename parameter override was affecting scheduled job |
| 10-31-2024 | Monthly Batch Job | Enable cron scheduling for monthly reconciliation |
| 11-13-2024 | Nightly Batch Job | Add logic to update redeemed date in offers |
| 11-13-2024 | Monthly Batch Job | Add logic to update redeemed date in offers; update reconciliation job to use Audit endpoint |

## Related Concepts

- job.domino-monthly-reconciliation-batch
- job.domino-nightly-daf-responder-batch
- job.domino-monthly-hulu-responder-batch

## Sources

Confluence: Domino Batch Jobs (Page ID: 96895502, Space: BD, Version: 5)
