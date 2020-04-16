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
- The order of the rows in the result set of a union is arbitrary.

## Union Distinct

- The **`union distinct`** returns only the unique rows in the result set and drops the copies.
- Most sql engines support union distinct, except older versions of **`Hive`**.
- The **`union`** keyword alone without the **`distinct or all`** keyword, often does the operation of **`union distinct`**.
- some sql engines do not allow the use of **`distinct`** keyword, where **`union`** does the work of **`union distinct`**.
- The select statement on both sides of the union operator should have the **`same schema`**, in other words, they should have the **`same number of columns`** and the pair of columns in both select statments should have the **`same names and the same data types`** or at least the same high level categories of data types like both numeric or character strings.
- When you use the union operator, we should use **`explicit type conversion and column aliases`**.
- A good recommendation when working with union operator is to keep the **`column names and data types of columns`** the same, so as to make the queries portable across the sql engines.
