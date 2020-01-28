# DataFrame API
- df.show(n) -> will print the dataframe in a nice format with the number of rows.
- df.take() -> will return a **`list`** of the row object.
- df.collect() -> will get all of the data from the entire dataframe and it can sometime
                    crash the **`Driver node`**.
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