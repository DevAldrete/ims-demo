---
id: IMS-1
title: "Diseñar modelo de dominio para Producto, Categoría y Ubicación"
status: todo
type: story
epic: EPIC-01
sprint: 2026-S01
points: 3
priority: 1
labels: [domain-model, mendix]
order: 10
created: 2026-11-04
updated: 2026-11-05
---

## Context

Crear las entidades fundamentales en el Domain Model de Mendix Studio Pro para la práctica de inventario.

## Acceptance criteria

- [ ] Entidad `Product` con atributos: `SKU` (String), `Name` (String), `Description` (String), `Price` (Decimal), `CurrentStock` (Integer), `MinimumStock` (Integer) y asociación a `System.Image`
- [ ] Entidad `Category` con atributo: `Name` (String)
- [ ] Entidad `Location` con atributos: `Code` (String) y `Name` (String)
- [ ] Asociación 1 a muchos entre `Category` y `Product`
