---
id: IMS-6
title: "Implement Excel bulk import and export microflows for Product catalog"
status: todo
type: story
epic: EPIC-01
sprint: 2026-S01
points: 5
priority: 2
labels: [data-migration, excel]
blocked_by: [IMS-1, IMS-4]
order: 60
created: 2026-11-04
updated: 2026-11-04
---

## Context

Allow inventory managers to bulk import products from spreadsheets and export catalog reports using Mendix Excel Importer and Export modules.

## Acceptance criteria

- [ ] Excel Importer template configured for Product, Category, and UoM mapping
- [ ] Microflow with batch transaction commit (every 250 objects) to avoid memory spikes
- [ ] Error logging table collecting rows with validation errors (e.g. duplicate SKU or invalid price)
- [ ] Single-click export microflow producing formatted Excel workbook of active catalog
