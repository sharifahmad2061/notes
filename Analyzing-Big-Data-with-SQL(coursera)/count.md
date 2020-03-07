# COUNT FUNCTION

- The **`count`** function sums the number of rows in the table when given the **`*`** as argument.
- It can also return the number of rows in a specific category when the table is grouped by a column. e.g.

```
SELECT shop, count(*)
    FROM fun.inventory
    GROUP BY shop;
```

- The outcome of the above query is the following:

| shop      | count(\*) |
| --------- | --------- |
| Dicey     | 2         |
| Board 'em | 3         |

- **`count`** function behaves differently when it is given a **`column reference`**, It then aggregates the number of **`NON NULL`** values in the column. e.g.

```
SELECT shop, count(price)
    FROM fun.inventory
    GROUP BY shop;
```

- The result of the above query is the following:

| shop      | count(price) |
| --------- | ------------ |
| Dicey     | 2            |
| Board 'em | 2            |

- The reason for 2 in the **`Board 'em`** is that there is a null value in 1 row against price.
- other **`aggregate function like sum and avg`** behave the same way, they ignore null values.
- The count function has another feature, it can be used to **`count distinct`** values in a column. e.g.

```
SELECT count(distinct aisle) FROM fun.inventory;
```

- In some SQL engines, multiple columns can be used after the **`distinct keyword`**, this returns the **`unique combinations`** of those specified columns. (postgresql can't work with this) e.g.

```
SELECT count(distinct red, green, blue) FROM wax.crayons;
```

- **`COUNT(DISTINCT ...)`** can also be used multiple times in a SQL statement. for example: (Impala can't work with this)

```
SELECT count(distinct red),
       count(distinct blue),
       count(distinct green)
    FROM wax.crayons;
```

- The above query returns a single row with 3 columns, with distinct values from each column.
- In SQL the opposite of **`DISTINCT`** is **`ALL`**.
