# Domain Model & Business Rules

This document defines the core business domain, entities, and rules for the ERP Core Supply Chain Module.

## 1. Business Context

The system manages the complete lifecycle of products in a supply chain environment, from catalog management through inventory control to order fulfillment with dynamic tax calculation.

## 2. Core Domain Aggregates

### 2.1 Product Aggregate

The Product aggregate is the central catalog entity containing master data for items managed in the system.

**Entities:**
- `Product` (Aggregate Root)
- `PriceHistory` (Entity)

**Value Objects:**
- `Sku` - Stock Keeping Unit identifier
- `Dimensions` - Physical dimensions (weight, size)
- `Ncm` - Nomenclatura Comum do Mercosul (tax classification code)

**Invariants:**
- SKU must be unique and non-empty
- Weight must be greater than zero
- Inactive products cannot be used in new transactions but must be preserved in historical reports (RF003)
- Price changes must maintain audit trail (RF002)

### 2.2 Inventory/Stock Aggregate

Manages multi-warehouse stock positions and transactional movements.

**Entities:**
- `Warehouse` (Aggregate Root)
- `StockPosition` (Entity)
- `StockTransaction` (Entity - Kardex record)

**Value Objects:**
- `MovementType` - Entry/Exit classification
- `TransactionCost` - Cost snapshot at transaction time

**Invariants:**
- Stock transactions are immutable after creation (RF005)
- Weighted average cost must be recalculated on each purchase entry (RF006)
- Concurrent reservations must not exceed available balance - prevention of overselling (RF007)
- Multi-warehouse support is required (RF004)

### 2.3 Tax Engine Aggregate

Handles dynamic tax calculation based on configurable rules.

**Entities:**
- `TaxRule` (Aggregate Root)
- `TaxZone` (Entity)

**Business Rules:**
- Tax calculation uses a decision matrix: Origin State × Destination State × NCM → Tax Rate (RF008)
- Orders must persist a snapshot of tax calculations at time of sale for compliance (RF009)

### 2.4 Sales Order Aggregate

Manages the order lifecycle from draft to fulfillment.

**Entities:**
- `Order` (Aggregate Root)
- `OrderItem` (Entity)
- `TaxSnapshot` (Entity)

**State Machine:**
```
Draft → Validated → Approved → Invoiced
                 ↘ Cancelled
```

**Invariants:**
- Orders with delinquent customers cannot be approved (RF011)
- Orders with profit margin below configured minimum cannot be approved (RF011)
- Approval operations must be idempotent to prevent duplicate stock deductions (RF012)

## 3. Domain Services

### 3.1 Tax Calculator Service

Calculates taxes based on the rule matrix. Must implement Strategy or Chain of Responsibility pattern for rule selection.

**Input:** Order with items, origin, destination
**Output:** Calculated tax values with applied rates

### 3.2 Inventory Service

Coordinates stock movements with concurrency control.

**Responsibilities:**
- Balance verification before deduction
- Weighted average cost recalculation
- Optimistic/pessimistic locking implementation

## 4. Non-Functional Requirements

### 4.1 Data Integrity
- All financial and inventory operations must be ACID compliant (RNF001)
- Failures in tax calculation must rollback stock reservations

### 4.2 Performance
- Inventory position reports must execute in under 200ms using optimized SQL (RNF002)
- Read operations should use Dapper/Raw SQL, not Entity Framework

### 4.3 Audit
- Critical entity changes (Price, Stock) must generate audit logs with UserId, Timestamp, OldValue, NewValue (RNF004)

## 5. Reporting Requirements

The following reports must be implemented using advanced SQL (Window Functions, CTEs):

| Report | Description | SQL Features |
|--------|-------------|--------------|
| ABC Curve | Product classification by revenue relevance (A=80%, B=15%, C=5%) | RANK(), PARTITION BY |
| Inventory Turnover | Analysis of product exit velocity vs. average balance | Aggregations, CTEs |
| Kardex Extract | Chronological and sequential view of all entries and exits for a specific SKU | Pagination, Ordering |

## 6. Architecture Constraints

1. **Domain Layer Independence:** No framework dependencies (EF Core, Dapper) in Domain layer
2. **Rich Domain Models:** Entities must contain business methods, not just getters/setters
3. **Separation of Concerns:** Controllers handle HTTP, Services orchestrate, Domain enforces rules
4. **Result Pattern:** Use Result objects for validation errors instead of exceptions
