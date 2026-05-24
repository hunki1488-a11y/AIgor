# Sprint Preview: augchatd#5 Spec Reconciliation

Lead: https://github.com/augchatd/augchatd/issues/5

## Proposed Paid Sprint

Price: `250 USD` for focused spec reconciliation, or `500 USD` for broader contract coverage.

Turnaround: `48h` after scope confirmation.

Deliverable: documentation PR that reconciles the visible drift without changing implementation.

## Current Finding

The issue is a good paid sprint candidate because the work is specific, bounded, and testable as documentation/spec quality:

- `DEMO_THEME` is described in the issue, but `spec/src/behavior/contracts/demo-mode.md` lists `DEMO_MODEL_*`, `DEMO_CONNECTORS`, and `DEMO_S3_URI` only.
- `GET /demo/jwt` is still documented as returning only `jwt`, while the issue says the response now carries theme state too.
- `AUGCHATD_DATA_DIR` is described in the issue but is absent from the demo-mode environment-variable list.
- `AUGCHATD_TRACE_DIR` needs a precise observability reconciliation: `spec/src/constraints/observability.md` says no built-in audit log or distributed tracing, while the issue describes a local JSONL debug trace.

## Patch Shape

The focused sprint should:

- Reconcile demo-mode environment variables.
- Update the `GET /demo/jwt` response contract.
- Clarify that JSONL debug trace output is operator-local debug output, not distributed tracing, metrics, or an audit log.
- Add pending reconciliation markers only where maintainer direction is still required.

The broader sprint should also cover model picker, conversation list, and abort/cancellation technical contracts.

## Acceptance Criteria

- The spec no longer contradicts the issue text for demo mode, JWT response shape, data directory, or trace directory.
- Observability constraints remain intact.
- Any unresolved product choices are marked as explicit pending reconciliation points instead of hidden drift.
