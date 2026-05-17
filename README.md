# Mastercard QA Add-On

A compact transaction-QA evidence sample for the Mastercard Mexico QA Analyst conversation. The pack is small on purpose: validation rigor, traceability, and manager-readable defect framing — not domain claims.

## What It Demonstrates In 30 Seconds

- The most interesting QA call in this sample is not the gateway timeout itself; it is that the money fields remain consistent while the operational handoff fails. Catching that distinction is the QA discipline a payments team actually needs.
- The matrix evaluates six transaction-QA dimensions and produces an explicit go / no-go recommendation, not a green-or-red verdict.
- The failure trace, classification, and manager summary all reference the same transaction ID so the artifact reads as one piece of evidence rather than three loose documents.

## Pack Contents

- `qa_matrix.md`: six QA dimensions evaluated against synthetic transaction `TXN-2026-0412-8841`, with risk and result per dimension and a regression-test design section.
- `sample_failure_trace.md`: the raw trace, expected-behavior contract, and QA classification table for the same transaction.
- `outreach_note.md`: short sharing note for the Mastercard Mexico QA Analyst conversation.

## Why It Fits Mastercard Mexico QA Analyst

The role centers on validation rigor, transaction quality, defect reporting, and stakeholder-readable QA evidence. This pack hits each of those without claiming insider payments knowledge:

- Validation rigor: six dimensions evaluated individually rather than a single pass/fail line.
- Transaction quality: financial consistency separated from operational consistency — both checked, both reported.
- Defect reporting: classification table with severity, reproducibility, blast radius, and release recommendation.
- Stakeholder readability: the manager summary names the specific operational issue (missing owner on retry handoff) rather than burying it under a generic "transaction failed" label.

## Honest Scope

The numbers and trace are synthetic. The pack is not a payments-domain claim; it is a QA discipline claim made on a synthetic but realistic payments context. The strongest evidence in the artifact is the operational-handoff insight — a QA analyst noticing that money consistency does not mean workflow consistency.

## Outreach Hook

A compact transaction-QA evidence sample where the lead finding is operational, not financial: the money fields stayed consistent, but the failed capture entered `retry_pending` without an owner — that is the real defect a QA team should catch.
