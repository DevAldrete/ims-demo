# Diseño de Interfaz y Páginas en Mendix

Esta guía describe cómo maquetar la interfaz de usuario en Mendix Studio Pro utilizando **Atlas UI 3.0** y el widget **Data Grid 2**.

---

## 1. Estructura de Navegación

En el menú principal de la aplicación (`Navigation`), define 3 accesos rápidos:

1. **Dashboard** (`Home_Dashboard.page.xml`): Vista resumen con KPIs y alertas de existencias.
2. **Productos** (`Product_Overview.page.xml`): Catálogo completo con buscador y filtros.
3. **Movimientos** (`StockMovement_Overview.page.xml`): Historial cronológico de entradas y salidas.

---

## 2. Pantalla 1: Dashboard de Inicio (`Home_Dashboard`)

Diseñado con layout de tarjetas KPI y paneles de información:

### A. Fila Superior: Tarjetas de Indicadores (KPI Cards)
Usa bloques `Container` con la clase de Atlas UI `card card-body shadow-sm`:

| Tarjeta KPI | Fuente de Datos / Microflow | Indicador | Estilo Atlas UI |
| :--- | :--- | :--- | :--- |
| **Total de Productos** | Cuenta de todos los objetos `Product` | Número total de SKUs | Badge Azul (`badge-info`) |
| **Productos en Alerta** | Cuenta con XPath `[CurrentStock <= MinimumStock]` | Artículos por reponer | Badge Rojo (`badge-danger`) |
| **Movimientos del Día** | Cuenta de `StockMovement` con fecha de hoy | Entradas/Salidas registradas | Badge Verde (`badge-success`) |

### B. Fila Inferior: Lista de Productos en Stock Crítico
- Un widget **Data Grid 2** con fuente de datos por XPath:
  ```xpath
  [CurrentStock <= MinimumStock]
  ```
- Columnas: SKU, Nombre, Stock Actual, Stock Mínimo.
- Botón de acción por fila: **"+ Registrar Entrada"** (abre el modal de movimiento precargando el producto seleccionado).

---

## 3. Pantalla 2: Catálogo de Productos (`Product_Overview`)

La pantalla principal para consultar y administrar el catálogo:

### Configuración de Data Grid 2:
- **Barra Superior de Herramientas**:
  - Botón principal: `+ Nuevo Producto` (abre `Product_NewEdit.page.xml`).
  - Campo de búsqueda por texto: busca en `Name` y `SKU`.
  - Filtro desplegable: filtra por `Category`.
- **Columnas**:
  1. **SKU**: Texto en negrita.
  2. **Nombre**: Nombre del producto.
  3. **Categoría**: Recuperado por la asociación `Product_Category/Name`.
  4. **Precio**: Formateado en moneda local (ej: `$1,250.00`).
  5. **Stock Actual**: Con badge de color dinámico:
     - Verde (`badge-success`) si `CurrentStock > MinimumStock`.
     - Rojo (`badge-danger`) si `CurrentStock <= MinimumStock`.
  6. **Stock Mínimo**: Límite de reposición.
  7. **Acciones**: Botón de editar (icono lápiz) y botón "+ Movimiento".

---

## 4. Pantalla 3: Modal de Registro de Movimiento (`StockMovement_New`)

Ventana emergente tipo modal (`Pop-up layout`):

- **Tipo de Movimiento**: Selector desplegable de la enumeración `MovementType` (`Entrada`, `Salida`, `Ajuste`).
- **Producto**: Selector de referencia desplegable (`Product_Category`).
- **Ubicación / Almacén**: Selector desplegable (`StockMovement_Location`).
- **Cantidad**: Input numérico entero (`Quantity`).
- **Notas**: Campo de texto para indicar motivo o número de comprobante.
- **Botón Guardar**: Llama al microflow `ACT_StockMovement_Process`.
- **Botón Cancelar**: Cierra el pop-up sin persistir cambios.
