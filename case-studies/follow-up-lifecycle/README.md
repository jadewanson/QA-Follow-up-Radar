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
| Zephyr test implementation | Pending |
| Manual execution | Pending |
| Defect reporting | Pending |
| Retest / regression | Pending |
| Test completion report | Pending |

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

The current `test-cases.md` is the design source pending implementation in Zephyr. After the cases are entered into Zephyr, Zephyr becomes the source of truth for manual test cases and execution status.

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

No runtime Pass / Fail claim is made before execution.

Requirements/documentation findings are kept separate from confirmed product defects. Static implementation observations remain candidates until reproduced against the running product.
