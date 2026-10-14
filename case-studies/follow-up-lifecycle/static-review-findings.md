# Static Review Findings — Follow-Up Item Lifecycle

This file separates specification findings from implementation defect candidates for the current lifecycle scope.

## SR-01 — “Paid” described as a close outcome

**Type:** specification contradiction  
**Related requirement finding:** RQ-01  
**Priority:** High

High-level product/user-flow wording describes closing a case as `paid/done/lost`, while the Domain Model and State Machine explicitly define payment and lifecycle as independent axes.

**QA handling:** use the State Machine / Domain Model as the current lifecycle oracle and keep the documentation contradiction visible.

---

## SR-02 — Create flow omits required `title`

**Type:** incomplete requirement / traceability gap  
**Related requirement finding:** RQ-02  
**Priority:** Medium

The user-flow creation step does not mention `title`, while the current executable create schema requires it.

**QA handling:** do not silently rewrite the user flow. Test the current UI behaviour and keep the missing requirement visible.

---

## SR-03 — Archive recovery behaviour is undefined

**Type:** incomplete requirement / product-flow gap  
**Related requirement finding:** RQ-03  
**Priority:** Medium

The authoritative State Machine says:

- archive is allowed from any non-archived state;
- archived cases are hidden/inactive;
- Reopen is allowed only from `done` or `lost`;
- Reopen from `archived` is forbidden.

No reviewed product document defines a recovery path for an accidentally archived or later-needed case.

**QA handling:** test the current rule as written — Reopen from `archived` must fail — and keep the missing recovery path as a separate requirements finding.

---

## SR-04 — Payment applicability mapping is not documented

**Type:** incomplete requirement  
**Related requirement finding:** RQ-04  
**Priority:** High

The State Machine allows Mark Paid only when payment is applicable, but the reviewed product documents do not state which item types are payment-applicable.

The current implementation maps:

- `invoice`, `renewal` → `unpaid`;
- other item types → `not_applicable`.

This implementation observation does not replace the missing product requirement.

**QA handling:** distinguish documentation-derived expected results from implementation observations until the mapping is clarified.

---

## SR-05 — Whitespace-only required strings may bypass validation

**Type:** implementation defect candidate from static review  
**Status:** Not dynamically reproduced yet

### Evidence from code path

`FollowUpItemCreate.title` and `client_name` use `Field(min_length=1)`.

The service later persists:

- `payload.title.strip()`;
- `payload.client_name.strip()`.

A whitespace-only value may satisfy the raw length validation and then become an empty persisted string after normalization.

### Required confirmation

Execute a dynamic negative test against the running product and inspect the observable result.

### Classification rule

This remains a **defect candidate**, not a confirmed product bug, until runtime reproduction provides evidence.
