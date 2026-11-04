---
id: IMS-15
title: "Build Pick, Pack, and Ship workflow with packing slip generation"
status: todo
type: story
epic: EPIC-04
sprint: 2026-S03
points: 8
priority: 1
labels: [fulfillment, dispatch]
blocked_by: [IMS-14]
order: 150
created: 2026-11-04
updated: 2026-11-04
---

## Context

Orchestrate the physical warehouse fulfillment path: generate pick paths, verify items at packing station, decrement stock, and dispatch.

## Acceptance criteria

- [ ] Optimized pick list sorting items by warehouse bin sequence (Aisle -> Shelf -> Bin) to minimize travel time
- [ ] Packing verification screen where clerk confirms SKU barcode scan before boxing
- [ ] Shipping dispatch microflow creating Outbound `InventoryTransaction`, decreasing QuantityOnHand and QuantityReserved
- [ ] PDF Document Template generating branded Packing Slip and Shipping Label
