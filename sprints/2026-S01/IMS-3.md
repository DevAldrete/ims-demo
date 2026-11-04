---
id: IMS-3
title: "Create Supplier master data entity and management pages"
status: todo
type: story
epic: EPIC-01
sprint: 2026-S01
points: 3
priority: 1
labels: [suppliers, ui]
order: 30
created: 2026-11-04
updated: 2026-11-04
---

## Context

Track vendors and suppliers who supply products, including payment terms, lead times, contact info, and supplier catalog pricing.

## Acceptance criteria

- [ ] Entity `Supplier` with Code, CompanyName, ContactEmail, Phone, PaymentTerms, LeadTimeDays
- [ ] Entity `SupplierProduct` association holding SupplierSKU, MinimumOrderQty, and ContractPrice
- [ ] Responsive Mendix list and detail view with supplier performance metrics placeholder
- [ ] Validation preventing duplicate Supplier Code
