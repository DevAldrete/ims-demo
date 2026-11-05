# Seguridad y Roles Básicos en Mendix

Para una práctica o demo académica en Mendix, la seguridad debe configurarse de forma clara pero suficiente para demostrar el control de acceso basado en roles (RBAC).

---

## 1. Nivel de Seguridad del Proyecto (Project Security)

En Mendix Studio Pro:
- Abre **App Security** en el Project Explorer.
- Selecciona el nivel de seguridad: **Prototype / Demo** o **Production**.
  - *Recomendación para la práctica:* **Prototype/demo** permite probar rápidamente las páginas; si deseas verificar las restricciones exactas por rol, selecciona **Production**.

---

## 2. Definición de Roles

Para este demo solo se necesitan **dos roles de usuario**:

1. **`Administrator` (Administrador de Sistema):**
   - Acceso total a la configuración, creación de categorías, almacenes y eliminación de productos.
2. **`WarehouseOperator` (Operador de Almacén):**
   - Acceso a consultar el catálogo de productos y registrar movimientos de stock (entradas y salidas).
   - No puede eliminar productos ni alterar categorías.

---

## 3. Matriz de Permisos por Entidad (Module Security)

Dentro de la pestaña **Security** del módulo `InventoryManagement`:

| Entidad | Administrator | WarehouseOperator |
| :--- | :--- | :--- |
| **`Product`** | Lectura y Escritura Total | Lectura de todos los atributos; sin permisos de eliminación |
| **`Category`** | Lectura y Escritura Total | Solo Lectura |
| **`Location`** | Lectura y Escritura Total | Solo Lectura |
| **`StockMovement`** | Lectura y Escritura Total | Puede crear nuevos movimientos y ver el historial |

---

## 4. Permisos de Ejecución de Microflows

- **`ACT_Product_Save`**: Permitido para `Administrator`.
- **`ACT_StockMovement_Process`**: Permitido tanto para `Administrator` como para `WarehouseOperator`.
