---
name: validate-and-approvals
description: Validation and approvals report for finance-website-pipeline
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

**MANDATORY:** When this skill is invoked, output the **Static Baseline Report** below verbatim first. Do not fetch live Harness data, do not run audits, and do not add meta commentary for this initial output. Output the report verbatim, then stop.

Only if the user explicitly asks for a **real-time sync** after the static report, then perform the workflow in the **Real-Time Sync** section at the bottom. Use `confirm: true` for all Harness MCP calls.

**Target Pipeline:** https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/pipelines/financewebsitepipeline/pipeline-studio/?storeType=INLINE

---

## Static Baseline Report

# validate-and-approvals

## 1. Validate

Pre-check whether the next pipeline run may fail because of Harness security or vulnerability policies. This scans the codebase and dependencies against the policies configured in the `Vulnerabilities_Gate` so you can fix blockers before clicking **Run**.

### What is checked
- **Harness Vulnerability Policy** active on `financewebsitepipeline`.
- **Root app dependencies** from `package.json`.
- **Other manifests in the repo** that fall inside the SCA scan scope (e.g., `policy-demo/package.json`).
- **npm audit findings** for the root app.

### Why this matters
If a **CRITICAL** vulnerability is present when the pipeline reaches the `Vulnerabilities_Gate`, the gate denies the run. Catching this now avoids wasted executions and blocked deployments.

### Policy Status
| Policy Type | Status | Details | Link |
|-------------|--------|---------|------|
| Vulnerability Policy | Active / Enforced | `new_policy_09_04_21_45` — deny if CRITICAL count from HarnessCode output is not zero | [Policy](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/settings/governance/policies/edit/new_policy_09_04_21_45) |
| Vulnerability Policy Set | Enabled | `new_policy_set_09_04_21_45` mapped to `Vulnerabilities_Gate` in `financewebsitepipeline` | [Policy Set](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/settings/governance/policy-sets/new_policy_set_09_04_21_45) |

### Codebase Check
| Component | Status | Issue | Link |
|-----------|--------|-------|------|
| Root app (`package.json`) | ✅ Compliant | 3 moderate advisories (`qs`, `body-parser`, `express`); 0 critical / 0 high | [Pipeline](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/pipelines/financewebsitepipeline/pipeline-studio/?storeType=INLINE) |
| `policy-demo` package | ❌ Violation | 1 critical (`minimist`) + 3 high (`axios`, `lodash`, `node-fetch`) | `policy-demo/package.json` |

### What to do if issues are found
- Upgrade or remove the offending dependency.
- Re-run `npm audit` to confirm the fix.
- Re-check with this skill before re-running the pipeline.

### Recommendations
| Priority | Issue | Action Required |
|----------|-------|----------------|
| 🔴 Critical | `minimist@1.2.5` prototype pollution — CVE-2021-44906, CVSS 9.8 | Upgrade `minimist` to >=1.2.6 or remove `policy-demo` if it is not production code |
| 🟠 High | `lodash@4.17.15` code injection / prototype pollution | Upgrade `lodash` to >=4.18.0 |
| 🟠 High | `axios@0.21.0` DoS / prototype pollution / header leak | Upgrade `axios` to latest stable |
| 🟠 High | `node-fetch@2.6.0` header leak | Upgrade `node-fetch` to >=2.6.7 |
| 🟡 Medium | Root `express` / `qs` moderate advisories | Run `npm update express` and re-run `npm audit` |

---

## 2. Harness Approvals

### What happened until the approval
The current `finance-website-pipeline` execution started, governance policy checks passed, and the run reached the **Change Mgmt** Jira Approval stage. It is now paused waiting for the Jira ticket to move to **Done** before it can proceed to **Deploy**.

**Current execution:** [finance-website-pipeline run #82](https://app.harness.io/ng/account/EeRjnXTnS4GrLG5VNNJZUw/all/orgs/sandbox/projects/Phuong_Sandbox_v2/pipelines/financewebsitepipeline/deployments/5A2rDt3eTvawUALYr1VVyw/pipeline?storeType=INLINE)

### Outstanding Approvals
| Ticket | Run | Status | Assignee | Jira Link |
|--------|-----|--------|----------|-----------|
| **HD-201049** | #82 | To Do | — | [HD-201049](https://harness.atlassian.net/browse/HD-201049) |

> The Jira approval criteria require the ticket **Status** to equal **Done** before the pipeline can continue.

---

## Real-Time Sync

If the user explicitly asks for a real-time sync after the static report, perform the following workflow. Use `confirm: true` for all Harness MCP calls.

1. Get the latest vulnerability policies from Harness (org: `sandbox`, project: `Phuong_Sandbox_v2`).
2. Run `npm audit` on the root app and the `policy-demo` directory.
3. Check for the latest `finance-website-pipeline` execution.
4. List outstanding Jira approval instances for that execution.
5. Output a fresh report in the same format as the static report above.
