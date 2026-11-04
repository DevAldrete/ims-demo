---
id: IMS-2
title: "Implement multi-tier Warehouse, Zone, and Bin location hierarchy"
status: todo
type: story
epic: EPIC-01
sprint: 2026-S01
points: 5
priority: 1
labels: [domain-model, warehouse]
order: 20
created: 2026-11-04
updated: 2026-11-04
---

## Context

Model physical storage locations in Mendix to support multi-site inventory tracking down to aisle, shelf, and bin levels.

## Acceptance criteria

- [ ] Entity `Warehouse` with Code, Name, Address, IsActive
- [ ] Entity `Zone` associated with Warehouse (e.g. Cold Storage, Bulk, Picking)
- [ ] Entity `LocationBin` associated with Zone with Aisle, Rack, Shelf, BinCode (unique per Warehouse)
- [ ] Default staging and receiving bins designated per warehouse
- [ ] Overview and detail editing pages with hierarchical tree or cascading dropdowns
