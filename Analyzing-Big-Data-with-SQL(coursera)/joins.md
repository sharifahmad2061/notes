# Joins

- A join combines data from 2 related tables into 1 resultset.
- A join takes columns from 1 table and columns from the other table and merges them together, It combines them horizontally.
- A join not randomly slaps together the columns from the 2 tables, it also matches the rows from the 2 tables.
- To get data from 2 separate tables, we **`specify both table references`** after the **`FROM`** clause, with a **`JOIN`** keyword in between, For example:

```sql
select *
    from toys join makers;
```

- The 2 tables can be in the same database on in 2 different databases, in which case we need to use **`db-name.table-name`**.
- We also need to specify the relationship between the 2 tables, so that the sql engine can match the rows, in order to do this, we use the **`on`** keyword and the expression after it, for example:

```sql
select *
    from toys join makers
        on toys.maker_id = makers.id;
```

- The expression after the **`on`** clause is known as **`join condition`** and it contains a reference to a column in the 1st table, an equal sign and a reference to the corresponding column in the 2nd table, The column names are prefaced by columns they come from, for example **`toys.maker_id`**.
-
