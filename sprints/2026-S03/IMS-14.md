---
id: IMS-14
title: "Model Sales Order fulfillment and stock reservation engine"
status: todo
type: story
epic: EPIC-04
sprint: 2026-S03
points: 5
priority: 1
labels: [fulfillment, orders]
blocked_by: [IMS-8]
order: 140
created: 2026-11-04
updated: 2026-11-04
---

## Context

Prevent overselling by immediately reserving available stock when sales orders are accepted, releasing reservations on order cancellation.

## Acceptance criteria

- [ ] Entity `SalesOrder` and `SalesOrderLine` with Status (Created, Reserved, Picking, Packed, Shipped, Cancelled)
- [ ] Sub-microflow `SUB_ReserveStockForOrder` checking AvailableQty >= OrderedQty and incrementing QuantityReserved
- [ ] Rollback / release microflow clearing reserved stock if customer cancels or payment fails
- [ ] Allocation rule: FIFO (earliest order reserves first)
