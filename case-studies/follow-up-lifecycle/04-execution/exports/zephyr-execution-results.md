# Zephyr Execution Results Snapshot — Follow-Up Item Lifecycle

**Source system:** Zephyr Advanced  
**Snapshot date:** 15 October 2026  
**Project:** Follow-Up Radar Testing

This file is a portable portfolio snapshot of the completed manual test executions. Zephyr remains the source of truth for the live test cases, executions and cycle statuses.

## Test Cycles

| Cycle | Purpose | Progress | Final status | Result |
|---|---|---:|---|---|
| FUT-R1 — Follow-Up Lifecycle — Manual System Test — Cycle 01 | Initial manual system test | 100% | Done | 17 Pass / 2 Fail / 1 Blocked |
| FUT-R2 — Follow-Up Lifecycle — FUT-13 Retest — Cycle 02 | Retest of FUT-13 | 100% | Done | 2 Pass |
| FUT-R3 — FUT-13 Targeted Regression — Cycle 03 | Targeted regression after FUT-13 fix | 100% | Done | 6 Pass |

## FUT-R1 — Initial System Test

| Zephyr key | Test case | Result |
|---|---|---|
| FUT-T1 | TC-01 — Create a valid invoice with a future due date | Pass |
| FUT-T2 | TC-02 — Project active state across the UTC date boundary | Pass |
| FUT-T3 | TC-03 — Reject invalid required creation data | Pass |
| FUT-T4 | TC-04 — Deliver reminder and provide return path | Blocked |
| FUT-T5 | TC-05 — Reschedule an overdue case to a future date | Pass |
| FUT-T6 | TC-06 — Reschedule between active temporal states | Pass |
| FUT-T7 | TC-07 — Prevent rescheduling a completed case | Pass |
| FUT-T8 | TC-08 — Mark a mutable payment-applicable case as paid | Pass |
| FUT-T9 | TC-09 — Prevent Mark Paid when payment is not applicable | Pass |
| FUT-T10 | TC-10 — Complete an active case as Done | Pass |
| FUT-T11 | TC-11 — Complete an active case as Lost | Pass |
| FUT-T12 | TC-12 — Reject completion without an outcome reason | Pass |
| FUT-T13 | TC-13 — Reject lifecycle mutations after completion | Pass |
| FUT-T14 | TC-14 — Reopen a Done case | Fail |
| FUT-T15 | TC-15 — Reopen a Lost case | Fail |
| FUT-T16 | TC-16 — Reopen control is unavailable for active cases | Pass |
| FUT-T17 | TC-17 — Archive a non-archived case | Pass |
| FUT-T18 | TC-18 — Reject repeated Archive and Reopen from Archived | Pass |
| FUT-T19 | TC-19 — Terminal states remain terminal when due date becomes past | Pass |
| FUT-T20 | TC-20 — Lifecycle activity remains traceable after manual actions | Pass |

### Initial-cycle outcome

- Executed: 20
- Pass: 17
- Fail: 2
- Blocked: 1
- Not Executed: 0

TC-14 and TC-15 exposed the same reminder re-arm defect, reported as Jira FUT-13.

TC-04 remained Blocked because the current local Docker Compose environment does not automatically run the reminder dispatch job at the configured due time. Manual reminder creation, manual dispatch, sent state, content and return path were verified.

## FUT-R2 — FUT-13 Retest

| Zephyr key | Test case | Result |
|---|---|---|
| FUT-T14 | TC-14 — Reopen a Done case | Pass |
| FUT-T15 | TC-15 — Reopen a Lost case | Pass |

The cycle is traced to Jira defect FUT-13. After the fix, reminder coverage was re-armed correctly for both Done and Lost reopen paths.

## FUT-R3 — Targeted Regression

Targeted regression was selected around lifecycle/reminder behaviour adjacent to the FUT-13 change.

| Zephyr key | Test case | Result |
|---|---|---|
| FUT-T5 | TC-05 — Reschedule an overdue case to a future date | Pass |
| FUT-T10 | TC-10 — Complete an active case as Done | Pass |
| FUT-T11 | TC-11 — Complete an active case as Lost | Pass |
| FUT-T13 | TC-13 — Reject lifecycle mutations after completion | Pass |
| FUT-T16 | TC-16 — Reopen control is unavailable for active cases | Pass |
| FUT-T17 | TC-17 — Archive a non-archived case | Pass |

### Regression outcome

- Executed: 6
- Pass: 6
- Fail: 0
- Blocked: 0

No regression defect was observed in the selected coverage.
