# Mendix Integrations & REST APIs

This document specifies the integration patterns, published REST services, bulk import/export capabilities, and webhook events for external ERP, eCommerce, and logistics connectivity.

---

## 1. Published REST Services Architecture

All endpoints are published using Mendix's native **Published REST Service** feature under the `/api/v1/` prefix.

- **Authentication**:
  - `OAuth2 Bearer Token` (preferred for enterprise ERP like SAP, Dynamics 365)
  - `API Key header` (`X-API-KEY: <secret_hash>`) for automated batch jobs and lightweight webhook listeners.
- **Content-Type**: `application/json; charset=utf-8`
- **Error Standard**: RFC 7807 (Problem Details for HTTP APIs).

---

## 2. API Endpoints Specification

### A. Retrieve Product Catalog: `GET /api/v1/products`
Retrieves paginated catalog with real-time aggregated inventory.
- **Query Parameters**:
  - `page` (Integer, default 1)
  - `pageSize` (Integer, default 50, max 250)
  - `updatedSince` (ISO 8601 DateTime, e.g. `2026-11-01T00:00:00Z`)
  - `category` (String)
- **Response `200 OK`**:
  ```json
  {
    "totalCount": 1240,
    "page": 1,
    "pageSize": 50,
    "data": [
      {
        "sku": "VALVE-IND-01",
        "name": "High Pressure Industrial Valve",
        "barcode": "012345678905",
        "category": "Valves & Actuators",
        "unitOfMeasure": "PCS",
        "costPrice": 45.50,
        "sellingPrice": 89.99,
        "totalOnHand": 350.0,
        "totalReserved": 40.0,
        "totalAvailable": 310.0,
        "isActive": true
      }
    ]
  }
  ```

### B. Query Stock by SKU & Location: `GET /api/v1/stock/{sku}`
- **Path Parameter**: `sku` (e.g. `VALVE-IND-01`)
- **Query Parameter**: `warehouseCode` (Optional: filter to specific warehouse)
- **Response `200 OK`**:
  ```json
  {
    "sku": "VALVE-IND-01",
    "warehouses": [
      {
        "warehouseCode": "WH-CHICAGO",
        "warehouseName": "Chicago Central Logistics Hub",
        "onHand": 250.0,
        "reserved": 30.0,
        "available": 220.0,
        "bins": [
          { "binCode": "CHI-A01-B04", "quantity": 150.0 },
          { "binCode": "CHI-A02-B10", "quantity": 100.0 }
        ]
      }
    ]
  }
  ```

### C. Create Outbound Sales Order: `POST /api/v1/orders`
Invoked by external e-commerce (Shopify, Magento, Salesforce Commerce).
- **Request Payload**:
  ```json
  {
    "externalOrderNumber": "ORD-2026-99881",
    "customerName": "Acme Manufacturing Ltd",
    "shippingAddress": "123 Industrial Parkway, Sector 4, Chicago IL 60601",
    "lines": [
      {
        "sku": "VALVE-IND-01",
        "quantity": 10.0,
        "unitPrice": 89.99
      }
    ]
  }
  ```
- **Response `201 Created`**:
  ```json
  {
    "salesOrderId": "SO-00452",
    "externalOrderNumber": "ORD-2026-99881",
    "status": "StockReserved",
    "reservedFully": true,
    "createdAt": "2026-11-04T10:30:00Z"
  }
  ```

---

## 3. Bulk Data Migration & Excel Importer

For importing existing catalogs and opening inventory balances from legacy systems:

1. **Module**: Mendix **Excel Importer** (Marketplace).
2. **Template**: `Template_ProductCatalogImport`:
   - Maps column headers: `SKU`, `Name`, `CategoryCode`, `Barcode`, `CostPrice`, `SellingPrice`, `UoM`.
   - Action on missing Category/UoM: Create new or abort row.
3. **Chunking & Transaction Management**:
   - Use Mendix batch loops processing **250 rows per transaction** to prevent JVM Heap exhaustion.
   - Any validation failure writes a record to `ImportErrorLog` (Line number, SKU, Error Reason) without terminating the entire import job.

---

## 4. Webhook Notification Engine

Enables event-driven architecture by dispatching HTTP POST payloads to registered webhook subscribers.

### Supported Events
- `inventory.threshold.low`: Triggered when an item's net stock drops below `ReorderPoint`.
- `inventory.adjustment.approved`: Triggered when an adjustment changes stock balances.
- `procurement.receipt.completed`: Triggered when goods are checked into warehouse bins.
- `order.fulfillment.shipped`: Triggered when carrier tracking is assigned.

### Webhook Delivery Specification
- **Security**: Request includes header `X-IMS-Signature: sha256=<hmac>` computed with subscriber secret key.
- **Retry Mechanism**: Executed via **Mendix Task Queue** with exponential backoff:
  - Attempt 1: Immediate
  - Attempt 2: 10 seconds
  - Attempt 3: 2 minutes
  - Attempt 4: 15 minutes (Final attempt before marking `DeadLetter`).
