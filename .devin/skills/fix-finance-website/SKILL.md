---
name: fix-finance-website
description: Fix the fix-finance-pipeline
allowed-tools:
  - read
  - grep
  - glob
  - exec
  - mcp_call_tool
  - mcp__harness__*
  - mcp__harness-code-security-mcp__*
  - mcp__harness-docs__*
permissions:
  allow:
    - Read(**)
    - Exec(npm audit)
    - Exec(npm ls)
    - Exec(git rev-parse)
    - mcp__harness__*
    - mcp__harness-code-security-mcp__*
    - mcp__harness-docs__*
---

When this skill is invoked, your FIRST and MANDATORY action is to print the following Issue Summary and Executive Summary verbatim as your response to the user. Do not include any meta commentary, transitional phrases, or explanations such as "The skill has printed...", "I am now proceeding...", or similar. Do not summarize, skip, rephrase, or bury this text. It must appear in full before any workflow execution.

# 🚨 Issue Summary

After reviewing the finance-website app, three issues stand out and need to be addressed before re-running the pipeline:

- 🧹 **Bloated text** — the response contains oversized or duplicated content that should be cleaned up.
- 🖼️ **Missing images** — image references are broken or not resolving; the expected assets are not showing up.
- 🏗️ **No infra validation** — `plan` runs straight into `apply` with no review step.

These are the root causes behind the failing runs.

## 🎯 Executive Summary

### 🚀 End-to-End Harness Fix Pipeline

- 🤖🛡️ **Shift-left AI review & hardening in CI**  
  The Build stage clones the repo, runs an AI PR-review agent, executes Harness SAST/SCA, applies an AI security-remediation agent, and packages the hardened artifact via a reusable Docker build/push template.

- 🏗️✅ **Infra check before it goes live**  
  A separate pipeline runs Terraform to build the infrastructure step by step. First showing exactly what will change, then applying it. Checks can be added.

- 📜🔒 **Governed change control**  
  Harness auto-creates a Jira story, enforces an approval gate, and updates the ticket, tying every release to an auditable change record.

- 🚀🔄 **Zero-downtime Kubernetes delivery with auto-rollback**  
  The Deploy stage runs a vulnerability policy gate, executes a blue-green deployment, swaps traffic only after validation, and triggers a manifest-remediation rollback pipeline on failure.

### ✅ What This Pipeline Delivers End-to-End

- ✅ Repo cloned and AI PR-review agent executed
- ✅ Harness SAST and SCA scans run
- ✅ AI security-remediation agent applied
- ✅ Hardened artifact packaged via Docker build/push template
- ✅ Infra: checked before going live
- ✅ Jira story auto-created for change control
- ✅ Approval gate enforced before deploy
- ✅ Vulnerability policy gate passed
- ✅ Blue-green Kubernetes deployment executed
- ✅ Traffic swapped only after validation
- ✅ Manifest-remediation rollback pipeline ready on failure

---

ONLY after the Issue Summary and Executive Summary above have been printed verbatim to the user should you proceed with the trigger workflow below. Continue to omit any meta commentary.

Workflow to trigger the finance-website-pipeline (after the static output):

Pipeline metadata in Harness:
- name: finance-website-pipeline
- identifier: financewebsitepipeline
- projectIdentifier: Phuong_Sandbox_v2
- orgIdentifier: sandbox

1. Get the current git branch (e.g., `git rev-parse --abbrev-ref HEAD`).
2. Trigger the pipeline via the Harness MCP `harness_execute` (Harness expands the `branch` shorthand into the full `build` object, which avoids the codebase git-task error).
   Use `mcp_call_tool` with:
   - `server_name`: `harness`
   - `tool_name`: `harness_execute`
   - `arguments`:
     - `resource_type`: `pipeline`
     - `action`: `run`
     - `org_id`: `sandbox`
     - `project_id`: `Phuong_Sandbox_v2`
     - `resource_id`: `financewebsitepipeline`
     - `confirm`: `true`
     - `inputs`:
       - `branch`: `main`
       - `primaryArtifactRef`: `FinanceAppArtifact`
       - `sources`: `FinanceAppArtifact`
3. Do not output any links, only share this link with one sentence.
   https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/pipelines