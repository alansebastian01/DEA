**Amazon Kinesis** is AWS's managed service for **real-time data streaming**. Unlike Amazon Redshift, which is designed for analytics on stored data, Kinesis is designed to **collect, process, and deliver streaming data** as it is generated.

Think of it this way:

* **Kinesis** = "Move and process data in real time."
* **Redshift** = "Analyze large amounts of stored data."

## Kinesis services

Amazon Kinesis consists of several services, each for a different use case.

| Service                    | Purpose                                   | Typical use case                        |
| -------------------------- | ----------------------------------------- | --------------------------------------- |
| **Kinesis Data Streams**   | Capture and process real-time streams     | Clickstream, IoT, application logs      |
| **Kinesis Data Firehose**  | Load streaming data into destinations     | Load data into S3, Redshift, OpenSearch |
| **Kinesis Data Analytics** | Analyze streams using SQL or Apache Flink | Real-time dashboards, anomaly detection |
| **Kinesis Video Streams**  | Stream video securely                     | Security cameras, connected devices     |

---

## 1. Kinesis Data Streams

This is the core streaming service.

### How it works

```
Applications / Devices
        │
        ▼
Kinesis Data Streams
        │
        ├── Lambda
        ├── EC2
        ├── Spark
        └── Custom applications
```

Example:

* Website clicks
* Mobile app events
* Sensor data
* Stock market prices

Characteristics:

* Low latency (typically milliseconds)
* Durable storage for a configurable retention period
* Multiple consumers can read the same stream independently

Example:

```
User clicked "Buy"
↓

Event immediately sent to Kinesis Stream
↓

Lambda updates dashboard
↓

Another application stores event in S3
↓

Another application detects fraud
```

---

## 2. Kinesis Data Firehose

Firehose is the easiest way to move streaming data into storage and analytics services.

```
Applications
      │
      ▼
Firehose
      │
 ┌────┼──────────────┐
 ▼    ▼              ▼
S3  Redshift   OpenSearch
```

It automatically:

* Buffers records
* Compresses data
* Encrypts data
* Retries failed deliveries
* Scales automatically

Typical destinations:

* Amazon S3
* Amazon Redshift
* Amazon OpenSearch Service
* Splunk
* HTTP endpoints

Best when:

* You don't need custom stream processing.
* You just want to ingest and store streaming data.

---

## 3. Kinesis Data Analytics

Allows you to process streaming data in real time using SQL or Apache Flink.

Example:

```
Incoming transactions
        │
        ▼
Kinesis Data Analytics
        │
        ▼
Average sales every minute
```

Example SQL:

```sql
SELECT
    product_id,
    COUNT(*) AS purchases
FROM STREAM
GROUP BY product_id;
```

Use cases:

* Fraud detection
* Live dashboards
* Anomaly detection
* IoT monitoring

---

## 4. Kinesis Video Streams

Designed specifically for streaming video.

Example:

* Security cameras
* Smart doorbells
* Drones
* Dashcams

Unlike Data Streams, it handles video formats and metadata.

---

# Kinesis vs SQS vs Kafka

| Feature             | Kinesis            | SQS                             | Kafka (Amazon MSK)                       |
| ------------------- | ------------------ | ------------------------------- | ---------------------------------------- |
| Streaming           | ✅                  | ❌                               | ✅                                        |
| Ordered messages    | ✅ (within a shard) | FIFO queues only                | ✅ (within a partition)                   |
| Multiple consumers  | ✅                  | Limited (messages are consumed) | ✅                                        |
| Real-time analytics | ✅                  | ❌                               | ✅                                        |
| AWS managed         | ✅                  | ✅                               | Partially (managed Kafka infrastructure) |

---

# Kinesis + Redshift example

A common AWS analytics pipeline looks like this:

```
Website
    │
    ▼
Kinesis Data Streams
    │
    ▼
Kinesis Data Firehose
    │
    ▼
Amazon S3
    │
    ▼
Amazon Redshift
    │
    ▼
Power BI / Tableau / QuickSight
```

Or, if near real-time loading is required:

