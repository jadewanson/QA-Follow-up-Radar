# Junior Manual QA Portfolio

Employer-facing manual QA portfolio focused on requirements analysis, risk-based test design, manual execution, defect reporting, retest and regression.

The current case study uses **Follow-Up Radar** as the system under test. Follow-Up Radar is a training product; the QA work in this repository is organized as a professional manual testing project.

## Featured Case Study

### Follow-Up Item Lifecycle

Scope:

**Create → Active State → Reminder → Reschedule → Mark Paid → Complete (Done/Lost) → Archive → Reopen**

[Open the case study](case-studies/follow-up-lifecycle/README.md)

## Completed QA Flow

- Planning — complete
- Requirements review — complete
- Test analysis — complete
- Test design — complete
- Zephyr test implementation — complete
- Manual execution — complete
- Defect reporting — complete
- Retest — complete
- Targeted regression — complete
- Test completion reporting — complete

## Execution Highlights

- **20** manual test cases executed in the initial system test cycle
- **17 Pass / 2 Fail / 1 Blocked** in the initial cycle
- **1 High-priority product defect** reported from the failed reopen/reminder path
- Defect fixed and verified through a dedicated **2-case retest cycle**
- **6-case targeted regression** completed with all tests passing
- Portable Jira and Zephyr execution exports included in the repository
- One residual environment risk documented for automatic scheduled reminder dispatch

## Working Model

- **GitHub** — planning, analysis, test-design, portable Jira/Zephyr evidence exports, execution-summary and completion artifacts
- **Zephyr Advanced** — manual test cases, test cycles and execution statuses
- **Jira** — defect tracking and related QA work
- **Postman / browser tools** — supporting manual API and runtime verification

No automated test implementation is included in this portfolio case study.

## Evidence Policy

Designed tests are kept separate from execution results.

Pass / Fail / Blocked statuses are reported only from completed manual executions. Requirements/documentation findings are kept separate from confirmed product defects, and unresolved risks remain visible in the completion report.
