# Topic 02 Assessment — CTFL 1.3 Seven Testing Principles

> **Purpose:** internal QA Portfolio assessment for topic 02. This is **not** an official ISTQB exam or certificate.
>
> **ISTQB learning objective:** FL-1.3.1 (K2) — explain the seven testing principles.

## Principles in scope

1. Testing shows the presence, not the absence of defects.
2. Exhaustive testing is impossible.
3. Early testing saves time and money.
4. Defects cluster together.
5. Tests wear out.
6. Testing is context dependent.
7. Absence-of-defects fallacy.

---

## Assessment A — Knowledge check

Answer without copying the syllabus. Each answer must be 2–5 sentences and include the practical consequence for a tester.

1. Why does passing every executed test not prove that the product has no defects?
2. Why is “test every possible input and combination” normally not a viable strategy?
3. Explain how testing earlier in the lifecycle can reduce later rework and cost.
4. What does defect clustering mean, and how should it affect test focus?
5. Why can a stable regression suite become less effective at discovering new defects over time?
6. Give two different product contexts where the appropriate test approach would differ, and explain why.
7. How can a system satisfy its written requirements and still fail as a product?

### Evidence
Save the answers in:
`portfolio/evidence/ctfl/01-fundamentals/topic-02-knowledge-check.md`

---

## Assessment B — Scenario identification

For each scenario:
- name the **single primary testing principle**;
- explain the evidence in the scenario;
- state one concrete QA action that follows from the principle.

### B1
A team executed 1,200 regression checks. All passed. The release manager concludes that the new version is “defect-free”.

### B2
A registration field accepts an 8-character code using uppercase letters and digits. A tester proposes executing every possible valid and invalid string combination.

### B3
A requirement says only: “The reminder should be sent at the correct time.” During implementation the team discovers that timezone, daylight-saving transitions and retries were never defined.

### B4
During the last five releases, most production incidents came from notification scheduling and retry logic, while the profile settings area produced almost none.

### B5
For six months the same smoke and regression checks have been executed unchanged. They still pass, but customers keep reporting failures in newly introduced workflows.

### B6
The same QA checklist is proposed unchanged for a medical alert system and for a low-risk internal demo dashboard.

### B7
A feature exactly matches every acceptance criterion, but users cannot complete their real business task efficiently and abandon the workflow.

### Evidence
Save the answers in:
`portfolio/evidence/ctfl/01-fundamentals/topic-02-scenario-analysis.md`

---

## Assessment C — Follow-up Radar application

Use the actual Follow-up Radar product. For **each of the seven principles**, document:

1. a concrete product example;
2. a bad QA decision that violates the principle;
3. the corrected QA decision;
4. the evidence or observation that supports the decision;
5. the remaining uncertainty or risk.

Use this table:

| Principle | Product example | Wrong QA decision | Corrected QA decision | Evidence | Residual risk |
|---|---|---|---|---|---|

### Constraints
- The seven examples must not be seven paraphrases of the same situation.
- At least three different product areas must be represented.
- At least two examples must concern pre-execution activities such as requirements/review/risk planning, not only running tests.
- Do not claim a defect, root cause, or risk as fact without evidence.

### Evidence
Save the completed table in:
`portfolio/evidence/ctfl/01-fundamentals/testing-principles.md`

---

## Assessment D — Certification case

### Case
Follow-up Radar is preparing a release that introduces:
- scheduled email follow-ups;
- retry after transient delivery failure;
- per-user timezone support;
- a redesigned dashboard.

You have limited time before release. The team says:
- 430 existing regression checks pass;
- most historical defects were in scheduling/retry;
- the timezone requirement was clarified only after development began;
- the dashboard has many new interaction paths;
- product management says that all written acceptance criteria are now satisfied.

### Task
Prepare a concise QA decision memo containing:

1. **At least five** of the seven testing principles that materially apply to this release.
2. For each selected principle:
   - the relevant fact from the case;
   - the risk or misconception;
   - the QA action you would take.
3. Explain why the 430 passing checks do **not** justify the statement “the release is defect-free”.
4. Explain where you would spend additional testing effort and why.
5. Name one way the existing regression suite should be changed.
6. Explain what must still be validated even if all acceptance criteria pass.

### Evidence
Save the memo in:
`portfolio/evidence/ctfl/01-fundamentals/topic-02-certification-case.md`

---

## Oral defense

After the written work, answer these without notes:

1. Name all seven principles.
2. Explain any three chosen at random using a new example not present in your written work.
3. Distinguish:
   - “testing shows presence, not absence of defects”
   - “absence-of-defects fallacy”
4. Explain why “tests wear out” does not mean regression tests should simply be discarded.
5. Give one example where context changes the test approach.

The oral defense should be recorded only as a result and short assessor note; a transcript is optional.

---

## Internal scoring rubric

This is a **portfolio gate**, not an ISTQB grading rule.

| Section | Points |
|---|---:|
| A — Knowledge check | 20 |
| B — Scenario identification | 25 |
| C — Follow-up Radar application | 30 |
| D — Certification case | 20 |
| Oral defense | 5 |
| **Total** | **100** |

### Pass criteria

Topic 02 is certified as complete only when all conditions are true:

- total score is **85/100 or higher**;
- all seven principles are explained correctly at least once;
- scenario section has no more than one principle-identification error;
- Follow-up Radar evidence covers all seven principles;
- certification case contains evidence-based actions, not only definitions;
- oral defense demonstrates the difference between principle 1 and principle 7;
- no unsupported claim is presented as a proven fact.

If any mandatory condition fails, status remains `Practicing` even if the numeric score is high.

---

## Completion record

When passed, record in the topic Issue:

- assessment date;
- score;
- assessor;
- links to A/B/C/D evidence;
- oral defense result;
- remaining weak points, if any.

Only then may the topic move to:
- `Evidence = Ready`
- `Interview = Ready`
- `Status = Interview Ready`
