# Mendix Security, Roles & Access Control

This document outlines the security architecture, Role-Based Access Control (RBAC) matrix, and XPath entity access constraints for the Inventory Management System (IMS).

---

## 1. Project Security Configuration

In Mendix Studio Pro:
- **Security level**: Set to **Production** (enforcing complete entity access rules and microflow execution rights).
- **Check security**: Enabled for all user roles.
- **Password policy**: Enforce standard enterprise complexity (minimum 8 characters, alphanumeric + symbol, 90-day expiry).

---

## 2. User Roles & Module Roles Mapping

Mendix decouples administrative User Roles (assigned to user accounts) from Module Roles (assigned within each module domain).

### User Roles
1. **`Administrator`**: Full system administration, user provisioning, global configurations.
2. **`InventoryManager`**: Complete operational authority over inventory, warehouses, adjustments, variance approvals, and catalog definitions.
3. **`WarehouseClerk`**: Floor operations: receiving shipments, executing pick lists, packing orders, performing blind cycle counts, and moving bins.
4. **`PurchasingAgent`**: Vendor management, Purchase Order drafting and issuance, receipt tracking.
5. **`Auditor`**: Read-only oversight across transactions, inventory ledgers, variance logs, and valuation metrics.

### Module Role Mapping Matrix

| Module | Administrator | InventoryManager | WarehouseClerk | PurchasingAgent | Auditor |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`MasterData`** | `Admin` | `Manager` | `Viewer` | `Viewer` | `Viewer` |
| **`Inventory`** | `Admin` | `Manager` | `Operator` | `Viewer` | `Auditor` |
| **`Procurement`**| `Admin` | `Manager` | `Receiver` | `Buyer` | `Viewer` |
| **`Fulfillment`**| `Admin` | `Manager` | `PickerPacker` | `None` | `Viewer` |
| **`InventoryAudit`**| `Admin` | `Manager` | `Counter` | `None` | `Auditor` |
| **`IMS_Integration`**| `Admin` | `ApiConsumer` | `None` | `None` | `None` |

---

## 3. Entity Access Rules & XPath Constraints

### A. Scoping Warehouse Clerks to Assigned Facilities
To prevent a clerk in Chicago from inadvertently picking or modifying stock in Dallas, link `Administration.Account` to `MasterData.Warehouse`.

```xpath
// Entity Access Rule on StockLevel for WarehouseClerk:
[StockLevel_LocationBin/LocationBin_Zone/Zone_Warehouse = '[%CurrentUser%]/IMS_UI.Account_AssignedWarehouse']
```

### B. High-Value Stock Adjustment Protection
A `WarehouseClerk` can propose adjustments, but only an `InventoryManager` can commit adjustments that exceed a financial threshold ($500 USD).

```xpath
// Entity Access Rule on StockAdjustment for WarehouseClerk:
[Status = 'Draft' or Status = 'Submitted']

// Entity Access Rule on StockAdjustment for InventoryManager:
[true()] // Can view, approve, reject, or execute all adjustments
```

### C. Blind Cycle Counting Integrity
During cycle counts, `WarehouseClerk` (acting as `Counter`) MUST NOT see the expected system balance (`SystemSnapshotQuantity`), preventing confirmation bias:

- In `InventoryAudit.CycleCountLine`:
  - **Module Role `Counter`**:
    - `CountedQuantity`: Read/Write
    - `Notes`: Read/Write
    - `SystemSnapshotQuantity`: **No Access** (Attribute hidden)
    - `VarianceQuantity`: **No Access** (Attribute hidden)
    - `VarianceValue`: **No Access** (Attribute hidden)
  - **Module Role `Manager` / `Auditor`**:
    - Full Read access to all attributes.

---

## 4. Microflow Execution Rights

| Microflow | Allowed Module Roles | Purpose |
| :--- | :--- | :--- |
| `ACT_StockAdjustment_Approve` | `Inventory.Manager`, `Admin` | Finalizes adjustment and calls transaction engine |
| `ACT_GoodsReceipt_Confirm` | `Procurement.Receiver`, `Inventory.Manager` | Posts incoming receipt and allocates to bins |
| `ACT_CycleCount_ApproveVariance`| `InventoryAudit.Manager`, `Admin` | Commits cycle count reconciliation |
| `ACT_PurchaseOrder_Issue` | `Procurement.Buyer`, `Inventory.Manager` | Issues PO to vendor via email / EDI |
| `ACT_SalesOrder_Dispatch` | `Fulfillment.PickerPacker`, `Inventory.Manager` | Decrements on-hand stock and issues tracking |
| `SE_EvaluateReorderThresholds` | `System` (Scheduled Event) | Background job evaluated under system user |

---

## 5. Audit Trail & Data Tampering Prevention

1. **Immutable Transactions**:
   - `InventoryTransaction` entity has **Read-Only** access for ALL roles (including `Administrator`).
   - Creation of `InventoryTransaction` is ONLY permitted via internal sub-microflow `SUB_UpdateStockLevel` running with **"Apply entity access = False"** within a server-side transaction.
2. **Change Tracking**:
   - The Mendix **AuditTrail** Marketplace component is bound to `Product`, `PurchaseOrder`, `SalesOrder`, and `StockAdjustment`.
   - Any attribute modification records the old value, new value, timestamp, and changing user.
