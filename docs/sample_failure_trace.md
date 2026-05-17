# Sample Failure Trace

## Context

Synthetic transaction QA trace for a Mexico QA Analyst conversation. Values are invented for demo purposes. For the six-dimension evaluation, release recommendation, and regression-test design built on this trace, see [`qa_matrix.md`](qa_matrix.md).

## Trace

```text
transaction_id: TXN-2026-0412-8841
merchant_region: MX
currency: MXN
authorized_amount: 1250.00
captured_amount: 1250.00

09:14:02 authorized
09:14:03 risk_check_passed
09:14:04 capture_requested
09:14:35 gateway_timeout
09:14:36 capture_failed
09:14:36 retry_pending
09:14:37 notification_sent
```

## Failure

`retry_pending` was created without an assigned owner or retry deadline.

## Expected Behavior

When capture fails after authorization, the retry record should include:

- owning system or queue
- retry deadline
- failure reason
- customer/merchant communication status
- reconciliation marker

## QA Classification

| Field | Value |
| --- | --- |
| Failure class | Workflow handoff defect |
| Severity | High |
| Reproducibility | Reproducible with gateway timeout fixture |
| Blast radius | Transactions that fail after authorization but before capture completion |
| Release recommendation | Fix before release or add operational compensating control |

## Evidence Note

The money fields remain consistent in this sample, so the clearest defect is traceability and ownership. That keeps the sample honest: it demonstrates QA discipline without overclaiming payments-system expertise. The matching regression-test design in [`qa_matrix.md`](qa_matrix.md) reflects this — the gate has to be at the workflow handoff, not at the money balance.
