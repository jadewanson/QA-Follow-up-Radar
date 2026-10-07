# Static Review Findings

This file separates confirmed specification inconsistencies from runtime defect candidates.

## SR-01 — “Paid” described as a close outcome in high-level flow

**Type:** specification contradiction  
**Severity estimate:** High for requirement clarity

### Evidence

High-level product/user-flow wording describes closing a case as `paid/done/lost`.

The current Domain Model and State Machine explicitly define:

- payment and lifecycle as independent axes;
- `paid` is not a lifecycle status;
- marking paid does not close a case.

### Risk

Different developers/testers can implement or verify different expected behavior.

### QA action

Use the state-machine/domain-model semantics as the current test oracle and request correction of the high-level wording.

---

## SR-02 — Activation definition differs between documents

**Type:** specification inconsistency  
**Severity estimate:** Medium

Product Constitution requires reminder return plus close/reschedule for activation. User Flows lists activation as complete at first reminder-driven return.

### Risk

Activation analytics, acceptance and product experiments can use incompatible definitions.

### QA action

Choose one authoritative activation milestone sequence.

---

## SR-03 — Create flow omits a field required by the API contract

**Type:** incomplete requirement / traceability gap  
**Severity estimate:** Medium

The user-flow creation step lists type, due date, client, note and optional amount.

The implemented create schema requires `title` and `client_name` plus type and due date.

### Risk

A tester following only the flow document can design an incomplete happy path.

### QA action

Update the product flow or explicitly document how title is produced.

---

## SR-04 — “Under 2–3 minutes” is not one acceptance threshold

**Type:** ambiguous / weakly measurable acceptance criterion  
**Severity estimate:** Low-Medium

### Risk

A 2:30 result can be both pass and fail depending on interpretation.

### QA action

Define one maximum time and test conditions.

---

## SR-05 — Billing webhook quality-gate drift

**Type:** documentation drift  
**Severity estimate:** Medium

Testing Strategy requires integration testing of billing webhook verification, while current API Contracts explicitly keep real provider checkout/webhook semantics outside the implemented beta contract.

### Risk

Release gate and implemented scope disagree.

### QA action

Remove/defer the stale gate or add an authoritative requirement if webhook behavior is actually required.

---

## SR-06 — Whitespace-only required strings may bypass validation

**Type:** implementation defect candidate from static review  
**Status:** Not dynamically reproduced yet

### Evidence from code path

`FollowUpItemCreate.title` and `client_name` use `Field(min_length=1)`.

The service persists:

- `payload.title.strip()`
- `payload.client_name.strip()`

There is no field validator shown for stripping/rejecting whitespace-only title/client before the minimum-length check.

### Hypothesis

A payload containing `"   "` can satisfy raw length validation, then be normalized to an empty string and persisted.

### Required confirmation

Execute TC-12 against the running API and inspect both response and persisted row.

### Why it is not called a confirmed bug yet

The static path strongly suggests the problem, but this portfolio does not label a runtime failure as proven until it is reproduced.
