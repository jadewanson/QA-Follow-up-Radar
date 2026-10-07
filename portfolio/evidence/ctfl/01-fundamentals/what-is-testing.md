# Evidence — CTFL 1.1–1.2: What Is Testing & Why It Is Necessary

- **Issue:** #1 — 01 · CTFL 1.1–1.2 — What Is Testing & Why It Is Necessary
- **Assessment date:** 2026-10-07
- **Level:** Junior
- **Result:** PASS — Interview Ready
- **Assessment mode:** guided practice followed by a no-hints gate and a focused terminology re-check

## What was demonstrated

The learner can explain, in their own words, the practical difference between testing and debugging:

- **Testing** evaluates a work product or running system against expectations, requirements, user needs, or other test basis and provides information about quality and risk.
- **Debugging** localizes, analyzes, and removes the cause of a failure/defect.
- A retest after a fix is testing again, not debugging.

The learner also distinguishes **testing** from **Quality Assurance**:

- testing is product/work-product oriented;
- QA is process oriented and aims to improve how quality is built and maintained.

## Terminology demonstrated

The final assessment established the following distinctions:

- **Error** — a human mistake, wrong decision, action, or assumption. It is not limited to developers.
- **Defect** — an imperfection in a work product such as a requirement, design, code, testware, or configuration. It may exist without having produced an observed failure yet.
- **Failure** — observed behavior in which the executing system/component does not perform as expected or required.
- **Root cause** — a deeper underlying reason for the problem; it must be established by analysis rather than guessed from the nearest visible symptom.

A causal example produced during the gate was normalized to the CTFL relationship:

> Process does not define which timezone the project must use → a person assumes UTC → implementation is built around that assumption → a reminder is delivered one hour later than expected.

This demonstrates the distinction between a deeper process cause, a human error, a defect in the work product, and an observed failure.

## Test objectives demonstrated

The learner independently named valid testing objectives including:

1. provide information about the product to support decisions;
2. reduce uncertainty and product/quality risk;
3. find problems introduced during development;
4. check conformance with requirements and applicable rules;
5. check that the product behaves as expected for its intended use.

The learner also understands that successful execution of performed tests does **not** prove that no defects remain, because exhaustive testing is generally impossible except for trivial cases.

## Scenario evidence

| Scenario | Concept | Rationale demonstrated |
| --- | --- | --- |
| Password requires at least 8 characters; a 7-character password is accepted | Testing vs debugging | Tester exercises the boundary and records the observed failure. Developer investigates and fixes the implementation. Tester then retests. |
| UI shows `Saved`, but after reload the old value appears | Evidence vs hypothesis | Proven facts are the `Saved` message and the old value after reload. “The data was not persisted” remains a hypothesis until Network/backend/storage evidence is inspected. |
| Requirement says a report must be generated “quickly” | Static testing / testability | The word “quickly” does not provide a measurable expected result. QA should first obtain an agreed measurable criterion instead of inventing one. |
| A requirement allows 100 characters while another rejects exactly 100 | Contradictory test basis | The boundary has two incompatible expected results and must be clarified before the complete requirement can be tested. |
| “Last 10 operations” without an authoritative timestamp | Ambiguity | “Last” can depend on creation, execution, completion, or update time; QA should not silently choose one interpretation. |

## Two explicit testing/debugging separations

### Example 1 — password boundary

**Testing:** enter a 7-character password when the minimum is 8 and compare the observed result with the requirement.

**Debugging:** inspect implementation and supporting evidence to determine why an invalid password is accepted, then remove the cause.

### Example 2 — save/reload behavior

**Testing:** change a value, save it, reload the object, and observe whether the reloaded state matches the saved state.

**Debugging:** inspect Network traffic, backend behavior, storage, logs, and/or code to localize why the state observed after reload is wrong.

## Requirement-quality reasoning demonstrated

The learner can distinguish:

- a testable requirement from a merely imaginable broader set of requirements;
- an incomplete requirement from an ambiguous one;
- a contradiction from a missing criterion;
- an observable failure from an unsupported explanation of its cause.

Examples correctly handled during practice included:

- measurable boundaries such as password length and file size/type;
- vague words such as “quickly”, “correctly”, and “temporarily”;
- contradictory boundaries;
- combined constraints that form a normal logical AND;
- cases where the expected result must be clarified rather than invented by QA.

## Final gate result

The no-hints gate covered:

- testing vs debugging;
- testing vs QA;
- error / defect / failure / root cause;
- at least five test objectives;
- testing's contribution to project success;
- incomplete/ambiguous requirements;
- evidence vs unsupported causal hypotheses.

The learner initially used two imprecise formulations in the terminology block:

- treated `error` as developer-only;
- described a defect as if it must already have caused a failure.

After a focused re-check, both distinctions were corrected independently:

- an error can be made by any person involved in the lifecycle;
- a defect can remain latent and need not already have produced a failure.

**Conclusion:** the completion boundary for Issue #1 is met at the Junior / Interview Ready level.
