---
type: Runbook
title: Disney Redemption File Upload Automation Setup and Operations
description: 'This runbook provides the operational procedures for automating the
  upload of Disney redemption files from the Kroger Offers Team to Disney''s external
  server. The solution eliminates manual coordination overhead by establishing a dedicated '
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/1247871291/Disney+Redemption+File+Upload+-+Automation+Approach
tags:
- okf
- runbook
okf_schema: okf.concept.v1
identity:
  canonical_id: runbook.disney-redemption-automation-setup
  concept_type: runbook
  display_name: Disney Redemption File Upload Automation Setup and Operations
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:80ab39c643184eba39ff243c8d5b63cbdbe95825c7c0dccdfbb3bd710e4e7598
  last_updated_at: '2026-09-02T21:58:57.429532Z'
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
- type: implements
  target_canonical_id: security_control.control-disney-redemption-vm-user-restriction
- type: part_of
  target_canonical_id: architecture.disney-redemption-file-automation
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:58:57.429532Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Disney Redemption File Upload Automation Setup and Operations

## Summary

This runbook provides the operational procedures for automating the upload of Disney redemption files from the Kroger Offers Team to Disney's external server. The solution eliminates manual coordination overhead by establishing a dedicated local user account on a VM with a scheduled automated process that detects, validates, and uploads redemption files on a defined schedule.

## Details

### Overview

The Offers Team generates Disney redemption files (named `kroger-*-redeemed-codes.csv`) that require uploading to Disney's external server. Previously, this required manual intervention and coordination with the Domino Team for every file transfer, creating dependencies and delays. This automated approach streamlines the process by providing the Offers Team with direct access to place files on a designated VM directory, where a Domino Team-maintained cron job monitors for new files and automatically uploads them to Disney's server on a defined schedule.

### Process Flow

1. **File Generation**: Offers Team generates redemption file locally following the naming convention `kroger-*-redeemed-codes.csv`
2. **File Transfer**: Offers Team connects to the VM using SFTP/SCP with provided credentials
3. **File Placement**: Files are uploaded to the designated directory (`/disneyData/incoming/`)
4. **Scheduled Execution**: Cron job runs on an agreed schedule based on file generation frequency
5. **Automated Upload**: Script detects new files and uploads to Disney server via API
6. **Verification**: Script validates successful upload using Disney list directory API
7. **Notification**: Dynatrace log alert notifies teams of job completion or failure
8. **Cleanup**: Successfully uploaded files are moved to the processed directory (`/disneyData/processed/`); failed uploads go to `/disneyData/failed/`

### Offers Team Responsibilities

- Generate redemption files according to the established schedule
- Connect to VM using provided SFTP/SCP credentials
- Upload files to the designated directory `/disneyData/input/`
- Provide file generation frequency and schedule to Domino Team
- Monitor Dynatrace alerts for upload status and notifications

### Domino Team Responsibilities

- Create a restricted local user account on the VM named `disney-upload-user`
- Configure the file directory with appropriate permissions (`/disneyData/incoming/`)
- Securely share VM connection details and credentials with the Offers Team
- Implement the cron job on a schedule based on Offers Team file generation frequency
- Set up the automated upload script with comprehensive error handling
- Configure Dynatrace log alerts for job completion and failure notification

### Script Functionality

The automated upload script performs the following operations:

- Monitor `/disneyData/incoming/` for new files matching the pattern `kroger-*-redeemed-codes.csv`
- Validate file format and contents before processing
- Upload files to Disney server via API
- Verify successful upload using Disney list directory API
- Move successfully uploaded files to `/disneyData/processed/`
- Move failed uploads to `/disneyData/failed/` for investigation
- Generate detailed logs for Dynatrace monitoring and alerting
- Implement checksum tracking to prevent duplicate uploads

### Security Controls

This solution implements security_control.disney-redemption-vm-user-restriction through the following measures:

- **Dedicated Local User**: A restricted local VM user account (`disney-upload-user`) is created specifically for Offers Team file uploads
- **Limited Shell Access**: User is configured for file transfer operations only, with `/bin/bash` shell and restricted permissions
- **No Elevated Privileges**: No sudo access or elevated privileges are granted; the account is restricted to file transfer operations only
- **Directory Restrictions**: File access is limited to `/disneyData/incoming/` with read/write permissions granted only in the designated directory and denied for all other directories

### Schedule Configuration

The cron job schedule will be determined and configured based on the file generation frequency provided by the Offers Team. This ensures the automated process runs at appropriate intervals to capture and process newly generated files.

### Assumptions

- Redemption file format remains consistent
- Offers Team can provide file generation frequency upfront
- Offers Team has access to SFTP/SCP client software
- File naming convention remains consistent: `kroger-*-redeemed-codes.csv`

### Contact Information

- **Domino Team Support**: [Contact details to be configured]
- **Emergency Contact**: [On-call rotation to be configured]
- **Documentation**: [Link to detailed runbook to be added]

## Related Concepts

- **security_control.disney-redemption-vm-user-restriction** — The security controls and restrictions implemented through this runbook
- **architecture.disney-redemption-file-automation** — The broader architectural approach for Disney redemption file automation

## Sources

- Confluence page [1247871291](https://kroger.atlassian.net/wiki/spaces/BD/pages/1247871291/Disney+Redemption+File+Upload+-+Automation+Approach): "Disney Redemption File Upload - Automation Approach"
