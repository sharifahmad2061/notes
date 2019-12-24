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