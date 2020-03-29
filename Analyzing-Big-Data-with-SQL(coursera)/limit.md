# LIMIT CLAUSE

- The limit clause is used to restrain the number of rows returned from the select statement.
- The order of the rows is arbitrary, except when an order by clause is used.

```sql
SELECT * FROM flights LIMIT 5;
```

- Limit clause shall come after all the other clauses and its applied after them.
- SQL engines only allow the use of numeric expressions in the LIMIT clause, they don't allow column references and other complex expressions.

## TOP-N Query | BOTTOM-N Query

- using order by clause in combination with limit to return the top N rows of the result with NULLS FIRST or LAST.

## Pagination

- SQL engines allow us to return chunks or pages of rows by using the **`offset`** keyword eg.
  - LIMIT 100 OFFSET 0
  - LIMIT 100 OFFSET 100
  - LIMIT 100 OFFSET 200
