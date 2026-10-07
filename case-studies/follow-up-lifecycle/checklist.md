# QA Checklist — Follow-Up Item Lifecycle

Status legend for future execution:

- `[ ]` not executed
- `[x]` executed and passed
- `[!]` executed and failed / defect raised

## Create

- [ ] Create a valid invoice item.
- [ ] Create a valid proposal/lead/promised-reply/renewal item.
- [ ] Verify invoice/renewal default to `unpaid`.
- [ ] Verify non-payment item types default to `not_applicable`.
- [ ] Verify initial lifecycle state from `due_at`.
- [ ] Verify initial reminder is created.
- [ ] Verify item-created event exists.
- [ ] Reject empty title.
- [ ] Reject whitespace-only title.
- [ ] Reject empty client name.
- [ ] Reject whitespace-only client name.
- [ ] Validate title/client maximum length.
- [ ] Validate `source_url` accepts only absolute HTTP(S).
- [ ] Validate currency normalization.

## Temporal status

- [ ] Future UTC date → `open`.
- [ ] Current UTC date → `due_today`.
- [ ] Past UTC date → `overdue`.
- [ ] Terminal state remains terminal regardless of due date.

## Reschedule

- [ ] Reschedule `open` item.
- [ ] Reschedule `due_today` item.
- [ ] Reschedule `overdue` item.
- [ ] Verify status is re-projected after reschedule.
- [ ] Verify reminder coverage is updated.
- [ ] Verify reschedule event is recorded.
- [ ] Reject reschedule for `done`.
- [ ] Reject reschedule for `lost`.
- [ ] Reject reschedule for `archived`.

## Payment state

- [ ] Mark active invoice as paid.
- [ ] Verify lifecycle status does not change after mark-paid.
- [ ] Reject mark-paid for `not_applicable`.
- [ ] Reject repeated mark-paid.
- [ ] Reject mark-paid for terminal lifecycle state.

## Completion

- [ ] Complete active item as `done` with reason.
- [ ] Complete active item as `lost` with reason.
- [ ] Reject completion without non-empty reason.
- [ ] Verify `completed_at` is set.
- [ ] Verify `next_action` is cleared.
- [ ] Verify pending reminders are canceled.
- [ ] Verify outcome reason persists after reload.
- [ ] Reject second completion of terminal item.

## Reopen

- [ ] Reopen `done`.
- [ ] Reopen `lost`.
- [ ] Verify `completed_at` clears.
- [ ] Verify lifecycle is projected from existing `due_at`.
- [ ] Verify reminder coverage is re-armed.
- [ ] Verify previous `next_action` is not restored.
- [ ] Reject reopen from `open`, `due_today`, `overdue`, `archived`.

## Archive

- [ ] Archive active item.
- [ ] Archive done/lost item.
- [ ] Verify pending reminders are canceled.
- [ ] Verify archived item is hidden from normal registry/list.
- [ ] Reject archiving an already archived item.

## Ownership / auth

- [ ] Unauthenticated create is rejected.
- [ ] User B cannot read User A item.
- [ ] User B cannot update User A item.
- [ ] User B cannot reschedule User A item.
- [ ] User B cannot complete User A item.
- [ ] User B cannot archive User A item.
- [ ] User B cannot read User A item events.

## Persistence / evidence

- [ ] Reload UI after each mutation and compare state.
- [ ] GET item after each mutation and compare fields.
- [ ] Check corresponding event history.
- [ ] Check PostgreSQL row after selected P0 scenarios.
- [ ] Check reminder row after create/reschedule/complete/reopen/archive.
