# Grouping and Aggregation

- When using other BI tools in chain with SQL engine, its better to do the grouping and aggregation on the SQL engine part, rather than pulling all the data to the BI tool and therefore saturating the network. This concept is known as **`PushDown`**
- **`PushDown`** also avoids using alot of memory to hold all the rows that are fetched.
- When grouping, it always better to group on **`categorical`** columns, as they have limited number of values and hence turn out with a small result.
- To check how many rows a **`groupby`** query will return before we run it we should use the **`count(distinct ...)`** query. e.g.

```
SELECT count(distinct year, month, day) FROM flights;
```

- We can groupby multiple columns in the following way.

```
SELECT year, month, day, count(*) AS num_of_flights
    FROM flights GROUP BY year, month, day;
```
