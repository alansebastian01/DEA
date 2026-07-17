# Amazon Kinesis Services (Study Notes)

## What is Amazon Kinesis?

**Amazon Kinesis** is an AWS service used to **collect, process, and analyze real-time streaming data**. It enables applications to process data as it arrives instead of waiting for batch processing.

---

# Main Amazon Kinesis Services

## 1. Amazon Kinesis Data Streams

* Captures and stores real-time streaming data.
* Allows multiple applications to process the same data simultaneously.
* Data is stored temporarily for replay if needed.

**Use Cases:**

* Website clickstream analysis
* IoT sensor data
* Log processing
* Financial transactions

---

## 2. Amazon Kinesis Data Firehose

* Fully managed service for delivering streaming data.
* Automatically loads data into destinations such as:

  * Amazon S3
  * Amazon Redshift
  * Amazon OpenSearch Service
  * Third-party tools (e.g., Splunk)

**Features:**

* No infrastructure management
* Automatic scaling
* Optional data transformation before delivery

**Use Cases:**

* Data lake ingestion
* Log storage
* Analytics

---

## 3. Amazon Kinesis Data Analytics

* Analyzes streaming data in real time.
* Supports SQL and Apache Flink applications.
* Detects patterns, trends, and anomalies.

**Use Cases:**

* Real-time dashboards
* Fraud detection
* Monitoring and alerts
* Live analytics

---

## 4. Amazon Kinesis Video Streams

* Captures, stores, and processes video streams.
* Supports video from cameras, smartphones, and IoT devices.
* Integrates with AWS AI services for video analysis.

**Use Cases:**

* Security surveillance
* Smart homes
* Video analytics
* Machine learning applications

---

# Kinesis Services Summary

| Service            | Purpose                                                  |
| ------------------ | -------------------------------------------------------- |
| **Data Streams**   | Collect and process real-time streaming data             |
| **Data Firehose**  | Deliver streaming data to storage and analytics services |
| **Data Analytics** | Analyze streaming data using SQL or Apache Flink         |
| **Video Streams**  | Capture and process real-time video streams              |

---

# Advantages of Amazon Kinesis

* Real-time data processing
* Highly scalable
* Fully managed by AWS
* Integrates with services like S3, Redshift, Lambda, and OpenSearch
* Suitable for analytics, monitoring, and IoT applications

---

# Common Applications

* Real-time dashboards
* Website clickstream analysis
* IoT sensor monitoring
* Fraud detection
* Log and event processing
* Video surveillance and analytics

---

# Quick Exam Summary

* **Kinesis Data Streams** → Collects and processes real-time data streams.
* **Kinesis Data Firehose** → Delivers streaming data to destinations like S3 and Redshift.
* **Kinesis Data Analytics** → Performs real-time analysis using SQL or Apache Flink.
* **Kinesis Video Streams** → Captures and processes video streams.
* **Key Benefit:** Enables **real-time** ingestion, processing, analysis, and delivery of streaming data.
