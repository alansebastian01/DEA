**Data lineage** in data engineering is the ability to track where data comes from, how it changes, and where it goes throughout its lifecycle.

Think of it as a **map or family tree for data** that answers questions like:

* Where did this data originate?
* Which systems and tables did it pass through?
* What transformations were applied?
* Which reports, dashboards, or machine learning models use it?

### Example

Suppose a company generates a sales dashboard.

```
Sales Database
       │
       ▼
Raw Sales Table
       │
   Clean duplicates
       │
       ▼
Clean Sales Table
       │
   Join with Customer Table
       │
       ▼
Sales Analytics Table
       │
       ▼
Power BI Dashboard
```

The lineage shows every step from the original sales records to the dashboard.

---

### Types of Data Lineage

1. **Source-to-Target Lineage**

   * Tracks data from its origin to its final destination.
   * Example:

     ```
     MySQL → Spark → Snowflake → Tableau
     ```

2. **Column-Level Lineage**

   * Tracks how individual columns are derived.
   * Example:

     ```
     total_price = quantity × unit_price
     ```

3. **Table-Level Lineage**

   * Shows relationships between tables.
   * Example:

     ```
     customers
           \
            --> customer_orders
           /
     orders
     ```

4. **End-to-End Lineage**

   * Covers the entire pipeline across multiple systems.

---

### Why Data Lineage Is Important

**Debugging**

* If a dashboard shows incorrect values, lineage helps identify where the problem was introduced.

**Impact Analysis**

* Before changing or deleting a table, you can see what downstream systems depend on it.

**Compliance**

* Regulations like GDPR or HIPAA often require organizations to know where sensitive data originates and how it is used.

**Trust**

* Analysts and business users can understand how metrics are calculated, increasing confidence in the data.

---

### Real-World Example

Imagine an e-commerce company.

```
Shopify Orders
      │
      ▼
Kafka
      │
      ▼
Spark ETL
      │
      ▼
Delta Lake
      │
      ▼
Snowflake
      │
      ▼
Looker Dashboard
```

If the dashboard suddenly reports incorrect revenue, lineage allows engineers to trace the issue backward:

* Was the SQL transformation wrong?
* Did Spark drop some records?
* Did Kafka miss events?
* Did Shopify send incorrect data?

Without lineage, troubleshooting can be much slower.

---

### How Lineage Is Captured

Organizations capture lineage in several ways:

* **ETL/ELT tools** automatically record pipeline dependencies (for example, dbt or Azure Data Factory).
* **Metadata catalogs** collect and visualize lineage information (for example, Microsoft Purview, DataHub, Apache Atlas, Collibra).
* **Query parsing** analyzes SQL to infer how data flows between tables and columns.
* **Workflow orchestration tools** like Apache Airflow provide pipeline-level dependencies.

---

### Example in SQL

Suppose you have:

```sql
CREATE TABLE monthly_sales AS
SELECT
    customer_id,
    SUM(order_amount) AS total_sales
FROM orders
GROUP BY customer_id;
```

The lineage would show:

```
orders.order_amount
        │
        ▼
SUM(order_amount)
        │
        ▼
monthly_sales.total_sales
```

---

### Summary

Data lineage is the record of a dataset's journey from its source through every transformation to its final use. It helps engineers understand data flows, troubleshoot issues, assess the impact of changes, meet compliance requirements, and build trust in analytical results. It can be tracked at the pipeline, table, or even individual column level, making it a foundational capability in modern data engineering.
