# QA Case Study — Follow-Up Item Lifecycle

## System Under Test

[Follow-Up Radar](https://github.com/jadewanson/Follow-Up-Radar)

Follow-Up Radar is a training product used as the system under test for this manual QA case study.

## Test Objective

Verify that a user can work with a follow-up case through its documented lifecycle and that the product preserves the expected lifecycle, payment and reminder behaviour at each supported transition.

## Scope

**Create → Active State → Reminder → Reschedule → Mark Paid → Complete (Done/Lost) → Archive → Reopen**

This is a coverage scope, not one mandatory linear path. Allowed and forbidden transitions are verified against the documented product rules.

## Current Status

| Activity | Status |
|---|---|
| Planning | Complete |
| Requirements review | Complete |
| Test analysis | Complete |
| Test design | Complete |
| Zephyr test implementation | Complete |
| Manual execution | Complete |
| Defect reporting | Complete |
| Retest / regression | Complete |
| Test completion report | Complete |

## Execution Outcome

| Cycle | Purpose | Result |
|---|---|---|
| FUT-R1 | Initial manual system test | 20 executed — 17 Pass / 2 Fail / 1 Blocked |
| FUT-R2 | FUT-13 retest | 2 executed — 2 Pass |
| FUT-R3 | Targeted regression | 6 executed — 6 Pass |

The two initial failures exposed one High-priority defect, **FUT-13 — Reopening a completed case does not re-arm the canceled reminder**. The defect was fixed, successfully retested across Done and Lost reopen paths, and closed. The targeted regression passed.

The repository now includes portable Jira and Zephyr result snapshots so the execution history and defect lifecycle can be reviewed without access to the private test-management workspace.

One residual environment limitation remains: automatic reminder dispatch at the configured due time could not be verified in the current local Docker Compose environment.

## Visual Evidence

Selected screenshots provide a compact review of the execution environment and QA workflow:

- **Docker** — local Docker Compose QA environment with PostgreSQL, API, web and Redis services running, plus a successful backend health check.
- **Zephyr Advanced** — 20 approved manual test cases organized by Follow-Up Item lifecycle area.
- **Jira** — QA work tracked through completion, including the lifecycle scope, completion activity and closed FUT-13 defect.
- **Postman** — manual API state verification used as supporting evidence during system testing.

[Open the visual evidence gallery](04-execution/evidence/README.md)

## Project Artifacts

### 01 — Planning

- [Test Approach](01-planning/test-approach.md)

### 02 — Analysis

- [Requirements Review](02-analysis/requirements-review.md)
- [Test Analysis](02-analysis/test-analysis.md)
- [Implementation Observations](02-analysis/implementation-observations.md)

### 03 — Design

- [State Transition Matrix](03-design/state-transition-matrix.md)
- [Manual Test Cases](03-design/test-cases.md)

### 04 — Execution

- [Test Execution Summary](04-execution/test-execution-summary.md)
- [Visual Execution Evidence](04-execution/evidence/README.md)
- [Execution Evidence Exports](04-execution/exports/README.md)
  - [Zephyr execution results](04-execution/exports/zephyr-execution-results.md)
  - [Zephyr execution results CSV](04-execution/exports/zephyr-execution-results.csv)
  - [Jira FUT-13 defect export](04-execution/exports/jira-fut-13-defect.md)

### 05 — Completion

- [Test Completion Report](05-completion/test-completion-report.md)

The Markdown test-case set is retained as the design artifact. Zephyr is the source of truth for the implemented manual test cases and execution statuses.

## Test Basis

Primary product documentation:

- `docs/01_product_constitution.md`
- `docs/03_scope_v1.md`
- `docs/04_user_flows_and_activation.md`
- `docs/05_domain_model.md`
- `docs/06_state_machine.md`
- `docs/08_api_contracts.md`
- `docs/11_testing_strategy_and_quality_gates.md`

Implementation may be reviewed as supporting evidence where the UI or documentation is insufficient to explain an observation. Implementation is not used to invent missing business requirements.

## Execution Model

Testing is manual and UI-first.

Browser DevTools, manual API requests and read-only persisted-state checks may be used as supporting evidence when the UI does not expose enough information to verify a result. The current web UI does not expose Archive, so Archive coverage requires manual API execution.

## Evidence Policy

Runtime Pass / Fail / Blocked claims are based on completed manual executions in Zephyr.

Requirements/documentation findings are kept separate from confirmed product defects. Product defects are reported only when observed behaviour contradicts a sufficiently defined expected result.
