# QA Case Study — Follow-Up Item Lifecycle

## Purpose

This case study demonstrates how I approach a product as a Junior QA Engineer: reading the specification, finding requirement gaps, identifying product risks, designing tests, separating evidence from assumptions, and planning execution.

The system under test is [Follow-Up Radar](https://github.com/jadewanson/Follow-Up-Radar), a training product built as a narrow follow-up tracker for freelancers and micro-studios. Its documented core loop is:

**Capture → Schedule → Remind → Act → Close**

This case study focuses on the most central domain feature:

**Follow-up item lifecycle — Create → Active State → Reminder → Reschedule → Mark Paid → Complete (Done/Lost) → Archive → Reopen**

## Test basis

The analysis uses the current product documentation and implementation snapshot, not invented product behavior:

- `docs/01_product_constitution.md`
- `docs/03_scope_v1.md`
- `docs/04_user_flows_and_activation.md`
- `docs/05_domain_model.md`
- `docs/06_state_machine.md`
- `docs/08_api_contracts.md`
- `docs/11_testing_strategy_and_quality_gates.md`
- `backend/app/api/routes_items.py`
- `backend/app/schemas/followup_item.py`
- `backend/app/services/item_service.py`
- existing backend tests for registry, lifecycle and outcome reasons

## Why this feature

The lifecycle crosses several QA surfaces at once:

- requirements and state-machine analysis;
- positive, negative and boundary testing;
- API contracts;
- authorization / ownership;
- persistent state;
- reminder side effects;
- event history;
- UI state;
- regression risk.

That makes it a better employer-facing sample than an isolated form validation exercise.

## Artifacts

- [Requirements review](requirements-review.md)
- [Test approach](test-approach.md)
- [Manual/API checklist](checklist.md)
- [Detailed test cases](test-cases.md)
- [Static review findings](static-review-findings.md)
- [Current test summary](test-summary.md)

## Current evidence status

### Completed

- product documentation review;
- contract/state-machine review;
- requirements contradiction and ambiguity analysis;
- risk analysis;
- test design;
- static implementation cross-check.

### Not claimed as completed yet

- browser execution against a running build;
- Postman collection execution;
- direct PostgreSQL validation;
- DevTools evidence;
- runtime bug reproduction.

Those are intentionally not marked as passed until they are actually executed.

## Core rules extracted from the product

- lifecycle states: `open`, `due_today`, `overdue`, `done`, `lost`, `archived`;
- payment state is independent: `unpaid`, `paid`, `not_applicable`;
- `paid` does **not** close the case;
- active status is projected from `due_at` against current UTC date;
- only active cases can be rescheduled;
- completion requires `done` or `lost` plus a non-empty reason;
- completion clears `next_action` and cancels pending reminders;
- only `done` and `lost` can be reopened;
- reopening re-arms reminder coverage;
- archiving cancels pending reminders;
- all item access is scoped to the authenticated owner.

## QA principle used throughout

A green test proves only that no failure was observed in that executed scenario. It does not prove that the feature is defect-free.

The same evidence rule is used in the artifacts below: static findings, planned tests and executed evidence are kept separate.
