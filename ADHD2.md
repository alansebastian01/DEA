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

