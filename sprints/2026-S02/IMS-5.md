---
id: IMS-5
title: "Crear entidad StockMovement y relaciones con Producto y Ubicación"
status: todo
type: story
epic: EPIC-02
sprint: 2026-S02
points: 3
priority: 1
labels: [domain-model, movements]
blocked_by:
  - IMS-1
order: 10
created: 2026-11-04
updated: 2026-11-05
---

## Context

Modelar el registro de entradas y salidas de inventario para registrar cada cambio físico en el almacén.

## Acceptance criteria

- [ ] Enumeración `MovementType` con valores: `In` (Entrada), `Out` (Salida), `Adjustment` (Ajuste)
- [ ] Entidad `StockMovement` con atributos: `MovementType` (Enum), `Quantity` (Integer), `MovementDate` (DateTime), `Notes` (String)
- [ ] Asociación 1 a muchos entre `Product` y `StockMovement`
- [ ] Asociación 1 a muchos entre `Location` y `StockMovement`
