# Test Analysis — Follow-Up Item Lifecycle

## Analysis Scope

**Create → Active State → Reminder → Reschedule → Mark Paid → Complete (Done/Lost) → Archive → Reopen**

The sequence above is the coverage scope, not one mandatory valid state path. Allowed and forbidden transitions are derived from the current authoritative lifecycle rules.

## Test Basis

Primary product sources:

- `docs/03_scope_v1.md`
- `docs/04_user_flows_and_activation.md`
- `docs/05_domain_model.md`
- `docs/06_state_machine.md`
- `docs/08_api_contracts.md`

Implementation reviewed as supporting evidence where relevant:

- `backend/app/schemas/followup_item.py`
- `backend/app/services/item_service.py`
- `backend/app/contracts/enums.py`

Future-planned functionality explicitly marked as not implemented is excluded.

## Test Conditions

| ID | Area | Test condition | Priority | Test-basis status |
|---|---|---|---|---|
| TCND-C01 | Create | A user can create a valid follow-up item and the item is persisted as a case. | P0 | Clear |
| TCND-C02 | Create | Creation assigns an active lifecycle state according to `due_at` and the current UTC date. | P0 | Clear |
| TCND-C03 | Create | Creation schedules the initial reminder in the same authoritative flow. | P0 | Clear |
| TCND-C04 | Create | Required creation fields exposed by the current product are validated before a case is created. | P0 | Clarification needed: user flow omits `title` |
| TCND-C05 | Create / Payment | Initial payment state is correct for the selected item type. | P0 | Blocked pending product definition of item-type-to-payment applicability |
| TCND-A01 | Active State | A case due after the current UTC date is represented as `open`. | P0 | Clear |
| TCND-A02 | Active State | A case due on the current UTC date is represented as `due_today`. | P0 | Clear |
| TCND-A03 | Active State | A case due before the current UTC date is represented as `overdue`. | P0 | Clear |
| TCND-A04 | Active State | Terminal lifecycle states are not replaced by temporal projection. | P0 | Clear |
| TCND-R01 | Reminder | The initial reminder corresponds to the created case and its due time. | P0 | Clear |
| TCND-R02 | Reminder | At the due time, the reminder is delivered through the configured product flow. | P0 | Clear |
| TCND-R03 | Reminder | The reminder provides a usable return path to the product/case flow. | P1 | Clear at product-flow level |
| TCND-R04 | Reminder | Lifecycle actions maintain the documented reminder behaviour: reschedule updates/recreates coverage, completion/archive cancel pending reminders, and reopen re-arms coverage. | P0 | Clear |
| TCND-S01 | Reschedule | Reschedule is allowed for active cases. | P0 | Clear |
| TCND-S02 | Reschedule | Reschedule changes `due_at` and recomputes the active lifecycle state. | P0 | Clear |
| TCND-S03 | Reschedule | Reschedule updates or recreates pending reminder coverage for the new due time. | P0 | Clear |
| TCND-S04 | Reschedule | Reschedule is rejected for `done`, `lost` and `archived` cases without changing the case state. | P0 | Clear |
| TCND-P01 | Mark Paid | Mark Paid is available only when payment is applicable and the case is mutable. | P0 | Rule clear; item-type applicability mapping unresolved |
| TCND-P02 | Mark Paid | Mark Paid changes payment state without changing lifecycle state. | P0 | Clear |
| TCND-P03 | Mark Paid | Mark Paid is rejected when payment is not applicable. | P0 | Rule clear; item-type applicability mapping unresolved |
| TCND-CM01 | Complete | An active case can be completed as `done`. | P0 | Clear |
| TCND-CM02 | Complete | An active case can be completed as `lost`. | P0 | Clear |
| TCND-CM03 | Complete | A non-empty outcome reason is required for both `done` and `lost`. | P0 | Clear |
| TCND-CM04 | Complete | Completion records the terminal outcome and completion information consistently. | P1 | Clear from domain/state model |
| TCND-CM05 | Complete | Completion clears `next_action` and cancels pending reminders. | P0 | Clear |
| TCND-CM06 | Complete | Completing an already terminal case is rejected without changing persisted state. | P0 | Clear |
| TCND-AR01 | Archive | Archive is allowed from any non-archived lifecycle state. | P0 | Clear |
| TCND-AR02 | Archive | An archived case becomes hidden/inactive and is represented as `archived`. | P0 | Clear |
| TCND-AR03 | Archive | Archiving cancels pending reminders. | P0 | Clear |
| TCND-AR04 | Archive | Archiving an already archived case is rejected. | P1 | Clear |
| TCND-AR05 | Archive / Reopen | Reopening an archived case is rejected under the current authoritative state-machine rule. | P0 | Clear current rule; recovery rationale/path is undefined |
| TCND-RO01 | Reopen | A `done` or `lost` case can be reopened. | P0 | Clear |
| TCND-RO02 | Reopen | Reopen clears `completed_at` and projects the new active state from the existing `due_at`. | P0 | Clear |
| TCND-RO03 | Reopen | Reopen does not restore the previous `next_action`. | P1 | Clear |
| TCND-RO04 | Reopen | Reopen re-arms reminder coverage. | P0 | Clear |
| TCND-RO05 | Reopen | Reopen is rejected from `open`, `due_today`, `overdue` and `archived`. | P0 | Clear |

## Requirements Affecting Testability

- **TCND-C04** — the user flow does not include `title`, while the executable create contract requires it.
- **TCND-C05** — cannot receive a complete documentation-derived expected result until the product defines which item types are payment-applicable.
- **TCND-P01 / TCND-P03** — the action rules are testable when the case already exposes `unpaid` or `not_applicable`, but the item-type mapping remains unresolved.
- **TCND-AR05** — the current rule forbids reopening archived cases and is testable; the product documentation does not define a recovery path for accidental archive.

Detailed requirement findings are maintained in [requirements-review.md](requirements-review.md).

## Analysis Conclusion

Most lifecycle conditions have a defined test oracle and are ready for test design.

Open documentation findings affect only the conditions that depend on the missing or contradictory requirement.
