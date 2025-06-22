# 🏞️ Guide: How Data Lake Architecture Works

A **Data Lake** is a centralized repository that stores raw data in its native format (structured, semi-structured, unstructured) for future analysis, ML, or operational processing.

This guide covers:
- What a data lake is (vs a data warehouse)
- Its layered architecture
- How it handles storage, processing, and querying
- Common tech stacks and best practices

---

## 1. 📚 What Is a Data Lake?

A **Data Lake**:
- Stores **any kind of data** (CSV, JSON, video, logs, binaries)
- Uses **cheap, scalable storage** (like S3, HDFS, or Azure Blob)
- Separates **storage from compute**
- Is schema-on-read: schema is applied when you query

Compare to a **Data Warehouse**:
```
| Feature               | Data Lake                      | Data Warehouse                   |
|-----------------------|---------------------------------|----------------------------------|
| Data Types            | Structured + semi/unstructured | Mostly structured                |
| Schema                | Schema-on-read                 | Schema-on-write                  |
| Storage               | Cheap object storage           | Columnar formats (Parquet, etc.) |
| Query Performance     | Slower                         | Fast OLAP                        |
| Use Cases             | ML, raw logs, staging           | BI, dashboards, reporting        |
```
---

## 2. 🧱 Data Lake Architecture: Key Layers

Data Lakes are typically organized in **4 logical layers**:

### 1️⃣ **Ingestion Layer**
Responsible for collecting and importing raw data from multiple sources:

- Batch: `CSV`, `JSON`, `Parquet`, database dumps
- Stream: Kafka, Kinesis, Flink
- Sources: APIs, DBs, devices, logs

```
# Example: ingesting Kafka stream to S3
kafka-console-consumer --topic logs --bootstrap-server localhost:9092 > logs.json
aws s3 cp logs.json s3://data-lake/raw/logs/
```

---

### 2️⃣ **Storage Layer**
Stores all raw and processed data in cheap, scalable storage (e.g. object store).

- Examples: Amazon S3, Azure Data Lake Storage, Hadoop HDFS
- Usually organized in **zones**:
```
| Zone        | Description                       |
|-------------|-----------------------------------|
| Raw         | Original, untransformed data      |
| Staging     | Cleaned but not validated         |
| Curated     | Validated, enriched, trusted      |
| Sandbox     | Ad-hoc, temporary explorations    |
```
---

### 3️⃣ **Processing Layer**
Transforms data for downstream use (ETL/ELT):

- Batch tools: Apache Spark, AWS Glue, dbt
- Stream tools: Apache Flink, Kafka Streams, Spark Structured Streaming

```
# Example: Spark job to clean CSV data
df = spark.read.csv("s3://data-lake/raw/sales.csv", header=True)
df_clean = df.dropna().filter("amount > 0")
df_clean.write.parquet("s3://data-lake/curated/sales/")
```

---

### 4️⃣ **Consumption Layer**
Exposes data to users/tools for querying, ML, analytics:

- SQL engines: Presto, Trino, Athena
- ML: SageMaker, Databricks, TensorFlow
- BI: Tableau, Superset, Looker

```
-- Query S3 using Athena
SELECT COUNT(*) FROM curated.sales WHERE region = 'EU';
```

---

## 3. 🗃️ Storage Formats & Metadata

Data lakes use open formats:
```
| Format     | Type         | Notes                        |
|------------|--------------|------------------------------|
| CSV/JSON   | Text         | Simple, not efficient        |
| Parquet    | Columnar     | Compressed, splittable       |
| Avro       | Row-based    | Good for streaming           |
| Delta Lake | Transactional| Adds ACID to data lakes      |
| Iceberg    | Transactional| Schema evolution + versioning|
```
Also: Catalog systems like **Apache Hive Metastore** or **AWS Glue Catalog** manage table schemas and partitions.

---

## 4. 🚢 ETL vs ELT in Data Lakes
```
| ETL                              | ELT                               |
|----------------------------------|------------------------------------|
| Transform data **before** loading | Load raw data, **transform later** |
| Used in warehouses                | Preferred in data lakes            |
| Slower ingestion                  | Faster ingestion, lazy transforms  |
```
Data Lakes favor **ELT** to keep ingest simple and defer transformation to query time.

---

## 5. 🔍 Querying Data Lakes

- Use **SQL-on-object-store engines** (Presto, Athena, Trino)
- Push queries down to files (Parquet/ORC optimized)
- Can join large datasets across S3 buckets or folders

```
-- Query a partitioned dataset in Athena
SELECT region, SUM(sales)
FROM "sales_data"
WHERE year = 2024 AND month = '06'
GROUP BY region;
```

---

## 6. ✅ Benefits of Data Lakes

- Ingest anything: structured, semi-structured, binary
- Decouples storage & compute
- Very cheap storage (S3 vs Redshift)
- Easy to integrate with ML pipelines
- Supports real-time + batch workloads

---

## 7. ⚠️ Challenges & Pitfalls
```
| Challenge                 | Mitigation Strategy                        |
|---------------------------|--------------------------------------------|
| Data swamp (unorganized)  | Enforce naming, folder structure, catalog  |
| Performance on large reads| Use columnar formats, partitioning         |
| Lack of ACID              | Use Delta Lake / Apache Iceberg            |
| Access control complexity | Centralize IAM policies, use lakehouse     |
```
---

## 8. 🔄 Lakehouse: The Evolution

**Lakehouse** = Data Lake + Data Warehouse

- Combines open formats with SQL and ACID support
- Technologies: Delta Lake, Apache Iceberg, Apache Hudi
- Popular in ML + BI hybrid systems

---

## 📚 Further Reading

- [AWS Data Lake Architecture](https://aws.amazon.com/big-data/datalakes-and-analytics/)
- [Apache Iceberg](https://iceberg.apache.org/)
- [Delta Lake](https://delta.io/)
- [Trino (formerly Presto)](https://trino.io/)
- [Data Mesh vs Data Lake](https://martinfowler.com/articles/data-mesh-principles.html)

---
