# Spark Session
- As of Spark V2 Spark session is a **`single unified entry point`** to all resources of spark.
- **`PySpark`** is a wrapper around **`Spark Core`**, so when we start a spark session, pyspark uses **`PY4J`** in the background to create a **`Java Virtual Machine (JVM)`** and creates a **`Java Spark Context`**.
- PY4J allows python programs to dynamically access **Java Objects** in **Java Virtual Machine**.
- 