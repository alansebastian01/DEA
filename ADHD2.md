**Answer: C.**

Use **AWS Glue DataBrew** with:

```text
S3 CSV/JSON files
   ↓
DataBrew dataset
   ↓
Reusable profiling job + reusable recipe rules
   ↓
Scheduled daily run
   ↓
Quality metrics: nulls, format issues, outliers, schema variations
```

Why: DataBrew is designed for **visual, reusable, rule-based data profiling and cleaning** with minimal custom code.

A and B require more custom ETL logic.
D relies on **manual review**, but the requirement asks for a repeatable, scalable process.

**Answer: B.**

Use **Amazon EMR on Amazon EKS + KEDA**.

```text id="ze63ni"
Streaming source
   ↓
Spark streaming application
   ↓
Amazon EMR on Amazon EKS
   ↓
KEDA monitors event metrics
   ↓
Kubernetes scales Spark workloads up/down
   ↓
Better resource utilization
```

Why B: **EMR on EKS** reduces Spark management overhead, while **KEDA** provides event-driven autoscaling for Kubernetes workloads based on external events or metrics. AWS describes KEDA on EKS as a way to scale applications based on custom/event metrics instead of only CPU or memory. ([AWS Documentation][1])

Why not C: Cluster Autoscaler mainly scales **nodes**, not Spark applications directly based on event-driven workload demand.

Why not A/D: self-managed Spark or custom scaling policies add more operational overhead.

✅ **D. Use Amazon Athena Federated Query to perform one-time joins and analysis across DynamoDB, Amazon RDS, Amazon Redshift, and Amazon S3.**

Athena Federated Query lets you query and join data across multiple sources without first moving all the data into one place. That fits the requirements:

* **One-time report**
* **Avoid unnecessary data movement**
* **Join data across DynamoDB, RDS, Redshift, and S3**
* **Minimize setup and query time**

Why not the others:

* **A** moves data into S3 unnecessarily.
* **B** uses ETL and centralizes data in S3, which adds movement and setup.
* **C** requires provisioning EMR and ingesting/merging datasets, too heavy for a one-time report.

So the best answer is **D**.

✅ **Correct answer: D. Deploy separate Amazon Redshift clusters for ETL and analytics workloads. Use cross-database queries and data sharing to maintain data consistency.**

### Why D is correct

The problem is:

* ETL jobs are **updating tables**.
* Analytical queries are **running concurrently**.
* This causes **table-level locks, deadlocks, and query timeouts**.
* The business needs **consistent data views three times per day**.

Amazon Redshift **Data Sharing** is designed for this scenario.

With this architecture:

* **Producer cluster (ETL cluster):** Runs ETL jobs and updates the data.
* **Consumer cluster (Analytics cluster):** Runs BI and analytical queries.
* **Redshift Data Sharing** lets the analytics cluster access the data **without copying it**, while isolating the workloads.

This eliminates contention between ETL writes and analytical reads while providing consistent snapshots of the shared data.

---

### Why the other options are incorrect

#### ❌ A. Materialized views + schedule ETL off-peak

* Materialized views can improve query performance, **but they don't eliminate locking during ETL**.
* Scheduling ETL during off-peak hours doesn't satisfy the requirement that queries and ETL run concurrently.

---

#### ❌ B. Federated queries + read replicas

* **Redshift does not have read replicas.**
* Federated queries are for querying external data sources (such as Amazon RDS or Aurora), not for solving locking issues within Redshift.

This option includes a feature that doesn't exist.

---

#### ❌ C. Redshift Spectrum

* Spectrum queries **data in Amazon S3**, not Redshift tables.
* The problem is contention on **Redshift tables**, not querying data stored in S3.

Spectrum does not address the locking issue.

---

## AWS Exam Tip

When you see:

* ETL and analytics competing for the same Redshift tables
* Lock contention, deadlocks, or query timeouts
* Need to isolate workloads without duplicating data

Think:

> ✅ **Amazon Redshift Data Sharing with separate producer and consumer clusters**

This is the AWS-recommended architecture for separating write-heavy ETL workloads from read-heavy analytics workloads.

**Answer: ✅ D**

✅ **Correct answer: D. Write an Amazon Redshift stored procedure that deletes the stale records from the orders table and inserts new records from the staging table.**

This is a typical **Redshift upsert/merge pattern**:

1. Delete matching or stale records from the target `orders` table.
2. Insert the latest records from the `staging` table.
3. Wrap the logic in a stored procedure/transaction so downstream apps see updated data consistently.

Why not the others:

* **A:** Redshift Spectrum queries external S3 data; it is not for updating native Redshift tables.
* **B:** Unloading to S3 and using Athena adds unnecessary data movement and complexity.
* **C:** Athena federated queries are for querying external sources, not managing Redshift table updates.

**Answer: D**