```
Applications
      │
      ▼
Kinesis Firehose
      │
      ▼
Amazon Redshift
      │
      ▼
Dashboards
```

---

# When to use each Kinesis service

| Requirement                                                              | Recommended service    |
| ------------------------------------------------------------------------ | ---------------------- |
| Build custom real-time processing applications                           | Kinesis Data Streams   |
| Deliver streaming data to S3, Redshift, or OpenSearch with minimal setup | Kinesis Data Firehose  |
| Perform real-time SQL or Flink analytics on streaming data               | Kinesis Data Analytics |
| Stream and process video                                                 | Kinesis Video Streams  |

In short:

* **Data Streams** is the foundation for building custom streaming applications.
* **Data Firehose** is the easiest option for ingesting and delivering streaming data to storage or analytics destinations.
* **Data Analytics** enables real-time analysis of streaming data.
* **Video Streams** is purpose-built for video ingestion and processing.

The correct answer is:

✅ **C. Configure query result reuse settings in the Athena workgroup.**

### Why C is correct

Amazon Athena has a **Query Result Reuse** feature that can reuse the results of a previous query instead of executing the query again, provided:

* The **SQL query is identical**.
* The **same parameters** are used.
* The underlying data has not changed (within the configured maximum age).

This provides two key benefits:

* **Improves performance** because Athena returns cached results instead of scanning the data again.
* **Reduces cost** because Athena charges based on the amount of data scanned, and reused results avoid rescanning the data.

Since the question specifically mentions **queries that are run multiple times with the same parameters**, this feature is exactly designed for that scenario.

---

### Why the other options are incorrect

**A. Convert the dataset to JSON format before running Athena queries.** ❌

* JSON is a text-based format and is generally **less efficient** for Athena.
* Best practice is to use **columnar formats** like Parquet or ORC, not JSON.
* JSON would likely increase scan costs and reduce performance.

---

**B. Use Amazon EMR to pre-process the data before running Athena queries.** ❌

* EMR can transform data into optimized formats, which may improve query performance.
* However, it **does not avoid re-running identical queries**, and it adds operational complexity.
* It doesn't directly address the requirement to optimize repeated queries.

---

**D. Use Amazon Redshift Spectrum to query the data in Amazon S3.** ❌

* Redshift Spectrum is intended for querying S3 data from within an Amazon Redshift data warehouse.
* It doesn't provide Athena's query result reuse capability.
* It also requires a Redshift environment, making it a different analytics solution rather than an optimization for Athena.

---

### Exam tip

If you see keywords like:

* **Athena**
* **Same query run repeatedly**
* **Same parameters**
* **Reduce cost and improve performance**

Think **Athena Query Result Reuse**.

**Answer: ✅ C**

The correct answer is:

✅ **A. Use AWS Glue Studio to build and schedule PySpark jobs. Configure an AWS Glue data connection that includes the custom libraries.**

AWS Glue is serverless, supports PySpark ETL jobs, can be scheduled, scales for large data workloads, and can write transformed data to Amazon Redshift. It also has lower operational overhead than managing EMR clusters or EC2 fleets.

Why the others are wrong:

**B. EC2 Auto Scaling** ❌
Too much operational overhead: you manage AMIs, scaling, Spark setup, failures, and scheduling.

**C. Amazon EMR** ❌
Technically works and supports bootstrap actions, but you still manage clusters more than with Glue. Not the least operational overhead.

**D. SageMaker Processing** ❌
Better for ML preprocessing workflows, not daily ETL into Redshift. Also “native SageMaker libraries” does not match the requirement for custom internal PySpark libraries.

**Answer: A**

The correct answer is:

✅ **D. Use row-level security policies with multi-condition SQL predicates. Attach the security policies to the data tables. Grant each department's role access to the appropriate policies.**

---

## Why D is correct

Let's break down the requirements:

### Requirement 1: Researchers should only see records from their own department

This is exactly what **Row-Level Security (RLS)** is designed for.

Example:

| Department | Researcher sees      |
| ---------- | -------------------- |
| Cardiology | Only Cardiology rows |
| Oncology   | Only Oncology rows   |

No separate tables or copies are needed.

---

