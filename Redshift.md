# Amazon Redshift Services (Study Notes)

## What is Amazon Redshift?

**Amazon Redshift** is a **fully managed cloud data warehouse** service from AWS. It is designed to store and analyze **large amounts of structured and semi-structured data** using SQL.

**Main Purpose:** Fast analytics and business intelligence (BI) on large datasets.

---

# Main Amazon Redshift Features/Services

## 1. Redshift Clusters

* A cluster is a collection of one or more compute nodes.
* Stores data and executes SQL queries.
* The cluster has a **leader node** and one or more **compute nodes**.

**Purpose:** Process and store data for analytics.

---

## 2. Redshift Spectrum

* Queries data stored directly in **Amazon S3** without loading it into Redshift.
* Uses standard SQL.

**Purpose:** Analyze data in S3 alongside data in Redshift.

---

## 3. Redshift Serverless

* No need to create or manage clusters.
* AWS automatically provisions and scales resources.
* You pay only for the compute resources used.

**Purpose:** Simplify data warehousing without infrastructure management.

---

## 4. Redshift ML

* Integrates with **Amazon SageMaker** to create and use machine learning models with SQL.
* Allows predictions directly from Redshift.

**Purpose:** Add machine learning capabilities to analytics.

---

## 5. Redshift Data Sharing

* Securely share live data across Redshift clusters or AWS accounts.
* No need to copy or move the data.

**Purpose:** Enable collaboration and data sharing.

---

# Redshift Architecture

```text
Users / BI Tools
        │
        ▼
   Leader Node
        │
        ▼
  Compute Nodes
        │
        ▼
   Data Storage
```

* **Leader Node:** Receives SQL queries, creates execution plans, and coordinates work.
* **Compute Nodes:** Store data and execute query operations.

---

# Advantages of Amazon Redshift

* Fast SQL query performance
* Fully managed by AWS
* Scalable storage and compute
* Integrates with S3, Glue, QuickSight, and Athena
* Supports petabyte-scale data warehouses

---

# Common Use Cases

* Business Intelligence (BI)
* Data warehousing
* Reporting and dashboards
* Customer behavior analysis
* Financial analytics
* Log and clickstream analysis

---

# Summary Table

| Redshift Feature/Service | Purpose                                  |
| ------------------------ | ---------------------------------------- |
| **Clusters**             | Store and process warehouse data         |
| **Spectrum**             | Query data directly in Amazon S3         |
| **Serverless**           | Data warehouse without managing clusters |
| **Redshift ML**          | Build and use ML models with SQL         |
| **Data Sharing**         | Share live data securely without copying |

---

# Quick Exam Summary

* **Amazon Redshift** = AWS **data warehouse** service.
* **Clusters** = Store and process data.
* **Spectrum** = Query data in **S3** without loading it.
* **Serverless** = No cluster management required.
* **Redshift ML** = Machine learning using SQL.
* **Data Sharing** = Share live data across clusters/accounts.
* **Leader Node** = Manages queries.
* **Compute Nodes** = Store data and execute queries.
