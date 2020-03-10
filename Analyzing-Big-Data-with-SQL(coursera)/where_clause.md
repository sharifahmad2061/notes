# `Where` Clause

- The where clause is optional in a SQL select statement.
- It is used to apply conditions on columns and can do so on multiple columns such as:

```
SELECT * FROM games WHERE list_price < 10;
```

- This is mainly used to filter rows based on conditions applied to certain columns.
- The `WHERE Clause` takes all the rows in a table, checks each row against some conditions and returns only the rows in which the conditions are true.
- We cannot use multiple expressions separated by commas in **Where clause**.
- The logical expression specified in **Where Clause** must return a **`Boolean Value`**.
- The **`logical expression`** used in where clause can be comprised of multiple boolean evaluating expressions such as:

```
SELECT * FROM games WHERE list_price < 10 AND quantity > 100;
```

- The SQL engines evalute the where clause before they compute the expressions in the SELECT LIST, therefore the below sql statement `wont work`:

```
SELECT red + green + blue AS rgb_sum FROM wax.crayons WHERE rgb_sum > 650;
```

- We can filter using **WHERE CLAUSE** based on **`Aggregate expressions`**.
