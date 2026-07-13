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


