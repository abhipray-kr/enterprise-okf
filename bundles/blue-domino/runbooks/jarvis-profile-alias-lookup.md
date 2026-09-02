---
type: Runbook
title: Find Profile Alias and Customer ID Using Jarvis
description: This runbook provides the steps to look up a customer's Profile Alias
  (Personalization ID) and associated User ID using the Jarvis tool. The procedure
  begins with searching for profiles by card number or loyalty ID, then retrieves
  the Profi
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
tags:
- okf
- runbook
okf_schema: okf.concept.v1
identity:
  canonical_id: runbook.jarvis-profile-alias-lookup
  concept_type: runbook
  display_name: Find Profile Alias and Customer ID Using Jarvis
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:b0b8036cfa271a3649160ae731a01e5a45f17074e5fc0dab1914b4bcd5420d47
  last_updated_at: '2026-09-02T21:59:24.231367Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895288'
    page_title: Support Dashboard And Monotoring
    page_version: 11
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895288/Support+Dashboard+And+Monotoring
    role: primary
relationships:
- type: uses_tool
  target_canonical_id: external_system.jarvis
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:59:24.231367Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Find Profile Alias and Customer ID Using Jarvis

## Summary

This runbook provides the steps to look up a customer's Profile Alias (Personalization ID) and associated User ID using the Jarvis tool. The procedure begins with searching for profiles by card number or loyalty ID, then retrieves the Profile Alias by User ID and partner name.

## Details

### Prerequisites

- Access to Jarvis. If you do not have access, raise a request for the profile `jcs000-jarvis-dev-user`.

### Step-by-Step Procedure

**Step 1: Access Jarvis**
1. Navigate to Jarvis
2. Log in with your credentials

**Step 2: Find User ID by Card Value**
1. In the "What to run" dropdown, select **Profiles by Card Value**
2. Enter the card number or loyalty ID in the text area
3. Click **Submit**
4. From the result table, copy the **User ID** value

**Step 3: Retrieve Profile Alias by User ID**
1. In the "What to run" dropdown, select **Profile Alias by User ID**
2. Paste the User ID copied in Step 2 into the text area
3. Enter the partner name on the next line in the same text area
4. Click **Submit**

**Step 4: Extract the Personalization ID**
- From the results, retrieve the **Personalization ID**, which is the **Profile Alias ID**

### Notes

- The Profile Alias ID is returned as the Personalization ID in the Jarvis results
- Both card number and loyalty ID can be used as search parameters in Step 2
- Partner name is required in Step 3 to retrieve the correct Profile Alias

## Related Concepts

- external_system.jarvis — The tool used to query customer profiles and aliases

## Sources

- Support Dashboard And Monitoring (Confluence page 96895288) — Steps to Find Profile Alias/Customer ID for a Given Card Number/Loyalty ID
