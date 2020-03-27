# Order By Clause

- When running a simple **`select`** statment on a distributed SQL engine, the rows are returned in a random order.
- To sort the rows into a specific order, we use the **`ORDER BY`** clause.
- The order by clause can take multiple column references.
- The purpose of multiple column references is to break the tie between rows of first column reference.
- The default behaviour of ORDER BY clause is **`ASC` (ascending)**.

```sql
SELECT * FROM fun.games
ORDER BY id;
SELECT * FROM fun.games
ORDER BY max_players, list_price;
```

- To explicity order the data in descending order we use the keyword **`DESC`**.

```sql
SELECT * FROM fun.games
ORDER BY id DESC;
```

- When sorting by multiple columns its helpful to specify the **`sort order`** for each column. The default sort order for each column is **Ascending**, which can be omitted. But **Descending** should be explicity specified.

```sql
SELECT * FROM fun.games
ORDER BY max_players DESC, list_price ASC;
```

- 1 important thing pertaining to **HUE** is that when we sort the data using table headers in HUE, it only sorts the present data i.e. 100 or more records that are shown.
- Many BI applications also have this shortcome, that should be looked out for.

## Order By Clause Expressions

- Order by clause can also take expressions, evaluate then and then order by them, for example:

```sql
SELECT * FROM wax.crayons
ORDER BY (greatest(red, green, blue) - least(red, green, blue))
         / greatest(red, green, blue)
DESC;
```

- Most SQL engines allow us to use the expression in **SELECT LIST**, give it an alias and then use that alias in **ORDER BY** clause. eg.

```sql
sql
SELECT *, (greatest(red, green, blue) - least(red, green, blue))
         / greatest(red, green, blue) AS saturation
    FROM wax.crayons
    ORDER BY saturation DESC;
```

- SQL engines handle **NULL values** in ORDER BY column differently, eg. **Impala and Postgresql** consider NULL values the **greatest** and puts it at the bottom when sorting in ascending order, and vice versa. Similarly **Hive and MySQL** consider NULL values the **smallest** and puts them at the top when sorting in ASCENDING order.
- With some SQL engines it is possible to tell the sql engine, where to put NULL values in the ORDER BY column, by using the **`NULLS FIRST or NULLS LAST`** keyword.
- The **`NULLS FIRST and NULLS LAST`** keywords can be used with each column reference in the ORDER BY LIST.
- MYSQL as well as older versions of HIVE doesn't support the **`NULLS FIRST and NULLS LAST`** keywords.
- For sql engines that don't support the **`NULLS FIRST or NULLS LAST`** keywords, there is a trick to achieve the same functionality. which is:

```sql
SELECT shop, game, price
FROM fun.inventory
ORDER BY price IS NULL ASC, price;
```

- When ordering by **BOOLEAN VALUES**, **false** comes before **true**, and then within false values, tie is broken by the price values.

## Limitation of HIVE

- When using Order by clause in hive, the **`order by column`** or **`columns used in order by expression`** must be in the **`select list`**, otherwise hive gives an error.
