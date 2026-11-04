---
id: IMS-17
title: "Develop automated Reorder Point (ROP) alerts and daily notification job"
status: todo
type: story
epic: EPIC-05
sprint: 2026-S03
points: 5
priority: 1
labels: [alerts, scheduled-events]
blocked_by: [IMS-8]
order: 170
created: 2026-11-04
updated: 2026-11-04
---

## Context

Monitor stock levels continuously. When AvailableQty falls below ReorderPoint, generate alerts and draft procurement requisitions.

## Acceptance criteria

- [ ] Scheduled Event running daily at 06:00 executing `SE_EvaluateReorderThresholds`
- [ ] Formula: Reorder Needed if (QuantityOnHand + QuantityOnOrder - QuantityReserved) <= ReorderPoint
- [ ] In-app notification center entity showing low-stock alerts with one-click 'Create PO Requisition'
- [ ] Email notification dispatched to Purchasing Managers for critical low stock
