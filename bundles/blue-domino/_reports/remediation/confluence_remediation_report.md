# Confluence Remediation Report

Generated at: 2026-09-02T22:58:06.463044+00:00

## Proposed OKF Remediation Changes

| Change | Action | Evidence | Confidence | Risk |
| --- | --- | --- | --- | --- |
| deployment_process.createsecret | create_concept | createsecret.yaml']} | 0.65 | low |
| deployment_process.default | create_concept | heartbeat']} | 0.68 | low |
| deployment_process.digital-sync-cdc | create_concept | aks_cx_encust_01_stage_centralus_encust_stage_ecDigitalSyncCdc.yaml'], 'namespaces': ['<+infra.namespace>']} | 0.72 | low |
| deployment_process.disney-daily-reconciliation | create_concept | aks_cx_encust_01_stage_centralus_encust_stage_disneydailyreconciliationbatchjob.yaml']} | 0.68 | low |
| deployment_process.disney-daily-reconciliation-batch-job | create_concept | disneydailyreconciliationbatchjob.yaml']} | 0.65 | low |
| deployment_process.disney-monthly-reconciliation | create_concept | aks_cx_encust_01_stage_centralus_encust_stage_disneymonthlyreconciliationbatchjob.yaml']} | 0.68 | low |
| deployment_process.disney-monthly-reconciliation-batch-job | create_concept | disneymonthlyreconciliationbatchjob.yaml']} | 0.65 | low |
| deployment_process.ec | create_concept | commonOneStageInputSet.yaml']} | 0.62 | low |
| deployment_process.ec-cp-forward-sync | create_concept | eccpforwardsync.yaml']} | 0.65 | low |
| deployment_process.ec-cp-sync | create_concept | eccpeventconsumer.yml']} | 0.65 | low |
| deployment_process.getting-started-service | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=getting-started-service]', 'fact_count': 3} | medium | low |
| deployment_process.harness | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=.harness]', 'fact_count': 8} | medium | medium |
| deployment_process.harness-example | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=harness-example]', 'fact_count': 2} | medium | low |
| deployment_process.health-monitor | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=health-monitor]', 'fact_count': 6} | medium | low |
| deployment_process.loyalty-bulkload-sync | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=loyalty-bulkload-sync]', 'fact_count': 10} | medium | low |
| deployment_process.loyalty-sync | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=loyalty-sync]', 'fact_count': 10} | medium | low |
| deployment_process.node-harness-cli-gh-action | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=Node Harness CLI GH Action]', 'fact_count': 1} | medium | low |
| deployment_process.pa-cp-dlq-consumer-cron | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=pa-cp-dlq-consumer-cron]', 'fact_count': 1} | medium | low |
| deployment_process.pa-cp-event-consumer-cron | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=pa-cp-event-consumer-cron]', 'fact_count': 1} | medium | low |
| deployment_process.pa-cp-forward-sync | create_concept | deployment_ground_truth.json', 'pointer': 'facts[service_name=pa-cp-forward-sync]', 'fact_count': 1} | medium | low |
| deployment_process.personcore | create_concept | {'artifact': 'code_comprehension/deployment_ground_truth.json', 'pointer': 'facts[service_name=personCore]', 'source_files': ['.harness/cp_person_core_dev_test.yaml']} | Medium | Low - Creating new documentation concept with no breaking changes. Adds clarity to deployment topology. |
| deployment_process.preferences-core | create_concept | {'artifact': 'code_comprehension/deployment_ground_truth.json', 'pointer': 'facts[service_name=preferences-core]', 'supporting_fact_count': 12, 'source_files': ['.harness/Overrides/ServiceSpecific/preferences-core/aks_cx_encust_01_prod_centralus_encust_prod_preferencesCore.yaml', '.harness/Overrides/ServiceSpecific/preferences-core/aks_cx_encust_01_prod_eastus2_encust_prod_preferencesCore.yaml', '.harness/Overrides/ServiceSpecific/preferences-core/aks_cx_encust_01_stage_centralus_encust_stage_preferencesCore.yaml', '.harness/Overrides/ServiceSpecific/preferences-core/aks_cx_encust_01_stage_eastus2_encust_stage_preferencesCore.yaml', '.harness/Overrides/ServiceSpecific/preferences-core/aks_cx_shared_01_dev_centralus_encust_dev_preferencesCore.yaml', '.harness/Overrides/ServiceSpecific/preferences-core/aksencusttest_preferencesCore.yaml', 'ValueYAMLOverrides/preferences-core/devValuesOverride.yaml', 'ValueYAMLOverrides/preferences-core/preferencesCoreProdCentralOverridesValue.yaml']} | Medium | Low - Creates documentation for existing infrastructure. No impact on running systems. |
| deployment_process.servicespecific | create_concept | {'artifact': 'code_comprehension/deployment_ground_truth.json', 'pointer': 'facts[service_name=ServiceSpecific]', 'supporting_fact_count': 2, 'source_files': ['.harness/Overrides/ServiceSpecific/aks_cx_encust_01_stage_centralus_encust_stage_cpperformancetest.yaml', '.harness/Overrides/ServiceSpecific/aks_cx_encust_01_stage_eastus2_encust_stage_cpperformancetest.yaml']} | Medium | Low - Documents existing deployment infrastructure pattern. |
| deployment_process.shopper-profiles-core | create_concept | {'artifact': 'code_comprehension/deployment_ground_truth.json', 'pointer': 'facts[service_name=shopper-profiles-core]', 'supporting_fact_count': 11, 'source_files': ['.harness/Overrides/ServiceSpecific/shopper-profiles-core/aks_cx_encust_01_prod_centralus_encust_prod_shopperProfilesCore.yaml', '.harness/Overrides/ServiceSpecific/shopper-profiles-core/aks_cx_encust_01_prod_eastus2_encust_prod_shopperProfilesCore.yaml', '.harness/Overrides/ServiceSpecific/shopper-profiles-core/aks_cx_encust_01_stage_centralus_encust_stage_shopperProfilesCore.yaml', '.harness/Overrides/ServiceSpecific/shopper-profiles-core/aks_cx_encust_01_stage_eastus2_encust_stage_shopperProfilesCore.yaml', '.harness/Overrides/ServiceSpecific/shopper-profiles-core/aks_cx_shared_01_dev_centralus_encust_dev_shopperProfilesCore.yaml', '.harness/Overrides/ServiceSpecific/shopper-profiles-core/aksencusttest_shopperProfilesCore.yaml', 'ValueYAMLOverrides/shopper-profiles-core/shopperProfileCoreProdCentralOverridesValue.yaml', 'ValueYAMLOverrides/shopper-profiles-core/shopperProfileCoreProdEastOverrideValues.yaml']} | Medium | Low - Documents existing production infrastructure. |
| deployment_process.sync-harness-ng-token | create_concept | {'artifact': 'code_comprehension/deployment_ground_truth.json', 'pointer': 'facts[service_name=Sync Harness NG Token]', 'supporting_fact_count': 1, 'source_files': ['.github/workflows/sync_harness_token.yaml']} | Medium | Low - Documents existing CI/CD automation workflow. |
| deployment_process.update-configs-for-performance-test-core-services | create_concept | {'artifact': 'code_comprehension/deployment_ground_truth.json', 'pointer': 'facts[service_name=Update configs for performance test (core services)]', 'supporting_fact_count': 1, 'source_files': ['.github/workflows/update_configs_for_ec_performance_test.yaml']} | Medium | Low - Documents existing performance test automation. |
| deployment_process.valkyrie | create_concept | {'artifact': 'code_comprehension/deployment_ground_truth.json', 'pointer': 'facts[service_name=valkyrie]', 'supporting_fact_count': 6, 'source_files': ['.harness/Services/Caspian/valkyrie.yaml', 'ValueYAMLOverrides/valkyrie/templates/deployment.yaml', 'ValueYAMLOverrides/valkyrie/templates/ingress.yaml', 'ValueYAMLOverrides/valkyrie/templates/service.yaml', 'ValueYAMLOverrides/valkyrie/valuesoverrideValues.yaml', 'ValueYAMLOverrides/valkyrie/valuesoverridethanosValuesValkyrie.yaml']} | Medium | Low - Documents existing Kubernetes deployment. |
| repository.harness-ng-config-kps-encust | update_concept | {'artifact': 'code_comprehension/codebase_ast_map.json', 'pointer': 'cross_repo_dependencies[harness-ng-config-kps-encust->partner-benefit-subscriber]', 'supporting_edge_count': 1} | Medium | Low - Adds relationship documentation without modifying code or configuration. |
| repository.harness-ng-config-kps-encust | update_concept | {'artifact': 'code_comprehension/codebase_ast_map.json', 'pointer': 'cross_repo_dependencies[harness-ng-config-kps-encust->partner-fulfillment-orchestrator]', 'supporting_edge_count': 8} | Medium | Low - Adds relationship documentation. |
| repository.harness-ng-config-kps-encust | update_concept | {'artifact': 'code_comprehension/codebase_ast_map.json', 'pointer': 'cross_repo_dependencies[harness-ng-config-kps-encust->partner-fulfillment-tokenmanager]', 'supporting_edge_count': 6} | Medium | Low - Adds relationship documentation. |

## Batch Responses

- remediation-batch-0001: response captured
- remediation-batch-0002: response captured
- remediation-batch-0003: response captured
- remediation-batch-0004: response captured
- remediation-batch-0005: response captured
- remediation-batch-0006: response captured
- remediation-batch-0007: response captured
- remediation-batch-0008: response captured
- remediation-batch-0009: response captured
- remediation-batch-0010: response captured
- remediation-batch-0011: response captured
