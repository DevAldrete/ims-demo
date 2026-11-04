---
id: IMS-20
title: "Implement Lot and Batch tracking with expiration date enforcement"
status: backlog
type: story
epic: EPIC-02
points: 8
priority: 2
labels: [batch-tracking, compliance]
order: 10
created: 2026-11-04
updated: 2026-11-04
---

## Context

Regulate perishable goods, pharmaceuticals, or regulated batches by enforcing Lot/Batch numbers, manufacturing dates, and FEFO picking.

## Acceptance criteria

- [ ] Entity `LotBatch` associated with Product and StockLevel
- [ ] Attributes: BatchNumber, ManufactureDate, ExpirationDate, CertificateOfAnalysisUrl
- [ ] First-Expired, First-Out (FEFO) picking rule allocation microflow
- [ ] Quarantine alert on lots within 30 days of expiration
