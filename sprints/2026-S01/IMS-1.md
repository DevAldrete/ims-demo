---
id: IMS-1
title: "Design core Mendix domain model for Product, Category, and UoM"
status: todo
type: story
epic: EPIC-01
sprint: 2026-S01
points: 5
priority: 1
labels: [domain-model, catalog]
order: 10
created: 2026-11-04
updated: 2026-11-04
---

## Context

Establish the foundational entities for the Inventory Management System within Mendix Studio Pro. Products need unique SKUs, barcodes, unit of measure relationships, categorization hierarchies, and pricing attributes.

## Acceptance criteria

- [ ] Entity `Product` created with attributes: SKU (unique), Name, Description, Barcode, CostPrice, SellingPrice, IsActive
- [ ] Entity `Category` created with self-referencing association for subcategories
- [ ] Entity `UnitOfMeasure` created with code (e.g., PCS, BOX, KG) and conversion factors
- [ ] Database unique constraint applied on SKU and Barcode
- [ ] Mendix validation rules configured for non-negative prices and mandatory fields
