# Lógica de Negocio y Microflows

En Mendix, los **Microflows** son diagramas de flujo visuales que ejecutan la lógica del servidor. Para este demo solo requerimos **dos microflows esenciales**:

1. **`ACT_Product_Save`**: Validación previa antes de guardar un producto.
2. **`ACT_StockMovement_Process`**: Cálculo de existencias (+/-) al registrar una entrada o salida.

---

## 1. Microflow: `ACT_Product_Save`

Se ejecuta al hacer clic en el botón **Guardar** del formulario de producto.

```mermaid
flowchart TD
    START([Inicio: Parámetro Product]) --> CHECK_SKU{¿SKU o Nombre vacíos?}
    CHECK_SKU -- Sí --> MSG_ERR1[Mostrar mensaje de validación: 'El SKU y Nombre son obligatorios']
    MSG_ERR1 --> END_ERR([Fin])

    CHECK_SKU -- No --> CHECK_PRICE{¿Precio <= 0 o StockMínimo < 0?}
    CHECK_PRICE -- Sí --> MSG_ERR2[Mostrar mensaje de validación: 'Precio debe ser positivo']
    MSG_ERR2 --> END_ERR

    CHECK_PRICE -- No --> COMMIT[Commit Product en Base de Datos]
    COMMIT --> CLOSE_PAGE[Cerrar página modal]
    CLOSE_PAGE --> END_OK([Fin: Guardado exitoso])
```

### Pasos en Mendix Studio Pro:
1. Crear microflow `ACT_Product_Save` con parámetro de entrada `Product`.
2. Insertar una decisión (`Exclusive Split`):
   ```mendix
   $Product/SKU != empty and trim($Product/SKU) != '' and
   $Product/Name != empty and trim($Product/Name) != ''
   ```
3. Si es falso: Insertar actividad **Validation Feedback** en el miembro `SKU` o `Name`.
4. Insertar segunda decisión para verificar `Price > 0`.
5. Si pasa las validaciones: Actividad **Commit** (`Product`) y actividad **Close Page**.

---

## 2. Microflow: `ACT_StockMovement_Process`

Se ejecuta al confirmar el registro de un movimiento de inventario en la ventana emergente.

```mermaid
flowchart TD
    START([Inicio: Parámetro StockMovement]) --> RETRIEVE[Obtener Product asociado]
    RETRIEVE --> CHECK_QTY{¿Cantidad <= 0?}
    CHECK_QTY -- Sí --> ERR_QTY[Validation Feedback: 'La cantidad debe ser mayor a 0']
    ERR_QTY --> END_FAIL([Fin])

    CHECK_QTY -- No --> SWITCH_TYPE{Tipo de Movimiento}
    
    SWITCH_TYPE -- In (Entrada) --> ADD_STOCK[Sumar: CurrentStock = CurrentStock + Quantity]
    
    SWITCH_TYPE -- Out (Salida) --> CHECK_ENOUGH{¿CurrentStock >= Quantity?}
    CHECK_ENOUGH -- No --> ERR_STOCK[Mensaje de Error: 'Stock insuficiente para despachar']
    ERR_STOCK --> END_FAIL
    CHECK_ENOUGH -- Yes --> SUB_STOCK[Restar: CurrentStock = CurrentStock - Quantity]
    
    SWITCH_TYPE -- Adjustment (Ajuste) --> SET_STOCK[Asignar: CurrentStock = Quantity]

    ADD_STOCK --> COMMIT_ALL[Commit StockMovement y Product]
    SUB_STOCK --> COMMIT_ALL
    SET_STOCK --> COMMIT_ALL

    COMMIT_ALL --> SHOW_MSG[Mensaje informativo: 'Inventario actualizado']
    SHOW_MSG --> CLOSE_PAGE[Cerrar página modal]
    CLOSE_PAGE --> END_SUCCESS([Fin])
```

### Pasos en Mendix Studio Pro:
1. Crear microflow `ACT_StockMovement_Process` con parámetro de entrada `StockMovement`.
2. Actividad **Retrieve** por asociación para traer el objeto `Product`:
   ```mendix
   $StockMovement/InventoryManagement.StockMovement_Product
   ```
3. Actividad de Decisión para evaluar `MovementType`:
   - **Caso `In` (Entrada):**
     - Actividad **Change Object** (`Product`):
       - Atributo `CurrentStock`: `$Product/CurrentStock + $StockMovement/Quantity`
   - **Caso `Out` (Salida):**
     - Decisión: `$Product/CurrentStock >= $StockMovement/Quantity`
     - Si es verdadero: Actividad **Change Object** (`Product`):
       - Atributo `CurrentStock`: `$Product/CurrentStock - $StockMovement/Quantity`
     - Si es falso: Actividad **Show Message** (Error: "No hay existencias suficientes para realizar la salida").
   - **Caso `Adjustment` (Ajuste):**
     - Actividad **Change Object** (`Product`):
       - Atributo `CurrentStock`: `$StockMovement/Quantity`
4. Actividad **Commit** guardando tanto `StockMovement` como `Product`.
5. Actividad **Close Page**.

---

## 3. Regla Visual de Stock Bajo

Para mostrar una alerta visual directa en el Catálogo de Productos y en el Dashboard:
- En las columnas o tarjetas del Data Grid 2, utiliza **Dynamic Classes** o **Conditional Visibility**:
  ```mendix
  $currentObject/CurrentStock <= $currentObject/MinimumStock
  ```
- Si se cumple la condición, aplica la clase CSS de Atlas UI `text-danger font-weight-bold` o muestra un badge rojo con la leyenda `"¡Stock Crítico!"`.
