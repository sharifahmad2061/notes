# PySpark Functions
```
from pyspark.sql.functions import mean
df.select(mean(df1.column1)).show()
```
- To Show all the functions
```
from pyspark.sql import functions
print(dir(functions))
```
- syntax and documentation of function
```
from pyspark.sql.functions import substring
help(substring)
```