### Requirement 2: No multiple dataset copies

RLS filters rows **at query time**.

There is:

* One table
* One copy of the data
* Different users see different rows

Perfect match.

---

### Requirement 3: Protect PII

The question mentions regulatory compliance and PII.

RLS can use **multi-condition predicates**, for example:

```sql
department = current_department
AND pii_access = true
```

or

```sql
department = current_department
AND researcher_role = 'approved'
```

Multiple conditions make the policy more secure than a simple department filter.

---

## Why the other options are wrong

### A. Redshift Data Sharing ❌

Problems:

* Data sharing is for sharing data across clusters/accounts.
* It does **not** implement row-level filtering.
* It mentions **copies of the entire dataset**, which directly violates:

> does not create multiple dataset copies

---

### B. Basic SQL filters + EXPLAIN RLS ❌

This sounds close but has two issues.

1. It specifies **basic SQL filters**, whereas the question requires protecting PII and complying with regulations. Multi-condition predicates are more appropriate.

2. **EXPLAIN RLS** permission is unrelated.

`EXPLAIN RLS` allows users to inspect how row-level security policies are applied in query plans. It is **not required** for enforcing access control.

Including it is a distractor.

---

### C. Separate schemas with views ❌

Views can filter rows, but:

* You must create and maintain many views.
* Doesn't scale well.
* More administrative overhead.
* Not as secure or flexible as native Row-Level Security.

AWS recommends RLS instead of creating many filtered views.

---

## Exam tip

If you see:

* One table
* Different users see different rows
* No duplicate data
* Department-based access

Think:

✅ **Amazon Redshift Row-Level Security (RLS)**

If security requirements include PII or compliance, expect the answer mentioning:

* **multi-condition predicates**
* **roles**
* **RLS policies**

---

### Correct Answer: ✅ D

✅ **Correct answer: A. Configure a workflow in AWS Step Functions that invokes the AWS Glue job and the Lambda functions. View the lineage in Step Functions Workflow Studio.**

Step Functions is **serverless**, can orchestrate **AWS Glue jobs** and **AWS Lambda functions**, and gives you a visual view of the full workflow in one place.

Why not the others:

**B. Apache Airflow** ❌
Airflow can orchestrate this, but it is not the lowest-overhead serverless choice. Even MWAA is managed, not fully serverless in the same way as Step Functions.

**C. Glue job + CloudWatch Logs Insights** ❌
This hides orchestration inside code and CloudWatch Logs does not provide full pipeline lineage.

**D. Step Functions + Lambda inside Glue code** ❌
The Lambda steps would not appear as separate pipeline steps in the Step Functions visual workflow, so you would not see full lineage in one interface.

**Answer: A**

✅ **Correct answer: C. Use AWS Glue DataBrew to configure profiling jobs and reusable recipe actions. Schedule the profiling jobs and reusable recipe actions to run against each dataset in Amazon S3.**

DataBrew is designed for **visual, rule-based data profiling and quality validation** with minimal custom code. It can profile CSV and JSON files, detect issues like:

* Null values
* Format inconsistencies
* Schema variations
* Outliers

Why not the others:

**A** ❌ PySpark validation works, but requires more custom code and manual maintenance.

**B** ❌ `ResolveChoice` helps handle schema ambiguity in Glue DynamicFrames, but it is not a complete reusable data quality profiling solution.

**D** ❌ Dashboards with manual review do not meet the requirement for a **repeatable, reusable, scheduled** process.

**Answer: C**

✅ **Correct Answer: A. Add the first S3 bucket and the S3 event source for the Lambda function to the SAM template. Run the `sam build` command to prepare the deployment package. Run the `sam deploy --guided` command to deploy the pipeline.**

### Why A is correct

AWS SAM is specifically designed to build and deploy serverless applications.

The workflow is:

1. Define resources in the **SAM template**:

   * S3 bucket
   * Lambda function
   * S3 event notification that triggers the Lambda function
2. Run:

   ```bash
   sam build
   ```

   This builds the application and prepares deployment artifacts.
