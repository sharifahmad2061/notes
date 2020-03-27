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
