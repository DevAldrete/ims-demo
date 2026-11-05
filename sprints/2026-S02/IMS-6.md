---
id: IMS-6
title: "Implementar microflow para registrar movimiento y actualizar stock"
status: todo
type: story
epic: EPIC-02
sprint: 2026-S02
points: 3
priority: 1
labels: [microflows, logic]
blocked_by:
  - IMS-5
order: 20
created: 2026-11-04
updated: 2026-11-05
---

## Context

Desarrollar la lógica principal que recalcula el stock del producto al registrar una entrada o salida.

## Acceptance criteria

- [ ] Microflow `ACT_StockMovement_Process` ejecutado al guardar el movimiento
- [ ] Si `MovementType == In`: suma `Quantity` a `Product.CurrentStock`
- [ ] Si `MovementType == Out`: verifica que haya suficiente stock (`CurrentStock >= Quantity`), resta la cantidad o muestra mensaje de error
- [ ] Si `MovementType == Adjustment`: asigna el nuevo valor directo a `Product.CurrentStock`
- [ ] Commit atómico de `StockMovement` y `Product`
