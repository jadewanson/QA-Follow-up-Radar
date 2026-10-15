# Test Completion Report — Follow-Up Item Lifecycle

## 1. Test Objective

Verify that a user can work with a follow-up case through the documented lifecycle and that lifecycle, payment and reminder behaviour remains consistent across supported and forbidden transitions.

## 2. Scope

The completed test scope covered:

**Create → Active State → Reminder → Reschedule → Mark Paid → Complete (Done/Lost) → Archive → Reopen**

This scope was treated as a set of lifecycle capabilities and transitions, not as one mandatory linear user path.

Testing focused on:

- lifecycle state transitions;
- active-state projection from due date;
- reminder creation and lifecycle handling;
- rescheduling;
- payment/lifecycle independence;
- completion as Done and Lost;
- required completion reason;
- terminal-state restrictions;
- reopening completed cases;
- archive behaviour;
- persistence after reload;
- lifecycle activity traceability.

## 3. Test Approach

Testing was performed as manual, functional, risk-based system testing.

Primary execution surface:

- Web UI

Supporting manual verification:

- Postman / HTTP requests where the UI did not expose the required operation;
- browser-visible state and activity;
- manual persisted-state confirmation where necessary.

No automated test implementation was included in this QA scope.

## 4. Test Environment

- Local Docker Compose environment
- Web UI running locally
- Backend API running locally
- PostgreSQL
- Redis
- Chrome
- Postman
- Jira
- Zephyr Advanced

Execution period: **14–15 October 2026**.

## 5. Execution Summary

### Initial system test — FUT-R1

**Follow-Up Lifecycle — Manual System Test — Cycle 01**

| Result | Count |
|---|---:|
| Executed | 20 |
| Pass | 17 |
| Fail | 2 |
| Blocked | 1 |
| Not Executed | 0 |

The two failed executions were:

- TC-14 — Reopen a Done case
- TC-15 — Reopen a Lost case

The blocked execution was:

- TC-04 — Deliver reminder and provide return path

### Defect retest — FUT-R2

**Follow-Up Lifecycle — FUT-13 Retest — Cycle 02**

| Result | Count |
|---|---:|
| Executed | 2 |
| Pass | 2 |
| Fail | 0 |
| Blocked | 0 |

### Targeted regression — FUT-R3

**FUT-13 Targeted Regression — Cycle 03**

| Result | Count |
|---|---:|
| Executed | 6 |
| Pass | 6 |
| Fail | 0 |
| Blocked | 0 |

Retest and regression results are reported separately from the initial cycle because they are additional executions of existing test cases, not additional unique test cases.

## 6. Defect Summary

### FUT-13 — Reopening a completed case does not re-arm the canceled reminder

**Priority:** High  
**Final status:** Done

The defect was first observed during TC-14 and reproduced independently through TC-15.

Observed behaviour before the fix:

- reopening from Done or Lost correctly returned the case to an active lifecycle state;
- the existing due date remained available for active-state projection;
- the previous next action was not restored;
- the reminder canceled during completion remained canceled after Reopen;
- the reopened active case therefore had no active reminder coverage.

Expected behaviour:

- reopening a Done or Lost case must return it to the correct active state;
- completion state must be cleared;
- the previous next action must not be restored;
- reminder coverage must be re-armed for the existing due date.

The fix was verified in FUT-R2 against both affected completion paths. TC-14 and TC-15 both passed after the fix.

A targeted regression was then completed in FUT-R3 with **6/6 passing executions**. No regression defect was observed in the selected coverage.

## 7. Blocked Test and Residual Risk

### TC-04 — Deliver reminder and provide return path

**Final execution status:** Blocked

The current local Docker Compose environment does not automatically run the reminder dispatch job at the configured due time.

The following behaviour was verified manually:

- a reminder is created for the case;
- due reminders can be dispatched manually;
- the reminder transitions to sent state;
- reminder content identifies the relevant follow-up;
- the return path leads back into the product/case flow.

The following remains unverified:

- automatic reminder dispatch at the configured due time without manual invocation.

### Residual risk

There is insufficient evidence from this environment to claim that scheduled reminder delivery operates automatically at due time.

This is an environment/implementation limitation for the completed test cycle and is not reported as a confirmed product defect without additional evidence of a requirement violation in a representative runtime environment.

## 8. Requirements and Documentation Findings

The requirements review identified documentation gaps that remain separate from confirmed runtime defects:

- inconsistent high-level wording around Paid versus terminal lifecycle states;
- create-flow documentation omits the required title field;
- archive recovery behaviour is not defined;
- item-type-to-payment-applicability mapping is not fully defined in the reviewed product documentation.

These findings did not prevent completion of the defined lifecycle scope where a sufficiently clear test oracle was available.

## 9. Exit Criteria Assessment

| Exit criterion | Result |
|---|---|
| Planned lifecycle tests executed or explicitly blocked | Met |
| Every planned test has an execution status | Met |
| Observed defects recorded with reproducible evidence | Met |
| Requirements/documentation findings kept separate from product defects | Met |
| Delivered fix retested | Met |
| Targeted regression performed | Met |
| Unresolved blocked tests and residual risks documented | Met |
| No unexecuted test represented as passed | Met |

The defined exit criteria for this lifecycle QA cycle are satisfied.

## 10. Overall QA Assessment

Within the tested scope, the core Follow-Up Item lifecycle is behaving consistently with the available test oracle after the FUT-13 fix.

The primary defect discovered during initial execution was fixed and successfully verified across both affected completion paths. Targeted regression did not reveal a regression in the selected adjacent lifecycle/reminder behaviour.

The remaining limitation is the inability to verify automatic scheduled reminder dispatch in the current local Docker Compose environment.

## 11. Completion Decision

**QA cycle completed with one documented residual environment risk.**

The tested lifecycle scope has sufficient evidence to close the current QA cycle.

This conclusion applies only to the defined Follow-Up Item lifecycle scope and does not imply that the entire product is defect-free or that out-of-scope functionality has been verified.
