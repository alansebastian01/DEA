The correct answer is:

✅ **A. Use API calls to access and integrate third-party datasets from AWS Data Exchange.**

### ADHD-friendly way to remember it

#### What is the question asking?

The company wants to:

* 📊 Get **third-party data** (data from other companies)
* ⚡ Add it to their analytics platform easily
* 🛠️ **Do the least amount of work** (least operational overhead)

### Think:

**"Where does AWS sell/share third-party datasets?"**

➡️ **AWS Data Exchange**

It's literally built for buying, subscribing to, and using third-party datasets.

You can access the datasets through **APIs** and integrate them into your applications.

---

### Why the others are wrong

**❌ B. AWS DataSync**

* DataSync = **moves files**
* Think: **moving data from one storage location to another**
* Not for finding or subscribing to third-party datasets.

**Memory trick:**
📦 DataSync = Moving boxes.

---

**❌ C. Kinesis + Git repository**

* Kinesis = **streaming real-time data**
* Git = stores **source code**, not data marketplaces.

**Memory trick:**
🌊 Kinesis = River of live data.

---

**❌ D. Kinesis + Amazon ECR**

* Amazon ECR = **Docker container images**
* Doesn't store datasets.

**Memory trick:**
🐳 ECR = Container warehouse.

---

## Easy comparison

| Service             | What it does            |
| ------------------- | ----------------------- |
| ✅ AWS Data Exchange | Third-party datasets    |
| DataSync            | Move files/storage      |
| Kinesis             | Stream live data        |
| Git                 | Source code             |
| ECR                 | Docker/container images |

---

### Exam shortcut

If you see:

> **third-party datasets**, **marketplace**, **external data providers**, **financial data**, **weather data**, **healthcare datasets**

👉 Your brain should immediately think:

> **AWS Data Exchange** ✅

That's exactly what it's designed for, making **A** the solution with the **least operational overhead**.


✅ **Correct Answer: B. Configure a data protection policy. Attach the policy to the CloudWatch log group.**

## ADHD Simple Concept

### Step 1: What's the problem?

The ETL job writes logs to **CloudWatch Logs**.

Those logs contain **PII** (names, SSNs, credit card numbers).

The requirement is:

> **Mask PII in CloudWatch Logs.**

### Step 2: Where is the problem?

📍 **CloudWatch Logs**

So the solution should be a **CloudWatch Logs feature**, not a Glue feature.

➡️ **CloudWatch Logs Data Protection Policy** automatically detects and masks sensitive data in log events.

**Answer: B** ✅

---

## Why the others are wrong

### ❌ A. AWS Glue Security Configuration

Purpose:

* Encrypts data
* Encrypts job bookmarks
* Encrypts CloudWatch logs

It **encrypts the log storage**, but it **does not mask PII inside the log messages**.

**Think:**
🔒 Encryption ≠ Masking

---

### ❌ C. Amazon Macie

Macie:

* Finds sensitive data in **Amazon S3**
* Does **not** protect CloudWatch Logs

**Think:**
📦 Macie = S3 scanner

---

### ❌ D. AWS Glue Sensitive Data Detection APIs

These APIs help identify sensitive data **during ETL processing**.

The question is **not** asking to detect PII in the data pipeline. It asks to **mask PII in CloudWatch Logs**.

Using CloudWatch's built-in data protection policy is simpler and requires less effort.

---

## Quick Memory Table

| If you see...              | Think...                           |
| -------------------------- | ---------------------------------- |
| PII in **CloudWatch Logs** | ✅ Data Protection Policy           |
| PII in **S3**              | Amazon Macie                       |
| Encrypt Glue job/logs      | Glue Security Configuration        |
| Detect PII during ETL      | Glue Sensitive Data Detection APIs |

### 🎯 Exam Tip

Whenever the question says:

* **CloudWatch Logs**
* **Mask sensitive data**
* **PII in logs**

Your brain should immediately think:

> **CloudWatch Logs Data Protection Policy** ✅