3. Run:

   ```bash
   sam deploy --guided
   ```

   On the first deployment, `--guided` prompts for configuration (stack name, region, S3 bucket for artifacts, IAM capabilities, etc.) and then deploys the application as a CloudFormation stack. It also saves the configuration for future deployments.

This fully automates deployment of the serverless pipeline.

---

### Why the other options are incorrect

#### ❌ B. Deploy Lambda and manually configure the S3 trigger

This defeats the purpose of using SAM to manage infrastructure as code. The requirement is to **use AWS SAM for the pipeline deployment**, which includes the S3 event source.

---

#### ❌ C. Use `sam package` and manually configure notifications

`sam package` is an older workflow. While it uploads artifacts, the option still requires **manual configuration of S3 event notifications**, which is unnecessary because SAM can define them in the template.

---

#### ❌ D. Use `aws cloudformation deploy`

Although SAM ultimately uses CloudFormation, the standard deployment command for SAM applications is:

```bash
sam deploy
```

Using `aws cloudformation deploy` directly bypasses SAM's deployment features (such as handling packaged artifacts and guided deployment) and is not the recommended SAM workflow.

---

## AWS Exam Tip

For AWS SAM, remember the standard lifecycle:

```text
Write template.yaml
        ↓
sam build
        ↓
sam deploy --guided   (first deployment)
        ↓
sam deploy            (subsequent deployments)
```

If an exam question asks how to deploy a serverless application using **AWS SAM**, the answer is almost always:

* Define all resources (Lambda, API Gateway, S3 triggers, etc.) in the SAM template.
* Run `sam build`.
* Run `sam deploy --guided`.

**✅ Final Answer: A**

✅ **Correct Answer: B. `ALTER TABLE DROP PARTITION`**

### Why?

The data engineer **manually moved** the objects in Amazon S3:

* **Old partition:** `status=01` → now **empty**
* **New partition:** `status=02` → contains the data

The **AWS Glue Data Catalog** still contains metadata for the old partition because moving files in S3 does **not** automatically update the Data Catalog.

To remove the stale partition metadata, use:

```sql
ALTER TABLE table_name
DROP PARTITION (status='01');
```

---

### Why the other options are incorrect

#### ❌ A. `MSCK REPAIR TABLE`

This command **discovers and adds missing partitions** that exist in S3 but are not in the Glue Data Catalog.

It **does not remove** partitions that no longer exist.

Think of it as:

> **MSCK REPAIR = Add missing partitions only**

---

#### ❌ C. `ALTER TABLE SET TBLPROPERTIES`

Changes table properties (such as metadata settings), not partitions.

---

#### ❌ D. `ALTER TABLE CHANGE COLUMN`

Used to modify a column definition (name, type, etc.), not partition metadata.

---

## AWS Exam Tip

Remember these Athena partition commands:

| Situation                         | Command                      |
| --------------------------------- | ---------------------------- |
| New partitions added in S3        | `MSCK REPAIR TABLE`          |
| Remove stale partition metadata   | `ALTER TABLE DROP PARTITION` |
| Add a specific partition manually | `ALTER TABLE ADD PARTITION`  |

Since the **`status=01` partition is stale and needs to be removed from the catalog**, the correct answer is:

> ✅ **B. `ALTER TABLE DROP PARTITION`**

✅ **Correct answer: A. Use the Data API in the Lambda function to access the data. Set up an Amazon VPC endpoint for the Data API.**

The requirement specifically says traffic between **Lambda and the Amazon Redshift Data API** must stay on the AWS network. AWS supports an interface VPC endpoint for the Redshift Data API service name `com.amazonaws.<region>.redshift-data`; with Private DNS, Data API calls stay within the VPC/AWS network. ([AWS Documentation][1])

Why not the others:

* **B:** A VPC endpoint “for Lambda” does not make Lambda-to-Redshift Data API traffic private.
* **C/D:** ODBC/JDBC connect directly to the cluster, but the question requires traffic between Lambda and the **Redshift Data API** to remain private.

**Answer: A**

✅ **Correct answer: B. Configure an Amazon SageMaker Unified Studio data catalog project that contains the dataset with appropriate metadata and project-based access controls.**

