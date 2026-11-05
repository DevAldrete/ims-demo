# Guía Práctica Paso a Paso: Construyendo el IMS en Mendix

Esta guía es un tutorial práctico estructurado para que cualquier estudiante o desarrollador pueda construir la aplicación de Gestión de Inventarios en **Mendix Studio Pro** de principio a fin.

---

## Paso 1: Crear el Proyecto en Mendix Studio Pro

1. Abre **Mendix Studio Pro** (versión 10.x recomendada).
2. Haz clic en **Create New App** -> Selecciona la plantilla base **Blank App**.
3. Nombra tu proyecto: `InventoryDemo` y haz clic en **Create App**.

---

## Paso 2: Diseñar el Modelo de Dominio (Domain Model)

1. En el panel de exploración izquierdo, abre **App** -> **MyFirstModule** (puedes renombrarlo a `InventoryManagement`).
2. Abre el archivo **Domain Model**.
3. **Crear Enumeración `MovementType`**:
   - Clic derecho en el módulo -> **Add Other** -> **Enumeration**.
   - Nombre: `MovementType`.
   - Agrega 3 valores:
     - `In` (Caption: `Entrada`)
     - `Out` (Caption: `Salida`)
     - `Adjustment` (Caption: `Ajuste`)
4. **Crear las 4 Entidades**:
   - Arrastra una entidad y llámala `Product`:
     - `SKU` (String, 30)
     - `Name` (String, 100)
     - `Description` (String, unlimited)
     - `Price` (Decimal, 10, 2)
     - `CurrentStock` (Integer, Default: 0)
     - `MinimumStock` (Integer, Default: 5)
   - Arrastra la entidad `Category`:
     - `Name` (String, 50)
     - `Description` (String, 200)
   - Arrastra la entidad `Location`:
     - `Code` (String, 20)
     - `Name` (String, 100)
   - Arrastra la entidad `StockMovement`:
     - `MovementType` (Enumeration: `MovementType`)
     - `Quantity` (Integer)
     - `MovementDate` (DateTime, Default: `CurrentDateTime`)
     - `Notes` (String, 200)
5. **Crear las Asociaciones (Líneas de Relación)**:
   - Haz clic en `Product` y arrastra la flecha hacia `Category` (relación `* -> 1`).
   - Haz clic en `StockMovement` y arrastra hacia `Product` (relación `* -> 1`).
   - Haz clic en `StockMovement` y arrastra hacia `Location` (relación `* -> 1`).

---

## Paso 3: Crear los Microflows de Lógica

### Microflow A: `ACT_Product_Save` (Validación de Producto)
1. Clic derecho en el módulo -> **Add Microflow** -> `ACT_Product_Save`.
2. Añade un parámetro de tipo `Product`.
3. Inserta una decisión (**Exclusive Split**):
   - Expresión: `$Product/SKU != empty and $Product/Price > 0`
   - Si es Falso: Agrega una actividad **Validation Feedback** indicando que el SKU y el precio son inválidos.
   - Si es Verdadero: Agrega una actividad **Commit Object** (`Product`) y luego **Close Page**.

### Microflow B: `ACT_StockMovement_Process` (Actualización de Stock)
1. Crea el microflow `ACT_StockMovement_Process` con parámetro de entrada `StockMovement`.
2. Agrega una actividad **Retrieve**:
   - By Association -> Trae el objeto `Product` asociado al `StockMovement`.
3. Inserta una decisión (**Exclusive Split**) para evaluar el tipo de movimiento:
   - Expresión: `$StockMovement/MovementType`
4. **Rama `In` (Entrada)**:
   - Agrega **Change Object** (`Product`):
     - Atributo `CurrentStock`: `$Product/CurrentStock + $StockMovement/Quantity`
5. **Rama `Out` (Salida)**:
   - Inserta una decisión: `$Product/CurrentStock >= $StockMovement/Quantity`
   - Si es falso: **Show Message** (Error: "Stock insuficiente").
   - Si es verdadero: **Change Object** (`Product`):
     - Atributo `CurrentStock`: `$Product/CurrentStock - $StockMovement/Quantity`
6. **Rama `Adjustment` (Ajuste)**:
   - **Change Object** (`Product`):
     - Atributo `CurrentStock`: `$StockMovement/Quantity`
7. Al final, conecta las ramas a un **Commit Object** que guarde tanto `Product` como `StockMovement`, y finaliza con **Close Page**.

---

## Paso 4: Diseñar las Páginas de Usuario

1. **Catálogo de Productos (`Product_Overview`)**:
   - Crea una página en blanco con Layout Atlas Responsive.
   - Inserta el widget **Data Grid 2** conectado a la entidad `Product`.
   - Agrega columnas para: SKU, Nombre, Categoría, Precio y Stock Actual.
   - En la columna Stock Actual, añade una condición de estilo: si `CurrentStock <= MinimumStock`, resáltalo con color rojo.
   - En la barra superior, añade un botón que abra la página de creación de producto.
2. **Registro de Movimiento (`StockMovement_New`)**:
   - Crea una página con Layout **PopupLayout**.
   - Coloca un contenedor de formulario conectado a `StockMovement`.
   - Incluye selectores para: Tipo de Movimiento, Producto, Almacén, Cantidad y Notas.
   - Conecta el botón Guardar al microflow `ACT_StockMovement_Process`.
3. **Dashboard Principal (`Home_Dashboard`)**:
   - Crea 2 tarjetas KPI en la parte superior:
     - "Total Productos en Catálogo"
     - "Productos con Stock Crítico"
   - Debajo, coloca un Data Grid con los productos que cumplan `[CurrentStock <= MinimumStock]`.

---

## Paso 5: Configurar la Navegación y Probar la Aplicación

1. En el Project Explorer, abre **App** -> **Navigation**.
2. En el menú de escritorio, agrega:
   - **Dashboard** -> Abre `Home_Dashboard`.
   - **Catálogo de Productos** -> Abre `Product_Overview`.
3. Presiona el botón verde de **Run Locally** (o tecla `F5`).
4. Una vez que termine el despliegue local, haz clic en **View App** para abrir tu sistema en el navegador web.
5. ¡Listo! Registra categorías, crea tus primeros productos y prueba registrar entradas y salidas de inventario.
