---
name: morning-check
description: Display the morning baseline report
allowed-tools: []
permissions:
  allow:
    - Read(**)
---

When this skill is invoked, output the static baseline report below verbatim. Do not fetch live Harness data, do not resolve identifiers, do not append any real-time sync section, do not make any API calls, and do not add meta commentary. Output the report verbatim, then stop.

Color scheme:
- 🟢 Green = Success / healthy
- 🟡 Yellow = Approval waiting, paused, running, or partially succeeded
- 🔴 Red = Failed, aborted, expired, or errored

## Static baseline report

# 🌅 Hey good morning, let's drive 🏎️

## 🚦 Status of your pipelines and a brief summary

### 🚗 finance-website-pipeline
- [Dashboard Link](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/pipelines/financewebsitepipeline/pipeline-studio/?storeType=INLINE)
- **Identifier:** `financewebsitepipeline` | **Project:** `Phuong_Sandbox_v2` | **Org:** `sandbox`
- **Latest status:** 🟡 ApprovalWaiting
- [Execution Link](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/pipelines/financewebsitepipeline/deployments/5A2rDt3eTvawUALYr1VVyw/pipeline?storeType=INLINE)
- **Summary:** The current run is paused at an approval gate; the last completed run succeeded through Build, Database, Change Mgmt, and Deploy.

### 🏗️ infra-demo
- [Dashboard Link](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/pipelines/infrademo/pipeline-studio?storeType=INLINE)
- **Identifier:** `infrademo` | **Project:** `Phuong_Sandbox_v2` | **Org:** `sandbox`
- **Latest status:** 🟢 Success
- [Execution Link](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/pipelines/infrademo/deployments/FhGiHXn_RQ65O4npz4oNQg/pipeline?storeType=INLINE)
- **Summary:** Latest run completed end-to-end cleanly in about a minute.

### 🚀 End2End Delivery Pipeline
- [Dashboard Link](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/demo/projects/Platform_Engineering/pipelines/End2End_Delivery_Pipeline/pipeline-studio?storeType=REMOTE)
- **Identifier:** `End2End_Delivery_Pipeline` | **Project:** `Platform_Engineering` | **Org:** `demo`
- **Latest status:** 🔴 Failed
- [Execution Link](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/demo/projects/Platform_Engineering/pipelines/End2End_Delivery_Pipeline/deployments/jBbbua_NTU2MyyMda-QB2w/pipeline?storeType=REMOTE)
- **Summary:** Run #1785 failed during Build-Test-Push with exit status 1 in the Gradle build and test steps; all downstream stages were skipped.

## 📌 Topics to look at

### 🤖 Review the AI agent run
- [Execution Link](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/pipelines/financewebsitepipeline/deployments/MkIGwiIFRn-ojr6TTZMbBQ/pipeline?storeType=INLINE)
- **What the AI agents do:** The pipeline runs three autonomous agents. The Kubernetes Manifest Remediator inspects failed Harness CD executions, correlates them with K8s/Helm manifests, applies safe fixes, and opens a PR. The Automated PR Review Agent reviews PR diffs and posts a review comment. The SAST Auto-Remediator fetches SAST findings, fixes CRITICAL/HIGH issues, and opens a security PR. In this run the agents completed the Build, Database, Approval, and Deploy stages successfully.

### 🔄 Review the rollback
- [Execution Link](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/demo/projects/Platform_Engineering/pipelines/End2End_Delivery_Pipeline/deployments/muLmWfhxQ_uaBiU5sK4uwg/pipeline?repoName=org.End2End_Delivery_Pipeline&branch=main-platformdemo&storeType=REMOTE&stage=oGMHHFgZTx6W33AL800iPA&step=VF2zL0KORUy_1d061ubMxQ)
- **What happened:** The `AI_Verify` step ran the post-deployment verification suite and the result came back **FAILED**: resource exhaustion during stress tests produced OOMKilled containers, Redis service outages, and cascading connectivity/time-out failures across dependent services. All test nodes reported `verificationResult: FAILED` with dozens of unhealthy log clusters. Because verification failed, the deployment was rolled back / not promoted to production.

## 🎯 What's on fire / needs my attention
- 🔴 **End2End Delivery Pipeline** latest run failed in Build-Test-Push — Gradle build/test exit status 1.
- 🟡 **finance-website-pipeline** has an execution waiting on approval.
- 🟢 **infra-demo** latest run is green.

## ✅ Action items
- Approve the waiting **finance-website-pipeline** execution.
- Investigate the Gradle **Build_Gradle_App** / **Build_and_Test** failures in **End2End Delivery Pipeline**.
- Confirm **infra-demo** health remains stable.
