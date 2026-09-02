---
type: Architecture
title: Disney Redemption File Upload Automation Architecture
description: This architecture describes an automated approach to manage Disney redemption
  file uploads from the Kroger Offers Team to Disney's external server. The solution
  eliminates manual coordination requirements by implementing a dedicated VM user
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/1247871291/Disney+Redemption+File+Upload+-+Automation+Approach
tags:
- okf
- architecture
okf_schema: okf.concept.v1
identity:
  canonical_id: architecture.disney-redemption-file-automation
  concept_type: architecture
  display_name: Disney Redemption File Upload Automation Architecture
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:570244aed22cd0e5b0df4274685f53303a700b2c8f816e1a2eb97b74538c9cb5
  last_updated_at: '2026-09-02T21:47:02.697844Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '1247871291'
    page_title: Disney Redemption File Upload - Automation Approach
    page_version: 13
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/1247871291/Disney+Redemption+File+Upload+-+Automation+Approach
    role: primary
relationships:
- type: describes
  target_canonical_id: job.disney-redemption-cron-job
- type: enforces
  target_canonical_id: security_control.control-disney-redemption-vm-user-restriction
- type: processes
  target_canonical_id: contract.disney-redemption-csv-format
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:47:02.697844Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: failed
---

# Disney Redemption File Upload Automation Architecture

## Summary

This architecture describes an automated approach to manage Disney redemption file uploads from the Kroger Offers Team to Disney's external server. The solution eliminates manual coordination requirements by implementing a dedicated VM user account with a scheduled cron job that monitors, validates, and uploads files automatically.

## Details

### Executive Overview

The Kroger Offers Team generates Disney redemption files that require uploading to Disney's external server. Previously, this required manual coordination with the Domino Team for every file transfer, creating dependencies and operational delays. The new architecture automates this process while maintaining security through restricted access controls and comprehensive validation.

### Current State Challenge

The Offers Team currently generates Disney redemption files and requires manual coordination with the Domino Team to upload these files to Disney's external server. The manual upload process requires Domino Team intervention for every file transfer, creating dependencies and delays.

### Proposed Solution

This approach provides the Offers Team with direct access to place files on the VM using a dedicated local user account (`disney-upload-user`). The Domino Team sets up an automated cron job that monitors the designated directory and automatically uploads files to Disney's external server on a scheduled basis.

### Process Flow

The automated workflow consists of eight sequential steps:

1. **File Generation**: Offers Team generates redemption file locally (`kroger-*-redeemed-codes.csv`)
2. **File Transfer**: Offers Team connects to VM using SFTP/SCP with provided credentials
3. **File Placement**: Files uploaded to designated directory (`/disneyData/incoming/`)
4. **Scheduled Execution**: Cron job runs on agreed schedule based on file generation frequency
5. **Automated Upload**: Script detects new files and uploads to Disney server via API
6. **Verification**: Script validates successful upload using Disney list directory API
7. **Notification**: Dynatrace log alert notifies teams of job completion or failure
8. **Cleanup**: Successfully uploaded files moved to processed directory

### Cron Job Implementation

The cron job monitors the `/disneyData/incoming/` directory for new files matching the pattern `kroger-*-redeemed-codes.csv`. The schedule is determined based on the file generation frequency provided by the Offers Team.

### Script Functionality

The automated upload script performs the following operations:

- Monitor `/disneyData/incoming/` for new files matching pattern `kroger-*-redeemed-codes.csv`
- Validate file format and contents according to the expected contract format
- Upload files to Disney server via API
- Verify successful upload using Disney list directory API
- Move successfully uploaded files to `/disneyData/processed/`
- Move failed uploads to `/disneyData/failed/`
- Generate detailed logs for Dynatrace monitoring
- Prevent duplicate uploads using checksum tracking

### Team Responsibilities

#### Offers Team

- Generate redemption files as per schedule
- Connect to VM using provided credentials (SFTP/SCP client)
- Upload files to designated directory: `/disneyData/input/`
- Provide file generation frequency/schedule to Domino Team
- Monitor Dynatrace alerts for upload status

#### Domino Team

- Create restricted local user account on VM (`disney-upload-user`)
- Configure directory with appropriate permissions (`/disneyData/incoming/`)
- Share VM connection details and credentials with Offers Team securely
- Share designated directory path for file placement
- Implement cron job based on Offers Team's file generation frequency
- Set up automated upload script with error handling
- Configure Dynatrace log alerts for job completion/failure

### Security Controls

The architecture implements multiple security layers to enforce restricted user access:

| Security Layer | Implementation Details |
|---|---|
| Dedicated Local User | Create local VM user account specifically for Offers Team file uploads. Account name: `disney-upload-user` |
| Limited Shell Access | User configured for file transfer operations only. Shell: `/bin/bash` with restricted permissions |
| No Sudo Access | No elevated privileges or sudo permissions. Restricted to file transfer operations only |
| Directory Restrictions | File access limited to: `/disneyData/incoming/`. Read/Write permissions: ONLY in designated directory. All other directories: NO ACCESS |

### Assumptions

- Redemption file format is consistent
- Offers Team can provide file generation frequency upfront
- Offers Team has access to SFTP/SCP client software
- File naming convention remains consistent: `kroger-*-redeemed-codes.csv`

### Contact Information

- Domino Team Support: Unknown
- Emergency Contact: Unknown
- Documentation: Unknown
- Document Version: 3.0
- Last Updated: 2026-05-26
- Prepared By: Domino Team
- Target Audience: Offers Team & Domino Team

## Related Concepts

- **job.disney-redemption-cron-job** — Describes the scheduled execution component that performs automated file uploads
- **contract.disney-redemption-csv-format** — Defines the file format that the architecture processes
- **security_control.disney-redemption-vm-user-restriction** — Security controls enforced by this architecture to restrict user access

## Sources

Confluence: Disney Redemption File Upload - Automation Approach (Page ID: 1247871291, Space: BD, Version: 13)
https://kroger.atlassian.net/wiki/spaces/BD/pages/1247871291/Disney+Redemption+File+Upload+-+Automation+Approach
