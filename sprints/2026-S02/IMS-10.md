---
id: IMS-10
title: "Build Inter-Warehouse and Bin-to-Bin stock transfer wizard"
status: todo
type: story
epic: EPIC-02
sprint: 2026-S02
points: 5
priority: 1
labels: [stock-transfer, ui]
blocked_by: [IMS-8]
order: 100
created: 2026-11-04
updated: 2026-11-04
---

## Context

Support both internal bin-to-bin movements (instant) and multi-day inter-warehouse transfers (requires dispatch and receipt confirmation).

## Acceptance criteria

- [ ] Entity `StockTransfer` with Status (Draft, Requested, Dispatched, Received, Cancelled)
- [ ] Entity `StockTransferLine` with Product, SourceBin, DestinationBin, RequestedQty, ShippedQty, ReceivedQty
- [ ] Microflow validating available quantity before dispatch and moving stock to In-Transit
- [ ] Receipt microflow acknowledging transfer arrival and committing stock to destination bin
- [ ] Multi-step wizard page guiding clerk through transfer creation, pick list, and dispatch
