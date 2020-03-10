# Grouping and Aggregation

- When using other BI tools in chain with SQL engine, its better to do the grouping and aggregation on the SQL engine part, rather than pulling all the data to the BI tool and therefore saturating the network. This concept is known as **`PushDown`**
- **`PushDown`** also avoids using alot of memory to hold all the rows that are fetched.
- When grouping, it always better to group on **`categorical`** columns, as they have limited number of values and hence turn out with a small result.
- To check how many rows a **`groupby`** query will return before we run it we should use the **`count(distinct ...)`** query. e.g.

```
SELECT count(distinct year, month, day) FROM flights;
```

- If there are **`null values`** in the columns **`count(distinct ...)`** will ignore them and return the number of rows without nulls.
- We can groupby multiple columns in the following way.

```
SELECT year, month, day, count(*) AS num_of_flights
    FROM flights GROUP BY year, month, day;
```

- When grouping by numerical column, we can use **`where`** clause to filter the rows and therefore get a small number of rows in the outcome.
- When working with numerical column, we can also use **`binning`** through the use of **`CASE WHEN THEN`** statement which will return only a few number of rows in the result.

```
SELECT MIN(dep_time), MAX(dep_time), count(*)
    FROM flights
    GROUP BY CASE WHEN dep_time IS NULL THEN 'missing'
                  WHEN dep_time < 500 THEN 'night'
                  WHEN dep_time < 1200 THEN 'morning'
                  WHEN dep_time < 1700 THEN 'afternoon'
                  WHEN dep_time < 2200 THEN 'evening'
                  ELSE 'night'
                END;
```

- The result of the above query doesn't include the **`binned`** categories, to show it in the result, we'll have to put it in the **`SELECT LIST`** and then use its **`alias`** in the **`GROUP BY`** clause. **`Hive`** doesn't allow the use of alias in the **`GROUP BY`** clause. In the case of **`Hive`** we'll have to put the whole case statement in the GROUP BY clause.

## Positional Reference

- Instead of putting the whole case statement in the GROUP BY statement we can use **`positional reference`** in the GROUP BY clause. **Positional reference** is the index number of the **`SELECT LIST`**, starting at 1. For example **`GROUP BY 1;`**
