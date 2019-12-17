# Spark Application Programming
## Purpose & Usage of the Spark Context
- The spark context is the main entry point to everything in Spark.
- It can be used to create RDDs and Shared Variables on the cluster.
- When we lauch the Spark shell, the spark context is automatically initialized for us, with the variable `sc`.
- For Spark application we must import classes and then initialize the Spark Context Object.
```
import pyspark
sc = pyspark.SparkContext.getOrCreate()
readmeRDD = sc.textFile('readme.md')
```
## Linking with Spark - Python
- Spark v(2.4.3) works with python 3.
- It uses the standard CPython Interpreter so C libraries like `NumPy` can be used.
- To run Spark Applications in python, we use the `bin/spark-submit` script located in Spark home directory.
    - This Script loads the Spark's Java/Scala libraries.
    - And allows us to submit applications to the cluster.
- If we want to access HDFS, we have to use a build of pyspark which can link to our version of HDFS.
- These classes need to be imported.
```
from pyspark import SparkContext, SparkConf
```
## Initializing Spark - Python
- Build a Spark Conf object that contains information about our application.
```
import pyspark
conf = pyspark.SparkConf().setAppName(appName).setMaster(master)
```
- The `appName` is the application name to show on the cluster UI.
- The ``master`` parameter is a spark, mesos or YARN cluster URL, or `'local'` keywork to use in local mode.
- In Production mode we don't hardcode the `master` into the program, rather we lauch using `spark-submit` and provide it as an argument.
- Then we create the Spark Context object.
```
sc = pyspark.SparkContext(conf=conf)
```
## Pass functions to Spark
- Spark API relies heavily on ``passing functions`` in the ``driver program`` to run on the cluster. When a job is executed, the ``spark driver`` needs to tell its workers, how to process the data.
- There are 3 ways of passing functions.
    - The 1st method is an `anonymous function syntax`.
    ```
    lambda x : x + 1
    or
    (x: Int) => x+1
    ```
    - The 2nd method is to use static method in a global singleton object.
        ```
        Object MyFunctions{
            def func1 (s: String) : String = {...}
        }
        MyRDD.map(MyFunctions.func1)
        ```
        - We create a global object, in this code block its the object `MyFunctions` and define the function inside it. When the driver requires the function it only sends out the object and workers can access it.
    - The 3rd method is `Pass by Reference`
        - It is possible to pass reference to a method inside a class instance instead of a singleton object.
        - To avoid sending the entire object, consider copying the data used by the function to a local variable inside the function.
        ```
        val field = 'hello'
        Avoid:
        def doStuff(rdd: RDD[String]):RDD[String] = {
            rdd.map(x => field + x)
        }
        Consider:
        def doStuff(rdd: RDD[String]):RDD[String] = {
            val _field = this.field
            rdd.map(x => _field + x)
        }
        ```
- To run a spark python application do the following.
```
.bin/spark-submit examples/src/main/python/pi.py
```
## Create & Run Spark Standalone application
- If our application depends on 3rd party libraries, we pass it using `--py-files` argument.
## Submit application to Spark Cluster
- Submitting application to the cluster
```
.bin/spark-submit \
--class <main-class> \
--master <master-url> or local[2] #(number of cores) \
--deploy-mode # driver on worker nodes (cluster) or locally as an external client (client) \
--conf <key> = <value> \
... # other options
<application jar> \
[application arguments]
```

```
spark-submit --help
```
- Example
```
.bin/spark-submit \
--class org.apache.spark.examples.SparkPi \
--master local[8] \
/path/to/examples.jar \
100
```