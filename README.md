# Junior QA Engineer Portfolio

This repository is an employer-facing QA portfolio built around a real training product, **Follow-Up Radar**.

The goal is not to collect theoretical notes. The goal is to show how I work with requirements, risk, test design, defects and evidence.

## Featured QA case study

### Follow-Up Item Lifecycle

A real product flow covering:

**Create → Schedule/Reschedule → Mark Paid → Complete → Reopen / Archive**

[Open the case study](case-studies/follow-up-lifecycle/README.md)

Included evidence:

- [Requirements review](case-studies/follow-up-lifecycle/requirements-review.md)
- [Risk-based test strategy](case-studies/follow-up-lifecycle/test-strategy.md)
- [QA checklist](case-studies/follow-up-lifecycle/checklist.md)
- [Detailed test cases](case-studies/follow-up-lifecycle/test-cases.md)
- [Static review findings](case-studies/follow-up-lifecycle/static-review-findings.md)
- [Test summary](case-studies/follow-up-lifecycle/test-summary.md)

## Skills demonstrated so far

- requirements analysis;
- ambiguity / contradiction detection;
- risk-based prioritization;
- positive / negative / boundary test design;
- lifecycle and state-transition reasoning;
- API contract analysis;
- authentication/ownership test design;
- evidence vs hypothesis discipline;
- residual-risk reporting;
- regression thinking.

## Evidence policy

I do not present planned tests as executed tests.

The current featured case has completed static analysis and test design. Runtime API/UI/DB execution will be added as separate evidence when performed.

## Product under test

[Follow-Up Radar](https://github.com/jadewanson/Follow-Up-Radar) — a training product for managing client follow-ups. It has a FastAPI backend, PostgreSQL domain state, Redis, a browser UI and a Chrome capture extension.

## Learning evidence

Formal QA fundamentals are kept as supporting material, not as the main portfolio surface:

- [CTFL 1.1–1.2 evidence](portfolio/evidence/ctfl/01-fundamentals/what-is-testing.md)
- [CTFL assessments](portfolio/assessments/ctfl/)

## Next portfolio evidence

1. Execute the lifecycle case against the running application.
2. Add Postman/API evidence.
3. Add PostgreSQL validation queries.
4. Add DevTools investigation evidence.
5. Convert reproduced failures into employer-quality bug reports.

---

**Current portfolio state:** one complete static QA case study, with dynamic execution intentionally still marked as pending.
