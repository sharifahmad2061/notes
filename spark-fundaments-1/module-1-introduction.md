# Introduction to Spark
## Purpose of Spark
Apache Spark is a computing platform designed to be fast, general purpose and easy to use. It provides **Parallel distributed processing**, **fault tolerance on commodity hardware**, **Scalability**, **aggressively cached in-memory computing**, **low latency**, **high level APIs** and a stack of high level tools.
- The **speed of Spark** comes from **in-memory** computations. It is faster than MapReduce for complex applications on disk.
- Spark is **General purpose** as it covers wide range of applications from **Batch processing jobs** to **Iterative Algorithms** (i.e. building on top of one another) and **Interactive queries and Streaming**.
- Spark is **easy to use** as it provides **APIs for different languages** like Python, Java & Scale. It provides **libraries** for **SQL, Machine Learning, Streaming and Graph Processing**.
- Spark can run as a **Standalone System** or on top of **Hadoop Clusters** such as **Hadoop YARN** or **Apache MESOS**.
## Components of **Spark Unified Stack**
|||||
|:-:|:-:|:-:|:-:|
|Spark SQL|Spark <br> Streaming <br> Real time Processing | MLlib <br> machine learning| GraphX <br> graph processing|
|| Spark Core ||
|Standalone Scheduler||YARN|Mesos|
- Spark Core is at the center of the Spark Unified Stack and is a general purpose system providing **scheduling, distributing and monitoring** of applications across the cluster.
- The Spark core can scale from 1 to 1000 nodes and can run on custom managers like Hadoop YARN or Apache Mesos as well as standalone with its own scheduler.
- **Spark SQL** works with spark via **SQL and HiveQL** (a Hive variant of SQL) and it allows developers to intermix Spark programming languages with SQL.
- The Spark Streaming provides processing of live streams of Data. Spark Streaming API closely resembles Spark Core API, making it easy for developers to move between applications that process data stored in memory to real-time data processing.
- **MLlib** is the machine learning library that provides multiple machine learning algorithms . They are designed to scale out across the cluster.
- **GraphX** is a graph processing library with APIs to manipulate graphs and perform graph parallel computations.
## Basics of Resilient Distributed DataSet (RDD)
- RDD is Spark's **primary core abstraction**. It is a **Distributed collection of elements** that is **distributed across the cluster**.
- There are 2 types of RDD Operations.
    1. Transformations
        - During transformations spark creates a **Direct Acyclic Graphs (DAG)** which are **evaluated at runtime**, called **Lazy evaluation** and **don't return a value**.
    2. Actions
        - Actions are when the transformations gets evaluated along with the action that is called for that RDD.
        - Actions **return values**.
- **Fault tolerance** aspect of RDD allows Spark to reconstruct the transformations used to build the lineage to get back the data.
- **Caching** is provided with spark so that data is processed in-memory. If it does not memory, data is spilled to Disk.
- **Data flow of RDD**.
    - First the hadoop dataset is loaded.
    - then we apply successive transformations on it. such as **Filter, Map Reduce**.
    - Nothing actually happens untill an action is called. The DAG is updated is continuously updated during each transformation.
## Basics of **Scala**
- Everything in Scale is an Object. Numbers such as int,float and booleans are objects. Functions are also objects, They can be stored in variables, passed to functions.
- 1 + 2 ➡➡➡ (1).+(2)
- function definition:
    - def function-name ([parameters]) : [return value]
- anonymous functions:
    - (parameters) => {statements}