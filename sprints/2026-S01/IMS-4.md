---
id: IMS-4
title: "Implementar microflow de validación de Producto (precio y SKU)"
status: todo
type: story
epic: EPIC-01
sprint: 2026-S01
points: 2
priority: 2
labels: [microflows, validation]
blocked_by:
  - IMS-1
order: 40
created: 2026-11-04
updated: 2026-11-05
---

## Context

Asegurar que los datos ingresados cumplan con las reglas de negocio antes de guardar un producto en la base de datos.

## Acceptance criteria

- [ ] Microflow `ACT_Product_Save` creado y asignado al botón 'Guardar' del formulario
- [ ] Validación que el `SKU` y el `Name` no estén vacíos (mostrar mensaje de validación)
- [ ] Validación de que `Price` sea mayor a 0 y `MinimumStock` sea mayor o igual a 0
- [ ] Acción de commit y cierre de página si las validaciones son exitosas
