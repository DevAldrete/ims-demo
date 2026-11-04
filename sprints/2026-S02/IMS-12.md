---
id: IMS-12
title: "Implement Goods Receiving and Putaway microflow with variance logging"
status: todo
type: story
epic: EPIC-03
sprint: 2026-S02
points: 8
priority: 1
labels: [receiving, warehouse]
blocked_by: [IMS-8, IMS-11]
order: 120
created: 2026-11-04
updated: 2026-11-04
---

## Context

Process physical shipments arriving at warehouse loading docks against active Purchase Orders, supporting partial deliveries and damaged goods quarantine.

## Acceptance criteria

- [ ] Entity `GoodsReceipt` and `GoodsReceiptLine` recording ReceivedQty, AcceptedQty, RejectedQty, and QuarantineReason
- [ ] Microflow creating `InventoryTransaction` of type PurchaseReceipt and updating `StockLevel`
- [ ] Automated calculation of PO status transition: `PartiallyReceived` or `FullyReceived`
- [ ] Putaway suggestion microflow directing clerk to preferred bin based on product category