The key clues are **discover**, **request access**, **governance controls**, and **track data lineage**. SageMaker Unified Studio supports data discovery, publishing/subscription workflows, project-based access, and lineage tracking for cataloged assets. ([AWS Documentation][1])

Why not **D**? Lake Formation is strong for cross-account governed access and tag-based controls, but by itself it does not best satisfy the **discover and request access** plus **lineage** requirements. ([AWS Documentation][2])

**Answer: B**

✅ **Correct answer: B. Use CloudFormation StackSets with service-managed permissions. Enable automatic deployments to target accounts by using an AWS Organizations integration.**

CloudFormation **StackSets** provide centralized deployment and management across multiple AWS accounts and Regions. With **service-managed permissions** and AWS Organizations integration, StackSets can automatically deploy to target accounts with much less operational overhead.

Why not the others:

* **A:** Requires managing separate stacks and permissions in each account.
* **C:** Custom Lambda automation adds unnecessary operational burden.
* **D:** Cross-account roles and custom deployment logic are more manual than StackSets.

**Answer: B**

A company has a data lake in Amazon S3 and uses AWS Glue across multiple AWS accounts. The company needs to control access to tables and databases in the AWS Glue Data Catalog. Specific departments need the ability to securely share data with each other. The company requires fine-grained access control that the company can manage based on department ownership.

Which solution will meet these requirements?

A. Create IAM roles for each department with policies that grant access to specific AWS Glue databases and tables. Use resource-based policies on S3 buckets for cross-account data access.
B. Register all S3 locations with AWS Lake Formation. Use Lake Formation tag-based access control (LF-TBAC) to assign permissions to databases and tables.
C. Create AWS Resource Access Manager (AWS RAM) resource shares for each database and table. Grant access to specific AWS accounts and IAM principals by using AWS RAM.
D. Enable AWS Lake Formation hybrid access mode. Use AWS Glue resource policies to share catalogs and databases across accounts and maintain IAM permissions for existing workloads.

✅ **Correct answer: A.**

Use **AWS Backup** to:

* Schedule monthly Aurora snapshots
* Copy backups to a secondary Region
* Apply lifecycle settings to transition backups to cold storage after **180 days**

This has the **least effect on database performance** because it uses managed snapshot-based backups, not database export tools.

Why not the others:

* **B:** Aurora automated backups are continuous/PITR, not the best fit for monthly managed backup lifecycle, and S3 Lifecycle does not apply to Aurora snapshots.
* **C:** Exporting snapshots to S3 adds unnecessary steps.
* **D:** `mysqldump` from EC2 would significantly affect database performance.

**Answer: A**

**B. Use AWS Glue jobs with job bookmarks enabled to process the data with automatic scaling based on workload.**

Lambda is a poor fit because processing can take **up to 30 minutes**, beyond Lambda’s typical event-processing fit. AWS Glue supports complex ETL, scales with workload, and **job bookmarks** help ensure each file is processed only once with low operational overhead.

**B. Write a custom grok classifier to match the predefined column names and schema.**

For text files with a changing structure and **predefined column names**, an **AWS Glue crawler with a custom grok classifier** can infer/update the schema and create/update the Data Catalog table for Athena and Quick Suite with less manual work than updating Athena tables directly.

**B. Create an Amazon Bedrock knowledge base linked to an Amazon S3 bucket that contains survey data. Use Retrieval Augmented Generation (RAG) to analyze feedback trends.**

Bedrock Knowledge Bases support natural-language Q&A, follow-up questions, summarization, and evidence-backed answers from your own data. It also keeps the solution managed within AWS, giving the **least operational overhead** compared with custom ML, EMR, Lambda, or manual indexing.

**B. Create a domain by using the Amazon SageMaker Catalog. Define business glossary terms and publish data assets from the AWS Glue Data Catalog to the SageMaker Catalog.**

This is the best fit for a **business data catalog** because it supports:

* business glossary terms
* business context on technical metadata
* data asset publishing/discovery
* governance controls
* organization across AWS data sources

AWS Glue Data Catalog is mainly technical metadata; SageMaker Catalog adds the business catalog and governance layer.


