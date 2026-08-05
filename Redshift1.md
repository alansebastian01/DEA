| Feature                 | Purpose                                                               | Memory Trick                    |
| ----------------------- | --------------------------------------------------------------------- | ------------------------------- |
| **COPY**                | Load data **from S3 (or DynamoDB/EMR)** into Redshift                 | **S3 → Redshift** 📥            |
| **UNLOAD**              | Export data **from Redshift to S3**                                   | **Redshift → S3** 📤            |
| **Redshift Spectrum**   | Query **S3 data without loading it** into Redshift                    | **Read S3** 🔍                  |
| **Federated Query**     | Query live data in **PostgreSQL/Aurora RDS**                          | **Read databases** 🔗           |
| **Data Sharing**        | Share live Redshift data with another Redshift cluster/workgroup      | **Redshift ↔ Redshift** 🤝      |
| **Materialized Views**  | Store precomputed query results for faster queries                    | **Cache results** ⚡             |
| **Concurrency Scaling** | Automatically add transient capacity for spikes in concurrent queries | **More users** 🚀               |
| **RA3 Nodes**           | Managed storage with independent compute and storage scaling          | **Scale storage separately** 💾 |
| **AQUA**                | Hardware-accelerated query processing for some workloads              | **Faster analytics** ⚡          |
| **Redshift ML**         | Train and use ML models from SQL                                      | **ML in SQL** 🤖                |
