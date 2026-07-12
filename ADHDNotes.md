This is a classic **DEA-C01 "keyword matching"** question. Let's break it down the ADHD way.

---

# 🟨 Step 1: Highlight the keywords

> **third-party datasets**

🚨 Stop here.

Think:

> "Where does AWS sell/share third-party datasets?"

✅ **AWS Data Exchange**

---

> **incorporate insights**

Means:

They just need to **consume data**, not build a streaming platform.

---

> **least operational overhead**

Translation:

👉 AWS should manage everything.

No custom pipelines.

No infrastructure.

---

# Step 2: Eliminate wrong answers

## ❌ B. AWS DataSync

Think:

> 🚚 Truck for moving files

Example:

```
On-prem
     ↓
AWS
```

DataSync **copies data**.

It **does not provide third-party datasets**.

❌ Wrong service.

---

## ❌ C. Kinesis + Git repository

Kinesis =

```
Live events

Click
↓

Click
↓

Click
```

Git repositories store **source code**, not commercial datasets.

Makes no sense.

❌ Wrong.

---

## ❌ D. Kinesis + ECR

ECR =

Docker container images.

```
Docker image

↓

Deploy application
```

No datasets.

❌ Wrong.

---

# ✅ A. AWS Data Exchange

AWS Data Exchange is literally:

> "Marketplace for third-party datasets."

Providers publish datasets like:

* Financial data
* Weather
* Healthcare
* Demographics
* Consumer behavior
* Satellite imagery

You subscribe.

AWS handles updates.

Minimal effort.

Exactly what the question asks.

---

# 🎯 Answer

✅ **A. Use API calls to access and integrate third-party datasets from AWS Data Exchange.**

---

# 🧠 ADHD Memory Trick

Whenever you see:

🟢 **third-party datasets**
🟢 **commercial datasets**
🟢 **external data providers**
🟢 **marketplace data**
🟢 **licensed datasets**

👉 Your brain should immediately think:

> **AWS Data Exchange = App Store for data**

---

# 🚨 Tiny Exam Trap

Many people see **"integrate data"** and instinctively choose **Kinesis** or **DataSync**.

Ask yourself first:

> **"Where is the data coming from?"**

* **Third-party provider** → **AWS Data Exchange**
* **On-premises files** → **DataSync**
* **Continuous event stream** → **Kinesis**
* **Existing database migration** → **DMS**

A data engineer needs a fully automated solution to check for new data in multiple databases and process data that the solution finds. The solution must run every hour. The solution must be compatible with Amazon RDS, Amazon DynamoDB, and Amazon OpenSearch Service. The solution must be able to process up to 10 MB of data at one time. The solution must be optimized for costs and operational overhead. The solution must have robust error handling capabilities.

Which solution will meet these requirements?

A. Use Amazon EventBridge to invoke AWS Step Functions every hour to deploy an AWS Lambda function to check for data. Configure Step Functions steps to process data that the Lambda function finds. Implement error handling in each state.
B. Use Amazon EventBridge to invoke an AWS Lambda function every hour to check for data. Configure the function to send a message to an Amazon Simple Queue Service (Amazon SQS) queue when the function finds new data. Use a second Lambda function to read the queue and perform the processing.
C. Configure an Apache Spark application to run on Amazon EMR to check for data. Implement error handling in the application. Use Amazon EventBridge to invoke the application every hour.
D. Use Amazon Managed Workflows for Apache Airflow (Amazon MWAA) to create a workflow that runs a directed acyclic graph (DAG) every hour to check for data. Configure the DAG to process identified data. Implement error handling in a Python operator.

That one question often narrows the correct service immediately.
