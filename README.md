# Supply Chain & Inventory Management System (SCIMS) - MySQL

A production-ready relational database project designed to manage multi-warehouse inventory, suppliers, transactional orders, and automated stock safety audits using advanced MySQL features.

## 📊 Database Schema Architecture

The database is built on 6 core normalized tables ensuring referential integrity and safe transaction handling:

- **suppliers:** Tracks vendor details, contact persons, and country locations.
- **warehouses:** Manages storage locations, capacity limits, and facility managers.
- **products:** Stores catalog items linked to specific suppliers and low-stock reorder thresholds.
- **inventory:** A bridge table handling the many-to-many relationship between warehouses and product stock quantities.
- **customers:** Separates clients into retail, wholesale, and corporate segments.
- **orders & order_items:** Transactional tables tracking order headers, fulfillment status, and line items.

---

## ⚙️ Advanced SQL Implementation Features

### 1. Automated Stock Validation Trigger
- **`trg_validate_stock_before_order`**: Executes before an order item is inserted. It checks the exact stock available in the selected warehouse and terminates the transaction (`SIGNAL SQLSTATE`) if the requested quantity exceeds available stock.

### 2. Transaction-Safe Order Processing Procedure
- **`sp_place_order`**: Handles order placement atomically using `START TRANSACTION`, `COMMIT`, and error handlers (`ROLLBACK`). It creates order headers, adds line items, and deducts inventory quantities safely in a single execution block.

### 3. Business Intelligence & Analytics
- **Category Performance & Ranking:** Utilizes Common Table Expressions (CTEs) and window functions (`DENSE_RANK() OVER (PARTITION BY ... ORDER BY ... DESC)`) to rank products by revenue within their respective categories.
- **Low-Stock Monitoring:** Uses conditional `CASE` statements to flag critical stock levels that require immediate replenishment.
