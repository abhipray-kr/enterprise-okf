---
type: Security Control
title: Disney Redemption File Upload VM User Restrictions
description: The Disney Redemption File Upload VM User Restrictions is a security
  control that establishes limited access and operational constraints for the `disney-upload-user`
  local user account on the file upload virtual machine. This control ensure
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/1247871291/Disney+Redemption+File+Upload+-+Automation+Approach
tags:
- okf
- security_control
okf_schema: okf.concept.v1
identity:
  canonical_id: security_control.control-disney-redemption-vm-user-restriction
  concept_type: security_control
  display_name: Disney Redemption File Upload VM User Restrictions
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:5bd0a21b5d392d7ffd442b6cdc94f96d15297855e39a3012c458b7a218b2e25e
  last_updated_at: '2026-09-02T21:59:38.512687Z'
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
- type: applies_to
  target_canonical_id: job.disney-redemption-cron-job
- type: part_of
  target_canonical_id: architecture.disney-redemption-file-automation
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:59:38.512687Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Disney Redemption File Upload VM User Restrictions

## Summary

The Disney Redemption File Upload VM User Restrictions is a security control that establishes limited access and operational constraints for the `disney-upload-user` local user account on the file upload virtual machine. This control ensures that the Offers Team can place redemption files on the VM while preventing unauthorized access to system resources or other directories. The control is implemented through a combination of dedicated account creation, restricted shell access, disabled sudo privileges, and directory-level file access limitations.

## Details

### Account Creation and Identification

A dedicated local user account named `disney-upload-user` is created specifically for Offers Team file upload operations. This dedicated account approach isolates file transfer activities from other system operations and enables fine-grained access control.

### Shell Access Restrictions

The user account is configured with limited shell access using `/bin/bash` with restricted permissions. The account is configured for file transfer operations only, preventing interactive shell access beyond what is necessary for SFTP/SCP-based file uploads.

### Privilege Restrictions

The `disney-upload-user` account has no elevated privileges and no sudo access. All operations are restricted to file transfer operations only, preventing users from executing administrative commands or accessing system-level functionality.

### Directory Access Restrictions

File access is limited exclusively to the designated directory `/disneyData/incoming/`. The account has read/write permissions only within this specified directory. All other directories on the system are completely inaccessible to this user account.

### Implementation Context

This control is implemented as part of the Disney Redemption File Upload automation approach, where the Offers Team generates redemption files locally and uploads them to the VM. The cron job process responsible for further automation operates with its own permissions and interacts with the files placed in the restricted directory by this user.

## Related Concepts

- `job.disney-redemption-cron-job` — the cron job that processes files uploaded by this restricted user
- `architecture.disney-redemption-file-automation` — the broader automation architecture that incorporates this security control

## Sources

- Confluence page 1247871291 (Space: BD): "Disney Redemption File Upload - Automation Approach" — Primary source documenting the complete specification of user account creation, shell restrictions, and directory access controls
