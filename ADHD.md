✅ **Correct Answer: C. Use Apache Airflow on Amazon Managed Workflows for Apache Airflow (Amazon MWAA) to create DAGs to manage ETL workflows.**

## ADHD Simple Concept

The question says:

* Complex ETL workflows
* AWS Glue + Amazon EMR
* Redshift + S3
* Dependency management
* Automated retries
* Monitoring
* Fully managed orchestration

That screams:

> **Amazon MWAA = managed Airflow for complex data pipelines** ✅

## Why C is best

**Airflow DAGs** are designed for ETL orchestration.

They can manage:

* Task dependencies
* Retries
* Scheduling
* Monitoring
* Glue jobs
* EMR jobs
* S3 and Redshift pipeline steps

## Why not A?

**Step Functions** can orchestrate workflows, but for **complex ETL pipelines with dependency management and monitoring**, AWS usually points to **Amazon MWAA / Apache Airflow**.

Also, **Express Workflows** are better for high-volume, short-duration event processing, not long-running complex ETL orchestration.

## Why not B or D?

❌ **B. Lambda + EventBridge**
Too much custom orchestration logic.

❌ **D. One Lambda scheduled daily**
Not scalable, not good for complex workflows, and Lambda has runtime limits.

## Memory trick

**Complex data pipeline + DAG + retries + monitoring = MWAA** ✅

**Answer: C**


✅ **Correct Answer: B. Enable Amazon Q in Quick Suite. Generate Quick Suite dashboards and reports.**

## ADHD Simple Concept

### What does the question want?

* Analysts **don't know SQL** ❌
* They want to ask questions in **natural language** 💬
* Automatically create dashboards and reports 📊
* **Least operational effort**

### Keyword to spot

> **Natural language queries**

Whenever you see this in AWS analytics exams, think:

> **Amazon Q** 🤖

Amazon Q in Quick Suite (formerly QuickSight) lets users ask questions like:

> "Show sales by region for the last quarter."

No SQL required.

---

## Why B is correct

✅ Amazon Q understands natural language.

✅ Generates dashboards, visuals, and insights.

✅ Built into Quick Suite.

✅ Minimal setup.

---

## Why the others are wrong

### ❌ A. Zero-ETL access

Zero-ETL is about **moving/synchronizing data**, not natural-language report creation.

---

### ❌ C. Tableau

Tableau is a BI tool, but it:

* Adds another product to manage.
* Doesn't satisfy the AWS-native, least-effort requirement as well as Amazon Q in Quick Suite.

---

### ❌ D. Federated query

Federated queries allow querying data from multiple sources.

They **do not** let non-technical users create dashboards using natural language.

---

## Quick Memory Table

| Keyword in question       | Think                   |
| ------------------------- | ----------------------- |
| Natural language reports  | ✅ Amazon Q              |
| Dashboards in Quick Suite | Amazon Q in Quick Suite |
| No SQL knowledge          | Amazon Q                |
| AI-generated insights     | Amazon Q                |

### 🎯 Exam Shortcut

If you see:

* **No SQL**
* **Natural language**
* **Generate dashboards/reports**

Your brain should immediately think:

> **Amazon Q in Quick Suite** ✅

**Answer: B**

**Answer: C.**

```text id="rxz1a9"
Amazon SNS topics
   ↓
CloudTrail data events enabled for SNS
   ↓
Capture Publish and PublishBatch API actions
   ↓
CloudTrail Lake event data store
   ↓
Security + application teams query audit data in CloudTrail Lake
```

Why: **SNS `Publish` and `PublishBatch` are CloudTrail data events**, not management events. Since the company already has **CloudTrail Lake**, querying there gives the least operational overhead—no Glue table or Athena setup needed. ([Repost][1])

[1]: https://repost.aws/knowledge-center/sns-log-data-events-cloudtrail?utm_source=chatgpt.com "Configure SNS data event logging in CloudTrail | AWS re:Post"

