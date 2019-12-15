# Resilient Distributed Dataset
RDD is spark's primary **data abstraction**
## RDD
- Is a **fault tolerant collection of elements** that can be operated on in parallel.
- They are the **primary units of data** in Spark and are **immutable**.
- The creation of RDD, creates a Direct Acyclic Graph (DAG), and this operation is called transformations.
- **Transformations are lazy operations**, which means that they don't do anything, they just store what the transformation will do.
- Transformations make updates to DAG which means that each transformation results in a new RDD, but nothing actually happens untill an action is called (i.e. during transformation, new RDD is not created, rather information is stored about the transformation which will result in the creation of a new RDD).
- **Transformations return pointers to the RDD created and Actions return value**.
-  The notion here is that the graphs can be replayed on nodes that need to get back to the state it was before it went offline, thus providing fault tolerance. Using DAG, RDD is able to re-compute missing partitions.
- **Actions** are final RDD operations. At first it loads the data into the original RDD, then performs all intermediate transformation jobs and finally returns to the driver program. Some spark actions are `first()`,`count()`, `collect()`, `take()`, and `reduce()`.
- There are 3 methods for creating an RDD.
    - Parallalize an existing collection. This means using an existing collection available in spark and distributing it across nodes, so that it can be operated on in parallel. eg. RDD can be created out of an array by calling `parallelize()` method on it.
    - The 2nd method is to reference a dataset. The dataset can come from any datasource support by spark such as HDFS, AWS S3, Cassandra, HBase etc.
    - Creating an RDD from another one.
- Types of files supported:
    - Text files.
    - Sequence files.
    - Hadoop Input Format.
- Creating RDD in scala
```
!.bin/spark-shell
val data = 1 to 1000
# sc ➡ spark context
val distData = sc.parallelize(data)
distData.filter(...)
# create an RDD from an external dataset
val readme = sc.textFile('filename')
```
## Direct Acyclic Graph (DAG)
- A DAG is essentially a graph of the business logic and does not get executed untill an action is called, which is known as **lazy evaluation**.
- To print the DAG after a series of transformations we call the `RDD.toDebugString()` method.
- Sample DAG
```
res4: String = 
MappedRDD[3] at map at <console>:16 (3 partitions)
    FilteredRDD[2] at filter at <console>:14 (3 partitions)
        MappedRDD[1] at textFile at <console>:12 (3 partitions)
            HadoopRDD[0]  at textFile at <console>:12 (3 partitions)
```
## Fault tolerance
- The behavior of DAG allows for fault tolerance. If a node goes offline and comes back on, all it has to do is just grab a copy of DAG from a neighboring node and rebuild the graph back to where it was before it went offline.
## Executors
- performs the transformations and actions on data blocks on each worker node.
## RDD Persistence
- The **cache function** is the default of the persist function with the **MEMORY_ONLY** storage. One of the key capabilities of spark is its speed through persisting or caching.
- Each node stores any partitions of the cache and computes it in memory, when a subsequent action is called on the dataset, it uses it from memory instead of having to retrieve it from disk, which results in actions being 10 times faster. **Caching is fault tolerant**.
- There are 2 methods to invoke RDD Persistence.
    1. `persist()` allows us to specify a specific storage level of caching.
    2. `cache()` essentially just persist with MEMORY_ONLY storage.
- The different storage levels are
    - MEMORY_ONLY
    - MEMORY_AND_DISK
    - MEMORY_ONLY_SER
    - MEMORY_AND_DISK_SER
    - DISK_ONLY
    - MEMORY_ONLY_2
    - OFF_HEAP store data in serialized format in tachyon
- If a partition doesn't fit in a specified cache location it will be recomputed on the fly.
- OFF_HEAP reduces garbage collection overhead and allows the executors to be small and to share a pool of memory.
## Shared Variables and key value pairs
- spark provides 2 types of shared variables for common usage patterns,
    1. Broadcast variables
    2. Accumulators
- Normally when a function is passed from the driver to the workers, a separate copy of the variables is used for each worker.
- Broadcast variables allows each node to work with a **read-only variable, cached** on each machine.
- Broadcast variables are distributed using efficient algorithms.
- Accumulators are used for counters in sums that works well in parallel. These variables can only be added through an associated operation. Only drivers are allowed to read its value. Workers can only add to it.
- RDDs can be made up of key,value pairs.
- Special operations are available for RDDs of key,value pairs such as grouping and aggregating.