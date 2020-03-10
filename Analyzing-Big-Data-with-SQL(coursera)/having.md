# Having Clause

- The purpose of **having clause** is to **`filter groups`** in the data using criteria that is based on **aggregates of the groups**.
- It is intended to be used with the **GROUP BY** clause.
- It comes after the **GROUP BY** clause in the **SELECT STATEMENT** and is processed after the **GROUP BY** clause, therefore the rows it filters represents groups.

```sql
SELECT shop, sum(price*quantity)
    FROM fun.inventory
    GROUP BY shop
    HAVING sum(price*quantity) > 300;
```

- Groups for which the having clause evaluates to **True** are returned in the **resultset**. Groups for which the having clause evaluates to **False** or **NULL** are **ignored**.
