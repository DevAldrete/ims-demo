---
id: IMS-18
title: "Integrate camera/barcode scanner widget for mobile receiving and picking"
status: todo
type: story
epic: EPIC-06
sprint: 2026-S03
points: 5
priority: 2
labels: [barcode, mobile]
blocked_by: [IMS-12, IMS-15]
order: 180
created: 2026-11-04
updated: 2026-11-04
---

## Context

Enable warehouse staff to use mobile devices or ruggedized barcode scanners (Zebra, Honeywell) to scan SKUs, Bins, and PO barcodes.

## Acceptance criteria

- [ ] Barcode Scanner widget integrated into Mendix Mobile/PWA responsive layout
- [ ] Nanoflow handling scanner callback: look up SKU or LocationBin immediately
- [ ] Audio/visual haptic feedback on successful scan vs invalid barcode error
- [ ] Support for Code 128, QR Code, and EAN-13 symbologies
