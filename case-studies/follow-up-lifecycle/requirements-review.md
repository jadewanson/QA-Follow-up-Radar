# Requirements Review — Follow-Up Item Lifecycle

## Review Goal

Check whether the current product documents provide one consistent and testable definition for the lifecycle scope selected for this case study.

## Current Scope

**Create → Active State → Reminder → Reschedule → Mark Paid → Complete (Done/Lost) → Archive → Reopen**

## Sources Reviewed

- Product Constitution
- Scope v1
- User Flows and Activation
- Domain Model
- State Machine
- API Contracts
- current executable create/lifecycle contract where supporting evidence was needed

## Findings

| ID | Type | Evidence | QA concern | Testing decision / clarification needed |
|---|---|---|---|---|
| RQ-01 | Contradiction | High-level product/user-flow wording describes closing a case as `paid/done/lost`. Domain Model and State Machine define payment and lifecycle as separate axes and explicitly state that `paid` does not close a case. | The same business action has conflicting expected behaviour in different documents. | Use the current Domain Model / State Machine as the lifecycle oracle. Record the documentation contradiction; do not treat `paid` as a terminal lifecycle state. |
| RQ-02 | Incomplete user flow | The create user flow lists type, due date, client, note and optional amount. The executable create contract requires `title`, `client_name`, type and `due_at`. | A tester following only the product flow can omit a field required by the current product contract. | Clarify whether `title` is explicitly user-entered, derived, or missing from the flow documentation. Until clarified, test the current UI behaviour but keep the documentation gap visible. |
| RQ-03 | Missing recovery requirement | State Machine defines `archived` as hidden/inactive and explicitly forbids Reopen from `archived`. No reviewed product document defines how a user recovers a case archived by mistake or later needed again. | The current forbidden transition is testable, but the intended product recovery path is undefined. | Test the documented rule: Reopen from `archived` must be rejected. Track the missing recovery path as a requirements/product decision rather than inventing one in QA. |
| RQ-04 | Incomplete payment applicability rule | State Machine says Mark Paid is allowed only when payment is applicable, but the reviewed product documentation does not define which item types are payment-applicable. Current implementation maps `invoice` and `renewal` to `unpaid` and other types to `not_applicable`. | The expected initial payment state and valid Mark Paid scenarios cannot be derived completely from product documentation alone. | Product documentation should define the item-type-to-payment-applicability mapping. Until then, distinguish documented behaviour from implementation observation in test evidence. |

## Requirements That Are Sufficiently Testable

The current lifecycle/state-machine documentation gives clear expected results for:

- temporal projection of active status;
- creating an active case and initial reminder;
- active-only rescheduling;
- payment/lifecycle separation;
- completion as `done` or `lost`;
- required completion reason;
- completion reminder cancellation and `next_action` clearing;
- reopening `done` and `lost`;
- reopen reminder re-arming;
- archive behaviour;
- forbidden lifecycle transitions, including Reopen from `archived`.

## Out-of-Scope Review Findings

Earlier review notes about activation analytics, the “under 2–3 minutes” UX acceptance wording and billing webhook quality gates are valid documentation topics, but they are outside this lifecycle test cycle and are not carried as blockers for this case study.

## QA Conclusion

Most of the selected lifecycle has a usable test oracle and can proceed to Test Design.

Where the requirement is incomplete or contradictory, QA records the gap and avoids inventing the business rule. A documentation finding blocks only the affected expected result, not unrelated lifecycle testing.
