---
id: IMS-8
title: "Crear Dashboard principal con KPIs y alertas de stock bajo"
status: todo
type: story
epic: EPIC-03
sprint: 2026-S02
points: 3
priority: 1
labels: [dashboard, ui, kpis]
blocked_by:
  - IMS-2
  - IMS-6
order: 40
created: 2026-11-04
updated: 2026-11-05
---

## Context

Diseñar la página de inicio que resume el estado del inventario para supervisores y operadores.

## Acceptance criteria

- [ ] Tarjeta KPI 1: Total de productos registrados en catálogo
- [ ] Tarjeta KPI 2: Cantidad de productos con stock bajo (`CurrentStock <= MinimumStock`)
- [ ] Data Grid en Dashboard mostrando productos críticos que requieren reorden urgente (resaltados con badge rojo)
- [ ] Lista de los últimos 5 movimientos registrados recientemente
