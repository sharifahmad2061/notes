# Casting Columns
- Sometimes the columns we want to work with have `string` data type and the operation can't work with it, so we have to **`cast or convert`** it to other data types such as `int`.
- Some SQL engines do this process automatically which is known as **`Implicit Casting`**, whereas others such as Impala require us to explicitly convert columns to required data types.
- This is done using the **`cast`** function.
```
SELECT name, cast(year AS INT) + 100 FROM games;
```
# distinct keyword
- **`distinct`** keyword is for filtering out the rows returned from the select statement, It removes the duplicate rows from the result set.
- distinct keyword works with a single column or multiple columns to return unique combinations of those columns.
- DISTINCT keyword also works with **`expressions`**
```
SELECT DISTINCT min_age, min_players FROM fun.games;
SELECT DISTINCT * FROM fun.games;
```