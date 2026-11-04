---
id: IMS-5
title: "Configure Mendix User Roles, Module Roles, and security access matrix"
status: todo
type: story
epic: EPIC-06
sprint: 2026-S01
points: 5
priority: 1
labels: [security, rbac]
order: 50
created: 2026-11-04
updated: 2026-11-04
---

## Context

Enforce enterprise security compliance through Mendix Project Security. Define module roles and assign them to business user roles.

## Acceptance criteria

- [ ] Defined User Roles: `Administrator`, `InventoryManager`, `WarehouseClerk`, `PurchasingAgent`, `Auditor`
- [ ] Module Roles mapped with granular Read/Write access on entities and microflows
- [ ] XPath entity access constraints restricting warehouse clerks to their assigned warehouse locations
- [ ] Navigational profiles tailored per user role
- [ ] Project Security level set to `Production`
