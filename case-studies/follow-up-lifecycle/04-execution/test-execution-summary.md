# Test Execution Summary — Follow-Up Item Lifecycle

## Execution Scope

Manual system testing of the documented Follow-Up Item lifecycle:

**Create → Active State → Reminder → Reschedule → Mark Paid → Complete (Done/Lost) → Archive → Reopen**

Execution was performed in Zephyr using the approved manual test cases. The primary execution surface was the web UI, with manual API checks used where the UI does not expose the required operation.

## Environment

- Local Docker Compose environment
- Web UI: localhost
- Backend API: localhost
- PostgreSQL
- Redis
- Chrome
- Postman for manual API checks
- Jira for defect tracking
- Zephyr Advanced for test execution

## Cycle Results

| Cycle | Purpose | Executed | Pass | Fail | Blocked | Status |
|---|---|---:|---:|---:|---:|---|
| FUT-R1 — Follow-Up Lifecycle — Manual System Test — Cycle 01 | Initial system test execution | 20 | 17 | 2 | 1 | Done |
| FUT-R2 — Follow-Up Lifecycle — FUT-13 Retest — Cycle 02 | Retest of the reminder re-arm fix | 2 | 2 | 0 | 0 | Done |
| FUT-R3 — FUT-13 Targeted Regression — Cycle 03 | Targeted regression around the affected lifecycle/reminder area | 6 | 6 | 0 | 0 | Done |

Cycle totals are reported separately because retest and regression reuse existing test cases and are not additional unique test cases.

## Initial Execution Findings

### Failed

- **TC-14 — Reopen a Done case**
- **TC-15 — Reopen a Lost case**

Both failures exposed the same defect: after reopening a completed case, the lifecycle returned to the correct active state but reminder coverage was not re-armed.

The defect was reported as **FUT-13 — Reopening a completed case does not re-arm the canceled reminder**.

### Blocked

- **TC-04 — Deliver reminder and provide return path**

The current local Docker Compose environment does not automatically run the reminder dispatch job at the configured due time.

The following parts were verified manually:

- reminder creation;
- manual reminder dispatch;
- reminder content;
- transition to sent state;
- return path to the product/case flow.

Automatic scheduled dispatch at the due time remains unverified in this environment.

## Retest Result

FUT-13 was retested in **FUT-R2** against both affected completion paths:

- TC-14 — Reopen from Done: **Pass**
- TC-15 — Reopen from Lost: **Pass**

After the fix, reopening a completed case restored the correct active lifecycle state and reminder coverage was re-armed for the existing due time.

FUT-13 was then marked **Done**.

## Targeted Regression Result

A targeted regression cycle was executed after the FUT-13 fix to check adjacent lifecycle and reminder behaviour that could have been affected by the change.

**FUT-R3 result: 6 Pass / 0 Fail / 0 Blocked.**

No regression defect was observed in the selected coverage.

## Execution Conclusion

The planned lifecycle suite was fully executed.

The only product defect found during the initial cycle was fixed, successfully retested through both affected paths, and followed by a passing targeted regression.

One environment limitation remains: automatic scheduled reminder dispatch could not be verified in the current local Docker Compose setup.
