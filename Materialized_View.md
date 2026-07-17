Here are concise, study-friendly notes on **Materialized Views** in databases.

# Materialized Views (Database Notes)

## Definition

A **Materialized View (MV)** is a database object that **stores the result of a query physically on disk**. Unlike a regular view, the data is precomputed and saved.

**Think of it as:**

* **View** = Saved SQL query (no stored data)
* **Materialized View** = Saved SQL query + Stored query results

---

## Why Use Materialized Views?

Materialized views improve **query performance** by avoiding repeated execution of expensive queries.

They are useful for:

* Large datasets
* Complex joins
* Aggregations (SUM, COUNT, AVG)
* Reporting
* Data warehouses
* Business intelligence (BI)

---

## How It Works

1. Execute a complex query.
2. Store the results in a materialized view.
3. Future queries read the stored data instead of recalculating everything.

---

## Example

### Original Query

```sql
SELECT department,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

Instead of calculating this every time:

```sql
CREATE MATERIALIZED VIEW dept_avg_salary AS
SELECT department,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

Now users simply query:

```sql
SELECT * FROM dept_avg_salary;
```

---

# Materialized View vs Regular View

| Feature           | View           | Materialized View         |
| ----------------- | -------------- | ------------------------- |
| Stores data       | ❌ No           | ✅ Yes                     |
| Uses disk space   | ❌ No           | ✅ Yes                     |
| Query speed       | Slower         | Faster                    |
| Always up-to-date | ✅ Yes          | ❌ Only after refresh      |
| Best for          | Simple queries | Complex queries & reports |

---

## Advantages

* Faster query execution
* Reduces CPU usage
* Improves reporting performance
* Good for analytical workloads
* Can precompute expensive calculations

---

## Disadvantages

* Uses additional storage
* Data can become outdated
* Requires refresh operations
* Maintenance overhead

---

## Refresh Methods

### 1. Complete Refresh

Deletes all stored data and rebuilds the materialized view.

**Pros:**

* Simple
* Accurate

**Cons:**

* Slow for large datasets

---

### 2. Fast (Incremental) Refresh

Only updates the rows that changed.

**Pros:**

* Faster
* Less resource usage

**Cons:**

* Requires additional setup (such as change tracking or logs)

---

### 3. On Demand Refresh

Refresh happens only when explicitly requested.

Example:

```sql
REFRESH MATERIALIZED VIEW dept_avg_salary;
```

---

### 4. Automatic/Scheduled Refresh

The database refreshes the materialized view at scheduled intervals (for example, every hour or every night).

---

## Common Uses

* Sales reports
* Monthly revenue summaries
* Dashboard analytics
* Data warehouses
* Business intelligence
* Frequently accessed aggregated data

---

## Example Scenario

Suppose a company has **10 million sales records**.

A report calculates:

```sql
SELECT product_id,
       SUM(amount)
FROM sales
GROUP BY product_id;
```

Running this query repeatedly is expensive.

Instead:

* Create a materialized view once.
* Store the results.
* Users query the materialized view for much faster performance.
* Refresh it periodically to keep it reasonably current.

---

## Key Points to Remember

* Materialized views **store query results** physically.
* They **improve performance** by avoiding repeated computation.
* They **consume storage space**.
* They **must be refreshed** to reflect changes in the underlying tables.
* Best suited for **read-heavy**, **analytical**, and **reporting** workloads.

---

# Quick Exam Summary

* **Materialized View = Stored query results**
* **Regular View = Stored SQL query only**
* **Main benefit:** Faster query performance
* **Main drawback:** Data may become stale until refreshed
* **Best use cases:** Data warehousing, reporting, dashboards, and complex aggregations
