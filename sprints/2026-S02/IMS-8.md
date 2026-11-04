---
id: IMS-8
title: "Design immutable Inventory Transaction Ledger and Stock Level entities"
status: todo
type: story
epic: EPIC-02
sprint: 2026-S02
points: 5
priority: 1
labels: [inventory, ledger]
blocked_by: [IMS-1, IMS-2]
order: 80
created: 2026-11-04
updated: 2026-11-04
---

## Context

Create the double-entry style stock ledger to track inventory changes. Stock levels are calculated projections, while InventoryTransactions are immutable audit records.

## Acceptance criteria

- [ ] Entity `StockLevel` with QuantityOnHand, QuantityReserved, QuantityAvailable (calculated), QuantityInTransit, ReorderPoint, SafetyStock
- [ ] Composite index on `StockLevel (Product_StockLevel, LocationBin_StockLevel)`
- [ ] Entity `InventoryTransaction` with TransactionType (PurchaseReceipt, SalesFulfillment, TransferIn, TransferOut, AdjustmentPlus, AdjustmentMinus, CycleCountVariance), Quantity, UnitCost, Timestamp, ReferenceNumber
- [ ] Sub-microflow `SUB_UpdateStockLevel` ensuring atomic transaction writes with database locking
