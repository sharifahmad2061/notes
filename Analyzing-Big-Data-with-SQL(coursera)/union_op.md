# Union Operator

- The union operator is used to combine the results of 2 or more select statements and stack them together.
- It stacks the result, vertically on top of one another.
- There are 2 variations of the union operator,
  - **`UNION ALL`**
  - **`UNION DISTINCT`**

## Union All

- ex

```sql
select id, name from fun.games;
select id, name from toy.toys;
select id, name from fun.games
UNION ALL
select id, name from toy.toys;
```

- The last query stacks the results together, but it may contain redundant data.
