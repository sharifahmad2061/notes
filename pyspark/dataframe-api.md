# DataFrame API
- df.show(n) -> will print the dataframe in a nice format with the number of rows.
- df.take() -> will return a **`list`** of the row object.
- df.collect() -> will get all of the data from the entire dataframe and it can sometime
                    crash the **`Driver node`**.
- df.select('column1').collect() -> will give all rows of the selected column.
- df.limit() -> returns a new dataframe by taking the **`first n rows`**
- df.head() -> returns **`an array`** of first n rows from the dataframe.

# Schema
- Schema defines the column names and their data types, such as Integer, Double, String, Date and so on.
```
df.dtypes
df.printSchema()
```
- Spark can infer the schema by default.
- In production environment we should explicity define our schema.
- pyspark types are defined in:
```
from pyspark.sql.types IntegerType, StringType,
                        StructField, StructType
schema - StructType([
    #            col name   type         can_contain_null_value
    StructField("column_1", IntegerType, True),
    StructField("column_2", StringType,  True),
    StructField("column_3", IntegerType, True),
])
```
- Access a column in pyspark
```
df.column1 -> not always works, column1 can be a reserved word.
df['column1']
df.select(col('column1'))
```
- Access multiple columns
```
df.select('col1','col2').show(3)
```
- Add a new column to dataframe
```
df.withColumn('new_col',2*df['existing_col'])
```
- Rename a column
    - renaming a column returns a new **`dataframe`**.
    - If a column does not exist then no operation will be performed.
```
df.withColumnRenamed('ExistingColumnName','NewColumnName')
```
- Dropping a column
    - This operation returns a new dataframe
    - If a column does not exist then no operation will be performed.
```
newdf = df.drop('col1')
newdf = df.drop('col1','col2')
```
- Groupby Operation
```
df.groupBy('col1')
```
- Filtering Rows in PySpark
```
df.filter(col('IUCR') >= 4.5)
```
- Getting Unique Rows
    - distinct can crash the program, because it has to go through the whole dataset.
```
df.distinct().show(4) -> crashed the driver
df.select(column or columns).distinct().show(4) -> better, has to only find unique
    	                                            values in specified columns
```
- Sort Rows in PySpark
```
df.orderBy(col('IUCR'))
```
- Append Rows in PySpark
    - Since PySpark Dataframe is immutable, we cannot just add rows.
    - Instead we have to concat 2 dataframes.
    - Both dataframes need to have the same number of columns and same schema, otherwise the union fails.
```
df.union(df2)
```