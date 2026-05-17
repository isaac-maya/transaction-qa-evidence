# Mastercard Transaction QA Add-On

## QA Matrix

This is a compact transaction-quality evidence sample for synthetic transaction `TXN-2026-0412-8841`. The raw trace and QA classification table live in [`sample_failure_trace.md`](sample_failure_trace.md); this file evaluates the same transaction across six QA dimensions and recommends a release action. The pack is deliberately narrow and does not claim Mastercard domain expertise; it shows validation rigor, traceability, and manager-readable defect communication.

## What It Demonstrates In 30 Seconds

- The sample stays honest about scope while still showing disciplined QA thinking.
- The failure is operationally meaningful: not a dramatic payment bug, but a missing ownership handoff after failure.
- The output is easy to skim in a recruiter thread or attach as a small add-on to a broader QA artifact.

| QA dimension | Validation question | Sample evidence | Risk | Result |
| --- | --- | --- | --- | --- |
| Transaction identity | Is the transaction uniquely traceable across systems? | `transaction_id=TXN-2026-0412-8841` appears in gateway, ledger, and notification traces | Duplicate or missing reconciliation | Pass |
| Amount integrity | Does authorized amount match captured amount? | Authorization: `1250.00 MXN`; capture: `1250.00 MXN` | Customer or merchant balance mismatch | Pass |
| Currency handling | Are currency and minor units consistent? | `MXN`, exponent 2, no rounding drift | Settlement discrepancy | Pass |
| State transition | Is the state sequence valid? | `authorized -> capture_requested -> capture_failed -> retry_pending` | Incorrect completion state | Fail |
| Failure reporting | Is the failure reason actionable? | Gateway timeout is preserved, but user-facing note says generic processing error | Slow triage | Warning |
| Reconciliation | Can operations identify owner and next step? | Missing owner in retry queue item | Unowned recovery work | Fail |

## Manager Summary

The transaction is financially consistent, but the workflow should not be considered clean because a failed capture entered `retry_pending` without an owner. The key QA issue is not the original gateway timeout; it is the missing operational handoff after the timeout.

## Recommended Action

Block auto-close for this transaction state until the retry queue requires an owner, retry deadline, and failure reason. Add a regression check for failed capture handoff before release.

## Regression Test Design

The defect is operational, so the regression test must check operational state, not just financial state. A passing test for this specific failure shape would assert:

| Assertion | Expected | Why |
| --- | --- | --- |
| `retry_record.owner` | Non-empty, matches a known queue | An unowned retry is the actual defect surface; this is the test the original missing check should have been. |
| `retry_record.retry_deadline` | Set and in the future | Without a deadline the retry can stall silently. |
| `retry_record.failure_reason` | Mirrors gateway error, not generic | Generic failure text slows triage and breaks ownership routing. |
| `transaction.authorized_amount == transaction.captured_amount` | True only when state is `completed` | Money consistency must be tied to terminal state, not asserted globally. |
| `notification.user_facing_message` | Maps to a reviewed copy entry for `capture_failed` | Generic notifications hide the operational issue from support. |

The point is that a single transaction-level pass/fail gate would have missed this defect. The regression gate has to be at the workflow handoff, not at the money balance.
