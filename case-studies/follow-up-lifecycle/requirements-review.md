# Requirements Review — Follow-Up Item Lifecycle

## Review goal

Check whether the current product documents provide one consistent and testable definition of the lifecycle before dynamic testing.

## Sources reviewed

- Product Constitution
- Scope v1
- User Flows and Activation
- Domain Model
- State Machine
- API Contracts
- Testing Strategy and Quality Gates

## Findings

| ID | Type | Evidence | QA concern | Clarification / action |
|---|---|---|---|---|
| RQ-01 | Contradiction | Product Constitution and User Flow describe closing a case as “paid/done/lost”. Domain Model and State Machine explicitly state that `paid` is a separate payment state and does not close a case. | Two different expected results exist for the same business action. | Treat Domain Model / State Machine as current authority for testing, and correct the high-level product wording to separate “mark paid” from “close done/lost”. |
| RQ-02 | Inconsistent activation definition | Product Constitution defines activation through reminder return **and close/reschedule**. User Flows activation path ends at first reminder-driven return. | Product analytics and acceptance can classify the same user differently. | Define one authoritative activation event sequence. |
| RQ-03 | Incomplete user flow | Create flow lists type, due date, client, note and optional amount. The API create contract also requires `title`. | A manual test based only on the user flow can omit a field required by the implementation contract. | Add `title` to the authoritative creation flow or explicitly define it as derived. |
| RQ-04 | Ambiguous acceptance threshold | UX acceptance says a new user should create the first item in “under 2–3 minutes”. | There is no single pass/fail threshold: <2 min and <3 min are different criteria. | Replace the range with one measurable maximum and define test conditions. |
| RQ-05 | Documentation drift | Testing Strategy requires integration coverage for “billing webhook verification”. API Contracts state that real payment-provider checkout/webhook semantics are intentionally outside the current contract. | Quality gate may require testing functionality that the current product explicitly says is not implemented. | Align the testing strategy with the current beta billing scope. |

## Requirements that are sufficiently testable

The lifecycle/state-machine document gives clear expected results for:

- temporal projection of active state;
- allowed/forbidden terminal transitions;
- rescheduling active cases;
- payment/lifecycle separation;
- required outcome reason;
- reopening done/lost cases;
- archiving;
- reminder cancellation/re-arming side effects;
- ownership enforced server-side.

## Open questions before full certification

1. Which activation definition is authoritative?
2. What is the exact first-item creation time SLA?
3. Should high-level “close as paid” language be removed everywhere?
4. Is webhook verification intentionally deferred or still part of a release gate?

## QA conclusion

The lifecycle itself is mostly well specified in the domain/state-machine documents, but the surrounding product documentation contains enough drift that testing should not rely on one document in isolation.
