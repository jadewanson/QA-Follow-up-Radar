# Manual Test Cases — Follow-Up Item Lifecycle

## Execution Model

All cases are designed for **manual execution**.

Primary execution surface: web UI.

Manual API checks may be used where the current web UI does not expose the required product operation or where additional evidence is required. No automated test execution is part of this suite.

## Test Cases

### TC-01 — Create a valid invoice with a future due date

**Priority:** P0  
**Technique:** Positive testing, state transition  
**Surface:** Web UI  
**Covers:** TCND-C01, TCND-C02, TCND-C03, TCND-A01, TCND-R01

**Preconditions**
- User is authenticated.
- Dashboard is available.
- Future date/time is available for test data.

**Steps**
1. Open the dashboard.
2. Enter a valid Title and Client name.
3. Select **Invoice**.
4. Set **Due at** to a future date/time.
5. Enter a Next action.
6. Click **Create item**.
7. Open the created case.

**Expected result**
- Case is created and remains accessible after navigation/reload.
- Lifecycle status is `open`.
- Payment status is shown separately from lifecycle status.
- Initial reminder state is present for the created case.
- Entered case data is retained.

---

### TC-02 — Project active state across the UTC date boundary

**Priority:** P0  
**Technique:** Boundary Value Analysis, state transition  
**Surface:** Web UI  
**Covers:** TCND-C02, TCND-A01, TCND-A02, TCND-A03

**Preconditions**
- User is authenticated.
- Current UTC date is known.

**Test data**
- Due date before current UTC date.
- Due date on current UTC date.
- Due date after current UTC date.

**Steps**
1. Create one active case with a past due date.
2. Create a separate active case with a due date on the current UTC date.
3. Create a separate active case with a future due date.
4. Return to the dashboard.
5. Observe each case status and board section.

**Expected result**
- Past UTC date → `overdue`.
- Current UTC date → `due_today`.
- Future UTC date → `open`.
- Each case appears in the matching dashboard group.

---

### TC-03 — Reject invalid required creation data

**Priority:** P0  
**Technique:** Equivalence Partitioning, negative testing  
**Surface:** Web UI  
**Covers:** TCND-C04

**Preconditions**
- User is authenticated.
- Create form is available.

**Test data**
- Empty Title.
- Whitespace-only Title.
- Empty Client name.
- Whitespace-only Client name.
- Missing Due at.

**Steps**
1. Attempt to create a case using each invalid data set while keeping the other required values valid.
2. Observe the result.
3. Check the dashboard for an incorrectly created case.

**Expected result**
- Invalid required data is rejected.
- No case with invalid required data is created.
- The user receives a visible validation/error result.

**Note**
- The product documentation does not currently define `title` consistently across the user flow and executable contract. Record observations without closing the requirement finding.

---

### TC-04 — Deliver reminder and provide return path

**Priority:** P0  
**Technique:** End-to-end functional scenario  
**Surface:** Web UI + configured reminder delivery surface  
**Covers:** TCND-R02, TCND-R03

**Preconditions**
- User is authenticated.
- Reminder delivery mechanism is available in the QA environment.
- Active case has a near-future due time.

**Steps**
1. Wait until the configured reminder is due.
2. Verify that the reminder is delivered through the configured test environment.
3. Use the reminder return path.
4. Verify the user can return to the product/case flow.

**Expected result**
- Reminder is delivered at the applicable due time.
- Reminder identifies the relevant follow-up.
- Return path is usable and leads back into the product flow.

---

### TC-05 — Reschedule an overdue case to a future date

**Priority:** P0  
**Technique:** State Transition Testing  
**Surface:** Web UI  
**Covers:** TCND-S01, TCND-S02, TCND-S03, TCND-R04

**Preconditions**
- User has an `overdue` case.
- Case has reminder coverage.

**Steps**
1. Open the overdue case.
2. Set **Reschedule** to a future date/time.
3. Click **Reschedule**.
4. Reload the case and dashboard.

**Expected result**
- New due date is saved.
- Lifecycle status becomes `open`.
- Case moves to the scheduled section.
- Reminder state reflects coverage for the new due time.
- Result persists after reload.

---

### TC-06 — Reschedule between active temporal states

**Priority:** P0  
**Technique:** State Transition Testing, Boundary Value Analysis  
**Surface:** Web UI  
**Covers:** TCND-S01, TCND-S02, TCND-A01, TCND-A02, TCND-A03

**Preconditions**
- User has an active case.

**Test data**
- Future due date.
- Current UTC date.
- Past UTC date.

