# Risk-Based Test Strategy — Follow-Up Item Lifecycle

## Objective

Provide evidence that the item lifecycle preserves the documented domain invariants across UI/API state changes and owner boundaries.

## In scope

- create follow-up item;
- temporal status projection;
- reschedule;
- mark paid;
- complete as done/lost;
- outcome reason;
- reopen;
- archive;
- reminder side effects;
- event history;
- authenticated ownership;
- persistence of lifecycle fields.

## Out of scope for this case study

- real SMTP deliverability;
- payment-provider checkout;
- billing provider webhooks;
- load/performance certification;
- cross-browser matrix;
- Chrome extension capture behavior.

## Risk priorities

| Priority | Risk | Why it matters |
|---|---|---|
| P0 | Cross-user access or mutation | Violates ownership/security boundary. |
| P0 | Forbidden lifecycle transition succeeds | Corrupts domain state. |
| P0 | Completion/reschedule/reopen leaves reminder coverage incorrect | Can create missed or duplicate follow-ups. |
| P0 | Payment state closes lifecycle or lifecycle changes payment incorrectly | Violates explicit two-axis model. |
| P0 | Persisted state differs from API/UI state after reload | User cannot trust the case state. |
| P1 | Outcome reason is missing/lost | Closed case becomes unauditable. |
| P1 | Temporal boundary produces wrong open/due_today/overdue state | Operational queue becomes misleading. |
| P1 | Archive still appears in active registry | Hidden/inactive semantics fail. |
| P1 | Event history does not match state mutations | Investigation and review evidence becomes unreliable. |
| P2 | Presentation wording or layout is inconsistent | Lower impact if state remains correct. |

## Test approach

### 1. Static testing

Review requirements for ambiguity, contradiction and missing expected results before execution.

### 2. API contract testing

Exercise lifecycle endpoints directly:

- `POST /items`
- `PATCH /items/{id}`
- `POST /items/{id}/reschedule`
- `POST /items/{id}/mark-paid`
- `POST /items/{id}/complete`
- `POST /items/{id}/reopen`
- `DELETE /items/{id}`
- `GET /items/{id}`
- `GET /items/{id}/events`

### 3. UI workflow testing

Verify that the browser exposes only actions valid for the current lifecycle state and that a reload reflects persisted state.

### 4. Data validation

For executed cases, compare API/UI observations with PostgreSQL state for:

- `follow_up_items`;
- reminders;
- event log.

### 5. Negative testing

Prioritize:

- terminal mutations;
- missing completion reason;
- mark-paid on non-payment types;
- repeated archive;
- reopen from active/archived;
- unauthenticated mutation;
- cross-user access.

### 6. Boundary testing

Focus on:

- due date around UTC date change;
- required strings at empty/whitespace-only boundary;
- title/client length limits;
- `next_action` and outcome reason length limits.

## Entry criteria

- test environment starts successfully;
- migrations applied;
- two independent users available;
- API reachable;
- database available for read-only validation.

## Exit criteria for portfolio evidence

- P0 lifecycle scenarios executed;
- evidence captured for API response and persisted state;
- ownership checks executed with two users;
- at least one end-to-end state flow executed;
- all observed defects documented;
- residual risks stated explicitly;
- no planned-but-unexecuted case presented as passed.
