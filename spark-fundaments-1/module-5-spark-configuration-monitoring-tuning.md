# Spark Configuration, monitoring & tuning

## Cluster Overview
- There are 3 main components of the Spark Cluster
    - `Driver` where the SparkContext is located within the main program.
    - To run on a cluster we need a `cluster manager`, this could either be Spark Standalone cluster manager, Mesos or YARN.
    - `Worker Nodes` Where the `Executors` resides. These are the processes that run computations and store the data for the applications.
- Diagram

![spark cluster components](components-of-spark.png 'components of spark cluster')

- The spark Context sends each executor the application defined as either `jar` or `python` files. finally it sends each executor the tasks it has to run.
- There are 3 locations for `Spark Configuration`:
    - We have the `Spark Properties` where the application parameters can be set using the `SparkConf` Object or through `Java System Properties`
    - The 2nd method is through `environment variables` which can be used to set `per machine setting` such as IP addresses. This can be done through `conf/spark-env.sh`.
    - Then there are the `logging properties` which can set using `log4j.properties`
- We can override the default configuration directory (i.e. `SPARK_HOME/conf`) by setting the `SPARK_CONF_DIR` environment variable to the desired path.
    - We have to provide our custom configuration files under that directory
    - spark-defaults.conf
    - log4j.properties
    - spark-env.sh
    - etc.
- > The Spark Shell output is verbose and therefore can be changed from `INFO` to `ERROR` in the `$SPARK_HOME/conf/log4j.properties`.
- There are 2 methods of setting spark properties.
    - The 1st method is by passing application properties using the `SparkConf` which is used to instantiate `SparkContext` Object.
    ```
    val conf = new SparkConf()
                .setMaster('local').setAppName('word-count')
                .set("spark.executor.memory","1g")
    val sc = new SparkContext(conf)
    ```
    - The 2nd method is to `dynamically` set spark properties by either passing them as `command line arguments` or using `conf/spark-defaults.conf` file.
    ```
    ./bin/spark-submit --name word-count --master local[4] \
                        --conf spark.shuffle.spill = false \
                        --conf "spark.executor.extraJavaOptions=-XX:+PrintGCDetails \
                                -XX:+PrintGCTimeStamps" myApp.jar
    ```
    - The Spark properties can be viewed from `Web UI` at port 4040.
    - There is a degree of precedence to properties set using `SparkConf` Object, passing using `command line` and saved in `spark-defaults.conf` file which is:
    ```
    SparkConf > Command Line > spark-defaults.conf
    ```

## Monitoring
- There are 3 ways of monitoring Spark Applications.
    1. Web UI
        - Port 4040
        - This is available for the duration of the application, if we want the information to persisted to storage, we have to set the `spark.eventLog.enabled` to true before starting the application.
    2. Metrics
        - this is another way to monitor Spark Applications.
        - This is based on `Coda Hale Metric Library`.
        - We can `customize` it so that it can report to a variety of output sources or sinks such as `csv`. this can be done by configuring the `metrics.properties` file under the `conf` directory
    3. External Instrumentation
        - External tools such as `Ganglia` can be also used to monitor spark.
        - `Ganglia` is used to view `Overal cluster utilization` and `resource bottlenecks`.
        - Various `OS Profiling tools` such as `dstat,iostat,iotop` and `JVM Utilities` such as `jstack, jstat, jmap, jconsole` can also be used to monitor Spark.
- The Web UI is found on port 4040 and shows information about the application that is currently running. It shows the following information
    - A list of scheduler stages and tasks.
    - A summary of RDD sizes and memory usages.
    - Environmental information.
    - Information about the running executors.
- The history of an application can be gazed upon by launching the history server found at `./sbin/start-history-server.sh`. Following configurations for the history server can be set.
    - Memory allocated for it.
    - Various JVM options.
    - Public Address for the server.
    - Various Other Properties.
## Tuning
- Spark Programs can be bottlenecked by any resource in the cluster.
- Due to Spark Nature of `In-memory Computations`, `Data Serialization` and `memory tuning` are 2 areas that will improve performance.
-  Data serialization is crucial for `network performance` and to `reduce memory use`.
### Data Serialization
- There are 2 Serialization libraries available for spark applications:
    - **Java** Serialization.
        - This is the default option, but is much slow and leads to large serialized objects.
    - **Kyro** Serialization.
        - This is much better in performance, compared to java but **does not support** all serializable types. It requires us to register our types in advance for best performance.
        - To use Kyro Serialization we set it using the **SparkConf** Object.
        `conf.set('spark.serializer','org.apache.spark.serializer.KyroSerializer')`.
### Memory Tuning
- With Memory Tuning we have to consider 3 things:
    - Amount of Memory used by the objects.
        - We can determine how much memory our dataset requires by creating an RDD, putting it into cache and then look at the **`SparkContext log`** on our driver program. this will show us how much memory our dataset uses.
        - **Avoid Java Features** that add overhead such as **Pointer based data structures** and **wrapper objects**.
        - If possible go with arrays and primitive types.
        - Avoid Nested Structures.
        - Serialized Objects can also help reduce memory usage.
    - Cost of accessing those objects.
        - Serialized objects take long to access the objects because we have to deserialize it first.
    - Overhead of Garbage Collection.
        - We can collect statistics on garbage collection to see how frequently it occurs.
        - To do so we set the `SPARK_JAVA_OPTS` environment variable to `-verbose:gc -XX:+PrintGCDetails -XX:+PrintGCTimeStamps`
### Level of Parallelism
- In order to fully utilize the cluster, the level of parallelism should be considered.
- Parallelism is automatically set to file size of the task. But we can also set it through optional parameters of `SparkContext.textFile`.
- We can also set the default level of parallelism in the `spark.default.parallelism` config property.
- > It is recommended to set 2-3 tasks per CPU Core in the cluster.
### Memory usage of reduce task
- sometimes when the RDD does not fit in memory, we get an **OutOfMemoryError**, which can be resolved by increasing the level of parallelism.
- By increasing the level of parallelism, each set of task input will be smaller and will fit into memory.
### Broadcasting large variables.
-  A good example would be if we have some type of static lookup table. We should consider turning it into a broadcast variable so it does not need to be passed on to each of the worker nodes.
### 