**Steps**
1. Reschedule the active case using each date class.
2. Observe the lifecycle status after each successful reschedule.

**Expected result**
- Future → `open`.
- Current UTC date → `due_today`.
- Past → `overdue`.
- Case remains active after each valid reschedule.

---

### TC-07 — Prevent rescheduling a completed case

**Priority:** P0  
**Technique:** State Transition Testing, negative testing  
**Surface:** Web UI  
**Covers:** TCND-S04, TCND-CM06

**Preconditions**
- Case is `done` or `lost`.

**Steps**
1. Open the completed case.
2. Inspect the Reschedule control.
3. Attempt the action if the UI permits interaction.
4. Reload the case.

**Expected result**
- Reschedule is unavailable or rejected.
- Lifecycle state remains unchanged.
- Due date is not modified.

---

### TC-08 — Mark a mutable payment-applicable case as paid

**Priority:** P0  
**Technique:** State Transition Testing  
**Surface:** Web UI  
**Covers:** TCND-P01, TCND-P02

**Preconditions**
- Active case currently exposes **Mark paid**.
- Payment status is not already `paid`.

**Steps**
1. Record the current lifecycle status.
2. Click **Mark paid**.
3. Reload the item.

**Expected result**
- Payment status becomes `paid`.
- Lifecycle status remains exactly the same.
- The case is not automatically completed, lost or archived.
- Payment state persists after reload.

---

### TC-09 — Prevent Mark Paid when payment is not applicable

**Priority:** P0  
**Technique:** Decision Table Testing, negative testing  
**Surface:** Web UI  
**Covers:** TCND-P01, TCND-P03

**Preconditions**
- Case is displayed with payment status `not_applicable`.

**Steps**
1. Open the case.
2. Inspect available actions.
3. Verify whether **Mark paid** is exposed.

**Expected result**
- Mark Paid is not available for a case whose payment status is `not_applicable`.
- Lifecycle and payment state remain unchanged.

**Requirement note**
- The product documentation does not currently define which item types must receive `not_applicable`; that mapping remains a separate requirement finding.

---

### TC-10 — Complete an active case as Done

**Priority:** P0  
**Technique:** State Transition Testing  
**Surface:** Web UI  
**Covers:** TCND-CM01, TCND-CM03, TCND-CM04, TCND-CM05, TCND-R04

**Preconditions**
- Active case has a Next action.
- Case has reminder coverage.

**Steps**
1. Open the active case.
2. Enter a non-empty Outcome reason.
3. Click **Mark done**.
4. Reload the case.

**Expected result**
- Lifecycle status becomes `done`.
- Outcome reason is displayed for the completed case.
- Completion state persists after reload.
- Previous Next action is cleared.
- Pending reminder coverage is canceled.

---

### TC-11 — Complete an active case as Lost

**Priority:** P0  
**Technique:** State Transition Testing  
**Surface:** Web UI  
**Covers:** TCND-CM02, TCND-CM03, TCND-CM04, TCND-CM05, TCND-R04

**Preconditions**
- Active case has a Next action.
- Case has reminder coverage.

**Steps**
1. Open the active case.
2. Enter a non-empty Outcome reason.
3. Click **Mark lost**.
4. Reload the case.

**Expected result**
- Lifecycle status becomes `lost`.
- Outcome reason is displayed for the completed case.
- Completion state persists after reload.
- Previous Next action is cleared.
- Pending reminder coverage is canceled.

---

### TC-12 — Reject completion without an outcome reason

**Priority:** P0  
**Technique:** Equivalence Partitioning, negative testing  
**Surface:** Web UI  
**Covers:** TCND-CM03

**Preconditions**
- Case is active.

**Test data**
- Empty reason.

**Steps**
1. Leave Outcome reason empty.
2. Click **Mark done**.
3. Repeat for **Mark lost**.
4. Observe the case state.

**Expected result**
- Completion is rejected.
- User receives a visible error/validation result.
- Case remains active.
- No completion reason or terminal state is persisted.

---

### TC-13 — Reject lifecycle mutations after completion

**Priority:** P0  
**Technique:** State Transition Testing, negative testing  
**Surface:** Web UI, with manual API confirmation where the UI does not expose the forbidden action  
**Covers:** TCND-S04, TCND-CM06

**Preconditions**
- One `done` case and one `lost` case exist.

**Steps**
1. Open each terminal case.
2. Inspect Reschedule, Mark done and Mark lost controls.
3. Confirm forbidden actions are disabled/unavailable or rejected.
4. Reload the case after any attempted action.

**Expected result**
- Terminal cases cannot be rescheduled or completed again.
- Existing terminal state and stored outcome information remain unchanged.

