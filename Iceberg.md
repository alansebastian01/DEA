Apache Iceberg is now an important topic for the **AWS Certified Data Engineer – Associate (DEA-C01)** exam. AWS explicitly added **"Manage open table formats (for example Apache Iceberg)"** to the exam guide, so you should expect conceptual and scenario-based questions. ([AWS Documentation][1])

## What is Apache Iceberg?

Apache Iceberg is an **open table format** for data lakes. It stores data in files (typically Parquet, ORC, or Avro) on Amazon S3 while maintaining metadata that enables database-like features.

Think of it as:

* **S3** = Storage
* **Parquet** = File format
* **Iceberg** = Table format (metadata + transactions)

## Why use Iceberg?

Without Iceberg:

* Data is just files in S3.
* Updates and deletes are difficult.
* Schema changes are painful.
* No transactions.

With Iceberg:

* ACID transactions
* Schema evolution
* Time travel
* Partition evolution
* Better query performance

## Features to know for DEA-C01

| Feature             | Meaning                               | Exam Importance |
| ------------------- | ------------------------------------- | --------------- |
| ACID Transactions   | Safe INSERT, UPDATE, DELETE, MERGE    | ⭐⭐⭐⭐⭐           |
| Time Travel         | Query older snapshots                 | ⭐⭐⭐⭐⭐           |
| Schema Evolution    | Add/remove/rename columns             | ⭐⭐⭐⭐            |
| Hidden Partitioning | Users don't specify partitions        | ⭐⭐⭐⭐            |
| Partition Evolution | Change partition strategy later       | ⭐⭐⭐             |
| Snapshot Isolation  | Readers aren't affected during writes | ⭐⭐⭐⭐            |

## AWS services that work with Iceberg

### Amazon Athena

* Read and write Iceberg tables
* Supports ACID operations
* SQL examples:

  * INSERT
  * UPDATE
  * DELETE
  * MERGE

### AWS Glue

Glue Data Catalog stores Iceberg metadata.

Glue ETL jobs can:

* Create Iceberg tables
* Read Iceberg tables
* Write Iceberg tables

### Amazon EMR

Spark on EMR has excellent Iceberg support.

Typical flow:

```
Spark
      ↓
Iceberg Table
      ↓
Amazon S3
```

### Amazon Redshift

Redshift Spectrum can query Iceberg tables stored in S3.

## Iceberg architecture

```
Applications
      │
Athena / Spark / EMR / Trino
      │
Iceberg Metadata
      │
Manifest Files
      │
Parquet Files
      │
Amazon S3
```

The actual data stays in S3; Iceberg manages metadata and snapshots. ([mindmeshacademy.com][2])

## Time Travel example

Suppose a table changes every day.

```
Monday Snapshot
Tuesday Snapshot
Wednesday Snapshot
Thursday Snapshot
```

You can query:

```
SELECT *
FROM sales
FOR VERSION AS OF 12345;
```

or by timestamp to view the table as it existed previously.

**Exam takeaway:** Time travel is useful for auditing, debugging, and recovering historical views.

## Schema Evolution

Suppose the table is:

```
id
name
salary
```

Later you add:

```
department
```

Iceberg updates metadata instead of rewriting all data files.

## Hidden Partitioning

Traditional partitioning:

```
/year=2025/month=01/day=20/
```

Users must know partition columns.

Iceberg:

Users simply write:

```sql
SELECT *
FROM orders
WHERE order_date='2026-07-30';
```

Iceberg automatically prunes partitions based on metadata.

## Partition Evolution

Initially:

```
Partition by Month
```

Later:

```
Partition by Day
```

Iceberg allows changing the partition strategy without rewriting all historical data.

## ACID Transactions

Iceberg supports:

* INSERT
* UPDATE
* DELETE
* MERGE

This makes data lakes behave more like traditional databases.

## Common exam scenario

**Question**

A company stores Parquet files in S3. They need:

* Updates
* Deletes
* Schema evolution
* Time travel

**Best answer**

✅ Apache Iceberg

## Iceberg vs Plain Parquet

| Plain Parquet            | Iceberg              |
| ------------------------ | -------------------- |
| Just files               | Managed table format |
| No transactions          | ACID transactions    |
| No time travel           | Time travel          |
| Difficult schema changes | Schema evolution     |
| Manual partitions        | Hidden partitioning  |

## Iceberg vs Delta Lake vs Hudi

For DEA-C01:

* **Apache Iceberg** is explicitly in scope.
* Delta Lake and Apache Hudi may be mentioned conceptually, but AWS guidance now specifically emphasizes Iceberg as the example open table format to know. ([AWS Documentation][1])

## Memorize these exam points

* ✅ Iceberg is an **open table format**, not a storage service.
* ✅ Data remains in **Amazon S3**.
* ✅ Supports **ACID transactions**.
* ✅ Supports **time travel**.
* ✅ Supports **schema evolution**.
* ✅ Supports **partition evolution**.
* ✅ Integrates with **Athena**, **Glue**, **EMR/Spark**, and **Redshift Spectrum**.
* ✅ Uses metadata and snapshots to enable efficient queries and consistent reads. ([mindmeshacademy.com][2])

These are the concepts most likely to appear in DEA-C01 scenario-based questions.

[1]: https://docs.aws.amazon.com/aws-certification/latest/data-engineer-associate-01/dea-01-revisions.html?utm_source=chatgpt.com "Revisions - AWS Certified Data Engineer - Associate"
[2]: https://www.mindmeshacademy.com/certifications/aws/aws-certified-data-engineer-associate/study-guide/3-2-2-open-table-formats-apache-iceberg-and-s3-tables?utm_source=chatgpt.com "DEA-C01 Study Guide | Open Table Formats: Apache Iceberg..."
