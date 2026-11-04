---
id: IMS-21
title: "Implement Serial Number tracking for serialized electronics and equipment"
status: backlog
type: story
epic: EPIC-02
points: 8
priority: 3
labels: [serialized-stock]
order: 20
created: 2026-11-04
updated: 2026-11-04
---

## Context

Maintain discrete tracking for high-value assets requiring individual serial number records from receipt to customer warranty.

## Acceptance criteria

- [ ] Entity `SerializedItem` with SerialNumber (unique), WarrantyStartDate, WarrantyEndDate, CurrentLocation
- [ ] Serial number capture during PO goods receipt and outbound packing validation
- [ ] Searchable serial number history timeline showing every movement and order
