# Detailed Test Cases — Follow-Up Item Lifecycle

These are designed cases, not execution results.

| ID | Scenario | Preconditions | Action | Expected result | Priority |
|---|---|---|---|---|---|
| TC-01 | Create valid invoice | Authenticated user | POST valid invoice with future `due_at` | 201; lifecycle `open`; payment `unpaid`; initial reminder exists | P0 |
| TC-02 | Payment/lifecycle independence | Active unpaid invoice | Mark paid | payment becomes `paid`; lifecycle remains unchanged | P0 |
| TC-03 | Mark paid on non-payment type | Active proposal/lead with `not_applicable` | Mark paid | Explicit domain error; state unchanged | P0 |
| TC-04 | Complete as done | Active item with `next_action` and pending reminder | Complete with outcome `done` and reason | status `done`; reason stored; `completed_at` set; `next_action` cleared; pending reminder canceled | P0 |
| TC-05 | Completion without reason | Active item | Complete as done/lost with empty/whitespace reason | Request rejected; item remains active | P0 |
| TC-06 | Reschedule overdue case | Owned overdue item | Reschedule to future date | lifecycle becomes `open`; due date changes; reminder coverage updated; event recorded | P0 |
| TC-07 | Forbidden mutation of terminal case | Done/lost item | Reschedule or normal update | Explicit state-transition error; persisted state unchanged | P0 |
| TC-08 | Reopen done case | Done case with future/past/current due date | Reopen | `completed_at` cleared; lifecycle projected from due date; reminder coverage re-armed; previous next action not restored | P0 |
| TC-09 | Archive | Non-archived owned item | DELETE item | 204; item archived; pending reminders canceled; item absent from normal registry | P0 |
| TC-10 | Cross-user access | User A owns item; User B authenticated | User B GET/PATCH/complete/archive User A item | No data disclosure or mutation; owner boundary enforced | P0 |
| TC-11 | UTC temporal boundary | Authenticated user | Create/reschedule items around previous/current/next UTC date | previous → `overdue`; current → `due_today`; next → `open` | P1 |
| TC-12 | Required text after normalization | Authenticated user | Create with title/client containing only spaces | Request must be rejected as invalid required text | P1 |
| TC-13 | Outcome persistence | Active item | Complete, reload UI, GET item again | same terminal state and reason remain visible after reload | P1 |
| TC-14 | Event traceability | Item goes through create → reschedule → complete | GET item events | history contains corresponding domain events in correct item/owner scope | P1 |

## TC-12 note

Static review found a code-path risk: the Pydantic schema checks `min_length=1` before the service later calls `.strip()`. A whitespace-only string may therefore pass schema validation and become an empty persisted value.

This is a **defect candidate**, not a claimed runtime failure, until executed.
