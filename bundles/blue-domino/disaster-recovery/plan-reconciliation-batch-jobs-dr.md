---
type: Disaster Recovery Plan
title: Reconciliation Batch Jobs Cloud Disaster Recovery Plan
description: This Cloud Disaster Recovery Plan provides a structured approach for
  recovering the PartnerFulfillmentMonthlyReconciliationBatchJob and PartnerFulfillmentDailyReconciliationBatchJob
  after a disruption. These batch jobs are designed to recon
resource: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895386/ReconciliationBatchJob+Monthly+and+Daily
tags:
- okf
- disaster_recovery_plan
okf_schema: okf.concept.v1
identity:
  canonical_id: disaster_recovery_plan.plan-reconciliation-batch-jobs-dr
  concept_type: disaster_recovery_plan
  display_name: Reconciliation Batch Jobs Cloud Disaster Recovery Plan
  domain: '96894978'
  lifecycle_status: unknown
version:
  content_version: 1
  source_fingerprint: sha256:f56ed336e304c537a0901a3624759ab81694d3e98d87e0667c229f3cf11e9935
  last_updated_at: '2026-09-02T21:54:30.711690Z'
aliases: []
provenance:
  source_documents:
  - platform: confluence
    space_key: BD
    page_id: '96895386'
    page_title: ReconciliationBatchJob(Monthly and Daily)
    page_version: 7
    url: https://kroger.atlassian.net/wiki/spaces/BD/pages/96895386/ReconciliationBatchJob+Monthly+and+Daily
    role: primary
relationships:
- type: applies_to
  target_canonical_id: job.partnerfulfillmentdailyreconciliationbatchjob
- type: applies_to
  target_canonical_id: job.partnerfulfillmentmonthlyreconciliationbatchjob
- type: owned_by
  target_canonical_id: team.resiliency-recovery
generation:
  generator: okf-confluence
  generator_version: 0.1.0
  llm_provider: claude-code
  generated_at: '2026-09-02T21:54:30.711690Z'
quality:
  standardization: passed
  coherence: failed
  comprehensiveness: passed
---

# Reconciliation Batch Jobs Cloud Disaster Recovery Plan

## Summary

This Cloud Disaster Recovery Plan provides a structured approach for recovering the PartnerFulfillmentMonthlyReconciliationBatchJob and PartnerFulfillmentDailyReconciliationBatchJob after a disruption. These batch jobs are designed to reconcile data without affecting real-time processes, making delayed execution acceptable during disaster scenarios. The plan covers infrastructure deployed in Kroger Company cloud environments (IaaS and PaaS), excluding SaaS offerings managed by external vendors.

## Details

### Purpose

The purpose of this plan is to provide a structured approach for recovering data and applications hosted in cloud environments after a disruption, ensuring business continuity for the Kroger Company.

### Scope

This plan covers:
- job.partnerfulfillmentdailyreconciliationbatchjob
- job.partnerfulfillmentmonthlyreconciliationbatchjob

These jobs are utilized by the Kroger Company including Infrastructure as a Service (IaaS) and Platform as a Service (PaaS). This plan does not cover Software as a Service (SaaS) offerings as they are the responsibility of the vendor.

### Objectives

- Minimize disruption to cloud-hosted services
- Ensure timely recovery of data and applications
- Maintain business continuity and operational resilience

### Resiliency Recovery Procedures

- Reconciliation batch jobs are not deployed to the east region
- During central region outage, cancellation requests will be delayed and re-run once the central region is functional again
- Delayed execution does not have significant impact on overall operations, as these jobs are designed to reconcile data without affecting real-time processes

### Recovery Actions

Upon recovery of the central region:
- Re-run the jobs once the central region is functional

### Team Structure

| Name | Role | Contact |
|------|------|---------| 
| Sunil P Bapat | IT Application Owner | sunil.bapat@kroger.com |
| Heather Alvey | Business Owner | heather.alvey@kroger.com |
| Venketa S Penmetsa / Santosh Boppidi / Goutam Kundu | Support Team/Escalation Point | venkata.penmetsa@kroger.com, santoshreddy.boppidi@kroger.com, goutam.kundu@kroger.com |

### Communication Plan

**Employees:**
- Team Email: EnterpriseCustomer-BlueArmy@kroger.com
- Infra Group: APP-DIG-SVC-Partner-Fulfillment

**Management Team:**
- Sunil P Bapat: sunil.bapat@kroger.com
- Nathan Subler: nathan.subler@kroger.com

### Asset Identification

- APM Number: APM0004876

## Related Concepts

- job.partnerfulfillmentdailyreconciliationbatchjob
- job.partnerfulfillmentmonthlyreconciliationbatchjob
- team.resiliency-recovery

## Sources

- Confluence page 96895386: ReconciliationBatchJob (Monthly and Daily)