---

### TC-14 — Reopen a Done case

**Priority:** P0  
**Technique:** State Transition Testing  
**Surface:** Web UI  
**Covers:** TCND-RO01, TCND-RO02, TCND-RO03, TCND-RO04

**Preconditions**
- Case is `done`.
- Case had a Next action before completion.
- Existing `due_at` is known.

**Steps**
1. Open the completed case.
2. Click **Reopen case**.
3. Reload the case and dashboard.

**Expected result**
- Case returns to the active status projected from existing `due_at`.
- Completion state is cleared.
- Previous Next action is not restored.
- Reminder coverage is re-armed.
- Reopened state persists after reload.

---

### TC-15 — Reopen a Lost case

**Priority:** P0  
**Technique:** State Transition Testing  
**Surface:** Web UI  
**Covers:** TCND-RO01, TCND-RO02, TCND-RO03, TCND-RO04

**Preconditions**
- Case is `lost`.
- Existing `due_at` is known.

**Steps**
1. Open the lost case.
2. Click **Reopen case**.
3. Reload the case and dashboard.

**Expected result**
- Case returns to the active status projected from existing `due_at`.
- Completion state is cleared.
- Previous Next action is not restored.
- Reminder coverage is re-armed.

---

### TC-16 — Reopen control is unavailable for active cases

**Priority:** P0  
**Technique:** State Transition Testing, negative testing  
**Surface:** Web UI  
**Covers:** TCND-RO05

**Preconditions**
- Cases exist in `open`, `due_today` and `overdue`.

**Steps**
1. Open each active case.
2. Inspect the available actions.

**Expected result**
- **Reopen case** is not available for any active lifecycle state.
- No lifecycle state changes.

---

### TC-17 — Archive a non-archived case

**Priority:** P0  
**Technique:** State Transition Testing  
**Surface:** Manual API  
**Covers:** TCND-AR01, TCND-AR02, TCND-AR03

**Preconditions**
- Authenticated user owns a non-archived case.
- Case has pending reminder coverage when applicable.

**Steps**
1. Send the archive request manually for the owned case.
2. Refresh the normal dashboard/registry.
3. Inspect the case state through an appropriate manual read operation if needed.

**Expected result**
- Case lifecycle becomes `archived`.
- Case is hidden from normal active views.
- Pending reminders are canceled.
- Other persisted case data is not unintentionally changed.

---

### TC-18 — Reject repeated Archive and Reopen from Archived

**Priority:** P0  
**Technique:** State Transition Testing, negative testing  
**Surface:** Manual API  
**Covers:** TCND-AR04, TCND-AR05, TCND-RO05

**Preconditions**
- Owned case is already `archived`.

**Steps**
1. Attempt to archive the case again manually.
2. Attempt to reopen the archived case manually.
3. Read the case state after both attempts.

**Expected result**
- Repeated Archive is rejected.
- Reopen from `archived` is rejected.
- Case remains `archived`.
- No new active reminder coverage is created.

---

### TC-19 — Terminal states remain terminal when due date becomes past

**Priority:** P0  
**Technique:** State Transition Testing, temporal boundary testing  
**Surface:** Web UI  
**Covers:** TCND-A04

**Preconditions**
- A case is completed as `done` or `lost` with a due date that will be or already is before the current UTC date.

**Steps**
1. Open the terminal case after its due date is in the past.
2. Reload the page/dashboard.

**Expected result**
- `done` remains `done`, or `lost` remains `lost`.
- Temporal projection does not convert the case to `overdue`.

---

### TC-20 — Lifecycle activity remains traceable after manual actions

**Priority:** P1  
**Technique:** Scenario testing  
**Surface:** Web UI  
**Covers:** Supporting lifecycle evidence

**Preconditions**
- A case has been created and has gone through at least two lifecycle/payment actions.

**Steps**
1. Open the case.
2. Review the **Activity** section.
3. Compare visible activity with the actions actually performed.

**Expected result**
- Relevant state/payment/operational activity is associated with the correct case.
- Activity does not show a lifecycle transition that was not performed.
- Visible history is consistent with the current case state.

## Design Notes

- These cases are the retained manual test-design baseline for the completed lifecycle cycle.
- Test results are not recorded in this file.
- The cases were implemented in Zephyr; Zephyr is the source of truth for manual test case versions and Pass / Fail / Blocked execution status.
- Archive coverage uses manual API execution because the current web UI implementation does not expose an Archive action.
- Requirement gaps remain tracked separately and are not silently converted into expected product behaviour.
