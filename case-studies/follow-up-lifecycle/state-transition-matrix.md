# State Transition Matrix — Follow-Up Item Lifecycle

## Lifecycle States

- `open`
- `due_today`
- `overdue`
- `done`
- `lost`
- `archived`

## User-Triggered Operations

| Current state | Reschedule | Mark Paid | Complete: Done | Complete: Lost | Archive | Reopen |
|---|---|---|---|---|---|---|
| `open` | Allowed → active state projected from new `due_at` | Conditional; lifecycle remains `open` | Allowed → `done` | Allowed → `lost` | Allowed → `archived` | Forbidden |
| `due_today` | Allowed → active state projected from new `due_at` | Conditional; lifecycle remains `due_today` | Allowed → `done` | Allowed → `lost` | Allowed → `archived` | Forbidden |
| `overdue` | Allowed → active state projected from new `due_at` | Conditional; lifecycle remains `overdue` | Allowed → `done` | Allowed → `lost` | Allowed → `archived` | Forbidden |
| `done` | Forbidden | Forbidden | Forbidden | Forbidden | Allowed → `archived` | Allowed → active state projected from existing `due_at` |
| `lost` | Forbidden | Forbidden | Forbidden | Forbidden | Allowed → `archived` | Allowed → active state projected from existing `due_at` |
| `archived` | Forbidden | Forbidden | Forbidden | Forbidden | Forbidden | Forbidden |

### Mark Paid condition

Mark Paid is permitted only when the case is mutable and payment is applicable.

A successful Mark Paid operation changes `payment_status` but does not change lifecycle status.

The product documentation does not currently define the item-type-to-payment-applicability mapping. This remains tracked as a requirements finding.

## Temporal Projection

For active cases, lifecycle state is determined from `due_at` against the current UTC date:

| Due date relative to current UTC date | Expected lifecycle state |
|---|---|
| Future date | `open` |
| Current date | `due_today` |
| Past date | `overdue` |

Terminal states are not replaced by temporal projection:

| Current terminal state | Time passes / due date becomes past | Expected lifecycle state |
|---|---|---|
| `done` | Yes | `done` |
| `lost` | Yes | `lost` |
| `archived` | Yes | `archived` |

## Transition Side Effects

| Operation | Required side effect |
|---|---|
| Create | Initial reminder is scheduled |
| Reschedule | Pending reminder coverage is updated or recreated for the new due time |
| Mark Paid | Lifecycle state remains unchanged |
| Complete | `completed_at` is set, `next_action` is cleared, pending reminders are canceled |
| Archive | Pending reminders are canceled |
| Reopen | `completed_at` is cleared, prior `next_action` is not restored, reminder coverage is re-armed |

## Test Condition Coverage

| Matrix area | Covered test conditions |
|---|---|
| Active temporal projection | TCND-A01, TCND-A02, TCND-A03, TCND-A04 |
| Reschedule transitions | TCND-S01, TCND-S02, TCND-S03, TCND-S04 |
| Payment/lifecycle independence | TCND-C05, TCND-P01, TCND-P02, TCND-P03 |
| Completion transitions | TCND-CM01, TCND-CM02, TCND-CM03, TCND-CM04, TCND-CM05, TCND-CM06 |
| Archive transitions | TCND-AR01, TCND-AR02, TCND-AR03, TCND-AR04, TCND-AR05 |
| Reopen transitions | TCND-RO01, TCND-RO02, TCND-RO03, TCND-RO04, TCND-RO05 |

## Manual Execution Scope

The transition coverage in this matrix is intended for manual execution through the product UI.

Supporting manual observations may use browser DevTools, API inspection or read-only persisted-state checks when the UI does not expose enough evidence to confirm the result.
