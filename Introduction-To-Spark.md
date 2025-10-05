# Introduction to Apache Spark

**Apache Spark** is an open-source, unified analytics engine designed for **large-scale data processing**.  
It provides high performance for both **batch** and **streaming** data workloads through in-memory computation and an optimized execution engine.

While Spark can integrate with the **Hadoop ecosystem**, it is **not limited to Hadoop** — it can run on various cluster managers (YARN, Kubernetes, Mesos, or standalone) and access data from multiple storage systems.


## Key Characteristics of Spark

- **Cluster computing framework** for distributed data processing.  
- **In-memory execution** allows faster computation than traditional disk-based systems like Hadoop MapReduce.  
- **Compatible with multiple data sources:** HDFS, S3, Azure Data Lake, GCS, JDBC, Kafka, and more.  
- **Multi-language APIs:** Python (PySpark), Scala, Java, SQL, and R.  
- **Unified engine** supporting SQL, streaming, machine learning, and graph processing.


## Spark Applications Overview

A **Spark application** consists of a **driver program** that coordinates distributed tasks running on a **cluster**.  
The driver creates a `SparkContext` (or more commonly, a `SparkSession`) that connects to a cluster manager and distributes work across worker nodes.

### Components of a Spark Application
1. **Driver Program:** Defines the main logic and creates the SparkSession.  
2. **Cluster Manager:** Allocates resources across applications (YARN, Kubernetes, Mesos, or standalone).  
3. **Executors:** Run tasks on worker nodes and store data in memory or disk.  

> The **SparkSession** is the entry point for all Spark functionalities in modern versions.

## SparkSession

In Spark 2.0 and later, **SparkSession** became the unified entry point for all Spark APIs.  
It replaces older contexts such as:

- `SparkContext` (core engine)
- `SQLContext` (for Spark SQL)
- `HiveContext` (for Hive queries)
- `StreamingContext` (for DStreams)

**Example:**
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("ExampleApp") \
    .getOrCreate()
```

## RDD (Resilient Distributed Dataset)

An RDD is the fundamental data structure in Spark — an immutable, distributed collection of objects that can be processed in parallel across a cluster.

### RDD Workflow Example

1. Initialize a Spark environment (e.g., PySpark session).
2. Load data from files or external sources into RDDs or DataFrames.
3. Apply transformations and actions to perform computations.

### Types of RDD Operations
- Transformations: Create a new dataset from an existing one.
  
  (e.g., map(), filter(), flatMap())

- Actions: Trigger computation and return results.

  (e.g., count(), collect(), saveAsTextFile())

Note:
- Transformations in Spark are lazy — they build a Directed Acyclic Graph
  (DAG) of execution but are not computed until an action is called.
- RDDs are immutable and partitioned across nodes, enabling fault tolerance and parallel processing.
- You can chain multiple transformations before executing an action.

## Submitting a Spark Job

Use the `spark-submit` command to run your Spark application on a cluster:
```bash
spark-submit \
  --master yarn \
  --deploy-mode cluster \
  your_spark_app.py
```

[Previous Page](High-Performance-Computing.md) -  [Next Page](Apache-Spark-Hands-On.md)



