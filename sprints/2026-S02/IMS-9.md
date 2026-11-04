---
id: IMS-9
title: "Implement atomic Stock Adjustment microflow with reason codes"
status: todo
type: story
epic: EPIC-02
sprint: 2026-S02
points: 5
priority: 1
labels: [stock, microflows]
blocked_by: [IMS-8]
order: 90
created: 2026-11-04
updated: 2026-11-04
---

## Context

Allow warehouse personnel to adjust inventory when physical items are damaged, expired, stolen, or found, recording financial reasons.

## Acceptance criteria

- [ ] Enumeration `AdjustmentReason` (Damaged, Expired, TheftOrLoss, FoundStock, InitialCountCorrection)
- [ ] Microflow `ACT_PerformStockAdjustment` creating `InventoryTransaction` and modifying `StockLevel` in one transaction
- [ ] Mandatory approval workflow if adjustment financial value exceeds threshold (e.g. $500)
- [ ] UI modal capturing reason, quantity delta, notes, and photo attachment if damaged
