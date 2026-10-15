# Jira Defect Export — FUT-13

**Jira key:** FUT-13  
**Issue type:** Bug  
**Priority:** High  
**Final status:** Done  
**Created:** 14 October 2026, 23:08 (+03:00)  
**Last updated:** 15 October 2026, 14:03 (+03:00)

## Summary

**Reopening a completed case does not re-arm the canceled reminder**

## Description Snapshot

A completed follow-up case can be reopened successfully and its lifecycle state is projected correctly from the existing `due_at`.

However, the reminder that was canceled when the case was completed remains in canceled state after reopening.

As a result, the reopened active case has no active reminder coverage.

## Preconditions

1. A follow-up case exists in an active lifecycle state.
2. The case has an existing `due_at` value.
3. A pending reminder exists for the case.
4. The case is completed as Done with a valid outcome reason.
5. Completion cancels the pending reminder.
6. Reopen case action is available.

## Steps to Reproduce

1. Open an active follow-up case with an existing `due_at` and a pending reminder.
2. Complete the case as Done using a valid outcome reason.
3. Verify that the lifecycle status becomes Done.
4. Verify that the pending reminder becomes Canceled.
5. Click **Reopen case**.
6. Inspect the lifecycle state.
7. Inspect the Last reminder state.

## Actual Result

The case reopened successfully and became active according to its existing `due_at`.

Observed on the original Done-path reproduction:

- lifecycle state changed from Done to `due_today`;
- `due_at` remained 15/10/2026 01:00;
- the previous `next_action` was not restored;
- the reminder remained Canceled at 15/10/2026 01:00.

Reminder coverage was not re-armed for the reopened case.

**Reproduced item ID:** `19461a5e-de80-4741-8163-edb88693676b`

**Test evidence:** TC-14 — Reopen a Done case, Step 7 — Verify reminder coverage.

## Expected Result

When a completed Done case is reopened:

- the case becomes active and its lifecycle state is projected from the existing `due_at`;
- the previous `next_action` is not restored;
- completion data is cleared as specified;
- reminder coverage is re-armed for the existing `due_at`.

The reopened active case must have active reminder coverage.

## Additional Reproduction — Lost Path

The same defect was reproduced during TC-15.

The case changed from Lost to Open according to its existing `due_at`, but the canceled reminder was not re-armed and remained canceled.

**Reproduced item ID:** `f29fd1ea-d12f-47a4-a2a3-b664ea8b3de7`

**Additional test evidence:** TC-15 — Reopen a Lost case, Step 7 — Verify reminder coverage.

## Retest Result

Retest passed in Zephyr cycle **FUT-R2**.

FUT-13 was verified against both affected completion paths:

- Reopen from Done — TC-14: Pass
- Reopen from Lost — TC-15: Pass

In both cases, the reopened case returned to the correct active lifecycle state and reminder coverage was successfully re-armed for the existing `due_at`.

No recurrence of the original defect was observed.

## Jira Traceability

Related work items:

- FUT-6 — Reopen Follow-Up Item
- FUT-12 — Manage Follow-Up Reminder Lifecycle

Zephyr retest cycle:

- FUT-R2 — Follow-Up Lifecycle — FUT-13 Retest — Cycle 02

## Jira Attachments Recorded on FUT-13

- `BUG-reminder-not-rearmed-01-before-reopen.png`
- `BUG-reminder-not-rearmed-02-after-reopen.png`
- `Screenshot 2026-10-14 at 23.20.21.png`
- `Screenshot 2026-10-14 at 23.21.18.png`

These attachments remain part of the Jira defect record. This Markdown file preserves the defect content and test outcome in the GitHub portfolio without requiring Jira access.
