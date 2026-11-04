---
id: IMS-7
title: "Establish automated unit testing suite for master data validations"
status: todo
type: chore
epic: EPIC-01
sprint: 2026-S01
points: 2
priority: 2
labels: [testing, quality]
blocked_by: [IMS-1]
order: 70
created: 2026-11-04
updated: 2026-11-04
---

## Context

Set up Mendix Unit Testing module to test master data entity constraints, price validations, and SKU deduplication microflows automatically.

## Acceptance criteria

- [ ] UnitTesting module imported and configured in Studio Pro
- [ ] Unit test microflow verifying SKU uniqueness validation failure
- [ ] Unit test microflow verifying negative price prevention
- [ ] Unit test microflow verifying UoM conversion calculations
- [ ] 100% passing test assertions
