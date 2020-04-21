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
- When we use the union operator, we should use **`explicit type conversion and column aliases`**.
- A good recommendation when working with union operator is to keep the **`column names and data types of columns`** the same, so as to make the queries portable across the sql engines.

## Limit Clause with union

- There is no way to limit the number of rows returned from the union operator, except with a **`sub query`**.
- The limit clause can be **`individually applied`** to the select statments on both sides of the union operator, but the limit at the end of the select on the right of union does not work with the whole resultset.

## Order By Clause with union

- The way **`ORDER BY`** clause works, when we use the union operator, differs according to the sql engine we are using.
- In **`MySQL and PostgreSQL`**, in order to sort the result of union operator by a column, we use the order by clause at the **`end of the whole query`**. eg:

```sql
select name, list_price as price from fun.games
union all
select name, price from toy.toys
order by price
```

- This order by clause does not get applied to the second select statement, rather it applies to the whole resultset of the union operator.
- This method works with **`🦈 and 🐘`** but not with **`impala and hive`**, at least up until the latest versions of hive.
- Depending on the client of impala used, it might give a warning, but the resultset will be arbitrary, not sorted.
- If we use the order by clause with **`both select statements`** like this,

```sql
select name, list_price as price from fun.games order by price
union all
select name, price from toy.toys order by price;
```

- Depending on the sql engine we are using, it might throw an error, or ignore the order by clauses altogether and return the rows in arbitrary order.
- It might **`sort the 2 intermedite resultsets`** but when union is applied, that order might be lost.
- **`order by clause in both select statements`** is not recommended for use in any sql engine.
- The only way to return the resultset of a union sorted by a column in Hive and Impala is to use a sub query.
