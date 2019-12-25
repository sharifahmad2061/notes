# Spark Libraries
- Spark comes with libraries that we can utilize for specific use cases. These include SparkSQL, Spark Streaming, MLlib, and GraphX.
- These libraries are an extension of the Spark Core API and there any improvements made to the Spark Core API will automatically will automatically be reflected in them.
- These libraries have a little overhead.
## Spark SQL
- allows relational queries expressed in either
    - SQL
    - HiveQL
    - Scala
- Spark SQL RDD is called `SchemaRDD` which consists of:
    - Row Objects
    - Schema (describes data in each column in a row)
    - Created from:
        - Existing RDD
        - Parquet File
        - JSON dataset
        - HiveQL (to query data stored in Hive).
    - supports scala, java, python.
- The SQL Context is created from the Spark Context.
```
import pyspark
sc = pyspark.SparkContext.getOrCreate()
sqlContext = pyspark.sql.SQLContext(sc)
```
- There are 2 ways to create the `SchemaRDD`.
    - Inferring the schema using reflection.
        - This method has more concise code and works well when we already know the schema.
    - Programmatic interface
        - This method uses a programmatic interface to construct a schema and then apply that to existing RDD.
        - This method gives us more control when we don't know the schema of RDD until runtime.
- Inferring the schema using reflection
    ```
    case class Person(name: String, age: Int)
    val people = sc.textFile('./examples.txt')
        .map(_.split(','))
        .map(p => Person(p(0),p(1).trim.toInt))
    people.registerTempTable(people)
    val teenagers = sqlContext.sql('SELECT name FROM people WHERE age > 13 AND age < 19')
    teenagers.map(t => 'Name: ' + t(0)).collect().foreach(println)
    ```

    - The `case` class in scala is used to define the schema of the table.
    - The arguments of the case class are read using reflection and becomes the columns of the table.
    - Next we define the SchemaRDD named as people, by loading a textfile and then splitting each line by coma and then convert each name and age to a `Person object`.
    - Then we register the people's schemaRDD as a table which can queried using the `SQLContext.sql()` method.
    - The result coming out of the sql method is also schemaRDD and we can run normal RDD operations on it.
- Programmatic Interface
    - The programmatic interface is used when cannot define the case classes ahead of time. For example, when the structure of records is encoded in a string or a text dataset will be parsed and fields will be projected different for different users.
    ```
    val people = sc.textFile()

    val schemaString = 'name age'
    val schema = StructType(schemaString.split(' ').map(fieldName => StructField(fieldName, StringType, true)))

    val rowRDD = people.map(_.split(',')).map(p => Row(p(0),p(1).trim)))
    val peopleSchemaRDD = SQLContext.applySchema(rowRDD,schema)

    peopleSchemaRDD.registerTempTable(people)

    val results = sqlContext.sql('SELECT name FROM people')
    results.map(t => 'Name: ' + t(0)).collect().foreach(println)
    ```
## Spark Streaming
- spark streaming gives us the ability to process live streams of data in small batches.
- spark streaming is scalable, high-throughput and fault-tolerant.
- spark streaming RDD is called `DStreams`, we write stream programs with DStreams which is a sequence of RDDs from a stream of data.
- Spark Streaming can receive data from these sources.
    - Kafka
    - flume
    - HDFS / S3
    - Kinesis
    - Twitter
- Spark Streaming can push data to:
    - HDFS
    - Databases
    - Dashboard
- At the time of `Spark 1.2`, python with spark streaming only supports `text data sources`. Support for flume and Kafka will be available in future releases.
- Overview of Spark Streaming
    - First the input stream comes into spark streaming.
    - Then the data stream is broken into batches of data that is fed into spark engine for processing.
    - Once the data is processed, it is sent out in batches.
    - Spark streaming support sliding window operations. In a windowed computation, every time the window slides over a source of DStream, the source RDDs that fall within the window are combined and operated upon to produce the resulting RDD.
    - There are 2 parameters for a sliding window.
        - The `window length` is the duration of the window.
        - The `sliding interval` is the interval in which the window operation is performed.
        - Both `window length` & `sliding interval` must be in multiple of the `batch interval` of the `source Dstream`.
    - Example
        - We want to count the number of words coming in from the TCP socket.
        ```
        # import libraries and modules
        import pyspark
        
        # create the spark context and streaming context
        sc = pyspark.SparkContext.getOrCreate()
        conf = pyspark.SparkConf().setMaster('local[2]').setAppName('NetworkWordCount')
        ssc = pyspark.StreamingContext(conf,Seconds(1))
        
        # create a DStream
        lines = ssc.socketTextStream('localhost',9999)

        # split lines into words
        words = lines.flatMap(lambda x : x.split(' '))

        # count the words
        pairs = words.map(lambda x : (x,1))
        wordCounts = pairs.reduceByKey(lambda acc,x : acc + x)

        # print to console
        print(wordCounts)

        # Explicitly start the processing
        ssc.start()
        ssc.awaitTermination()
        ```
    - > One Import thing to note that when each element of the application is executed, the real processing doesn't actually happens yet. We have to explicitly tell it to start using `ssc.start()` and once the application starts it will continue on running untill the computation terminates.