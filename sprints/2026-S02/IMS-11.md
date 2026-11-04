---
id: IMS-11
title: "Create Purchase Order lifecycle entities and status transitions"
status: todo
type: story
epic: EPIC-03
sprint: 2026-S02
points: 5
priority: 1
labels: [procurement, po]
blocked_by: [IMS-1, IMS-3]
order: 110
created: 2026-11-04
updated: 2026-11-04
---

## Context

Track procurement lifecycle from draft requisition, approval, vendor issuance, through fulfillment and billing closure.

## Acceptance criteria

- [ ] Entity `PurchaseOrder` with PONumber, OrderDate, ExpectedDeliveryDate, Status (Draft, PendingApproval, Approved, Issued, PartiallyReceived, FullyReceived, Closed, Cancelled), TotalAmount
- [ ] Entity `POLineItem` with Product, OrderedQty, ReceivedQty, InvoicedQty, UnitPrice, Subtotal
- [ ] Approval microflow verifying budget limits based on user role
- [ ] State machine microflows preventing invalid status progressions
