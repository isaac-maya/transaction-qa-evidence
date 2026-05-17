# 🧾 Transaction QA in Six Dimensions

> For the failures that don't look like failures.

**Built for:** QA Lead · QA Manager · Fintech Quality

---

## Why this exists

Dramatic payment bugs get caught. The dangerous ones are operational — a successful capture with a missing ownership handoff after retry, an authorized amount that matches the captured amount but lands in a `retry_pending` state with no owner. Functional tests pass. Reconciliation reports stay green. The defect ships.

This evidence pack frames a single synthetic transaction across **six QA dimensions** so the operational gap is impossible to miss. Money fields stayed consistent. Workflow ownership did not. That's the call a QA team should make — and the one most release reviews don't.

---

## What's inside

- ✅ **[QA Matrix](qa_matrix.md)** — six dimensions evaluated against one transaction with risk + result per dimension and an explicit release recommendation.
- ✅ **[Failure Trace](sample_failure_trace.md)** — the raw trace, expected-behavior contract, and QA classification table for the same transaction.

The same transaction ID (`TXN-2026-0412-8841`) threads through both pages so the artifact reads as one piece of evidence rather than three loose documents.

---

## How to read this

**If you have 2 minutes:** [QA Matrix](qa_matrix.md) — the matrix is the deliverable. The "State transition" and "Reconciliation" rows are the interesting QA calls.

**If you want to see the QA reasoning:** [Failure Trace](sample_failure_trace.md) — the raw trace + classification table show how the six dimensions are derived, not just declared.

---

## Honest scope

The numbers and trace are synthetic. This is not a payments-domain claim; it's a QA discipline claim made on a synthetic but realistic payments context. The strongest evidence in the artifact is the operational-handoff insight — a QA analyst noticing that money consistency does not imply workflow consistency.

---

## What this proves

**For QA Lead roles:** I see operational gaps that pass functional tests. Severity is a decision, not a default.

**For QA Manager roles:** I write evidence packs a non-engineer can review. The matrix is the deliverable, not an appendix.

**For Fintech Quality roles:** I respect that "fail" and "warning" are different release decisions. The release recommendation is derived from the matrix, not from severity vibes.

---

**Isaac Maya** — QA · Agentic AI · Data Quality
📧 theisaacmaya@icloud.com · 💼 [LinkedIn](https://linkedin.com/in/isaac-maya) · 🔗 [Source](https://github.com/isaac-maya/transaction-qa-evidence) · 📝 [Essays](https://isaac-maya.github.io/essays/)
