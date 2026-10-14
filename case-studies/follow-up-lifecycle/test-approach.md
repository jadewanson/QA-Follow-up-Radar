# Test Approach — Follow-Up Item Lifecycle

## Context

Follow-Up Radar is a training product used as the system under test. The QA work in this case study follows a professional manual testing process.

## Test Objective

The goal of this test cycle is to verify that a user can work with a follow-up case through its documented lifecycle and that the application preserves the expected state and behaviour at each step.

## Scope

The following lifecycle functionality is included:

**Create → Active State → Reminder → Reschedule → Mark Paid → Complete (Done/Lost) → Archive → Reopen**

These are the product capabilities covered by the test cycle. They are not treated as one mandatory linear path: allowed transitions and restrictions are verified against the documented product rules.

## Out of Scope

This test cycle does not attempt to validate the whole Follow-Up Radar product.

The following areas are outside the current scope:

- authentication and account-management flows except where an authenticated user is required to execute lifecycle tests;
- billing, plan limits, trials and payment-provider behaviour;
- daily digest and message-template features;
- Chrome extension capture behaviour;
- load, stress and performance testing;
- security or penetration testing beyond functional access restrictions encountered in the selected lifecycle;
- a full browser, device or operating-system compatibility matrix;
- automated unit, integration or end-to-end test implementation.

If an out-of-scope area blocks or materially affects a lifecycle scenario, the dependency will be recorded in the execution evidence.

## Test Level

The primary test level is **System Testing**.

The product will be tested as a complete running application from the user's point of view rather than as separate code components.

## Test Type

The main test type is **Functional Testing**.

The focus is on whether the lifecycle behaviour available to the user matches the documented expected behaviour.

## Testing Approach

Testing will be primarily:

- manual;
- black-box;
- user-focused;
- risk-based.

The main test execution will be performed through the web interface.

Browser DevTools, API observations or read-only state checks may be used when they help investigate or confirm behaviour, but they are supporting tools rather than the main focus of this case study.

No test automation is included in this test cycle.

## Static Testing

Product documentation will be reviewed before dynamic testing.

The review will look for:

- contradictions between documents;
- ambiguous requirements;
- missing expected results;
- incomplete lifecycle rules;
- requirements that cannot be tested reliably as written.

If the documentation does not provide a clear expected result, the behaviour will not be invented by QA. The problem will be recorded as a requirements or documentation finding.

## Test Design Techniques

Test techniques will be selected according to the behaviour being tested.

### State Transition Testing

This is the main technique for lifecycle testing.

It will be used to verify:

- valid state transitions;
- invalid or forbidden transitions;
- behaviour after completion;
- reopen behaviour;
- archive behaviour;
- state changes caused by time or user actions.

### Equivalence Partitioning

Used where input data can be divided into meaningful valid and invalid groups.

### Boundary Value Analysis

Used where behaviour depends on boundaries such as dates, times, text lengths or other constrained values.

### Decision Table Testing

Used where the expected behaviour depends on combinations of several conditions.

### Exploratory Testing

Used after or alongside designed tests to investigate unexpected behaviour and areas where predefined test cases may not provide enough coverage.

## Risk-Based Prioritisation

Higher-risk behaviour will be tested first.

The highest attention will be given to failures that could:

- leave a case in an incorrect lifecycle state;
- lose or incorrectly schedule a reminder;
- incorrectly connect payment state with lifecycle state;
- allow an invalid lifecycle transition;
- lose or corrupt persisted case information;
- show a different state after reload or subsequent actions.

Lower-impact presentation issues will receive lower priority unless they prevent the user from completing the workflow.

## Expected Results and Test Oracle

Expected behaviour will be taken from the current authoritative product documentation.

Where documents disagree, the conflict will be recorded as a requirements finding.

QA will not redefine business behaviour in order to make a test pass or fail.

A functional defect will be reported only when observed behaviour contradicts a sufficiently defined expected result.

## Entry Criteria

Dynamic test execution can start when:

- the selected lifecycle test basis is available for review;
- the current test build can be started in the agreed QA environment;
- the web application is reachable;
- required backend services and dependencies are available for the tested flow;
- the tester can authenticate and create the test data needed for execution;
- planned lifecycle tests are available in the test management system;
- known environment limitations that could affect execution are documented.

If an entry criterion is not met, the affected tests may be recorded as **Blocked** rather than forced to a pass/fail result.

## Exit Criteria

The test cycle can be closed when:

- all planned P0 lifecycle tests have been executed or have a documented blocking reason;
- remaining planned tests have an explicit execution status;
- observed product defects are recorded with reproducible evidence;
- requirements/documentation findings affecting the scope are recorded separately;
- fixes delivered during the cycle have been retested where applicable;
- targeted regression has been performed where a change could affect previously tested lifecycle behaviour;
- unresolved defects, blocked tests and residual risks are clearly stated in the test completion report;
- no unexecuted test is presented as passed.

Meeting the exit criteria means the test cycle has enough evidence to conclude. It does not mean the product is defect-free or automatically ready for release.

## Defect Handling

Two types of findings will be kept separate:

**Product defects** — the implemented product behaves differently from a sufficiently defined requirement.

**Requirements/documentation findings** — the expected behaviour itself is unclear, incomplete or contradictory.

Reported defects will include enough information to reproduce and understand the problem.

After a defect is fixed, the affected behaviour will be retested and appropriate regression testing will be performed.

## Test Evidence

Execution evidence may include:

- Zephyr test executions and statuses;
- screenshots;
- observed UI behaviour;
- relevant DevTools or request/response evidence;
- Jira defect reports;
- retest and regression results.

Only tests that were actually executed will be reported as passed.
