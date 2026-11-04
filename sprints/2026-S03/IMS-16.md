---
id: IMS-16
title: "Implement Cycle Count planning, blind count entry, and variance approval"
status: todo
type: story
epic: EPIC-05
sprint: 2026-S03
points: 8
priority: 1
labels: [audit, cycle-count]
blocked_by: [IMS-8, IMS-9]
order: 160
created: 2026-11-04
updated: 2026-11-04
---

## Context

Conduct periodic inventory audits without shutting down operations. Clerks perform blind physical counts; system computes variances for manager approval.

## Acceptance criteria

- [ ] Entity `CycleCount` and `CycleCountLine` with SystemSnapshotQty, CountedQty, VarianceQty, VarianceValue
- [ ] Blind entry UI hiding SystemSnapshotQty from counting clerks to prevent bias
- [ ] Variance analysis view highlighting count discrepancies above threshold ($100 or 5%)
- [ ] Manager approval microflow triggering automatic stock reconciliation adjustments and audit log entry
