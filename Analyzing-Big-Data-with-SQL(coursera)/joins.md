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
- Instead of printing all the columns, we may want to print out a select list of columns, in which case if a column name is available in both the tables, then that **column name much be prefaced with the table name**, for example **`toy.toys`**. eg:

```sql
select toys.id AS id, toys.name AS toy, price,
        maker_id, makers.name AS maker, city
    FROM toys JOIN makers
        ON toys.maker_id = makers.id
```

- The table name can be further qualified by prefacing it with a database name.
- Incase of column name ambiguity, we can also give table names **`aliases`** in the **FROM** clause and then the column names can be prefaced by those aliases in both the **`select list`** as well as **`join condition`**. eg:

```sql
select t.id AS id, t.name AS toy, price,
        maker_id, m.name AS maker, city
    FROM toys AS t JOIN makers AS m
        ON t.maker_id = m.id
```

- The **`AS`** keyword in **`FROM clause`** is optional and is common to omit it.

## Inner Join

- **Inner Join** is the default join, sql engines use, when we specify join in a query.
- It can also be explicitly specified by using the keyword **`INNER`**.
- It returns the rows which matches the **join condition**.
- In mathematical terms, it returns the **intersection** of 2 sets.
- > The result of inner join can be problematic sometimes, because they leave out data, for example in case of makers and the number of toys they have made, if we use inner join, mattel is left out. In our result set, we might want, mattel with 0 against number of toys.

## Outer Join

- There are 3 types of **outer joins**:
  - Left Outer Join
    - In a left outer join, if there are rows in the left table, with **join column** values, that does not exist in the right table, it returns them anyway, with the matching (intersection) rows.
  - Right Outer Join
    - In a right outer join, if there are rows in the right table, with **join column** values, that does not exist in the left table, it returns them anyway, with the matching (intersection) rows.
  - Full Outer Join
    - It returns the **`union`** of both the sets.
- **Outer Join** have to be explicitly specified with the keywords **LEFT/RIGHT/FULL OUTER JOIN**.
- MySQL doesn't support **FULL OUTER JOIN**, Hive, Impala and PostgreSQL support all 3 types of outer joins.
- Most sql engines allow us to **left out the outer** keyword in the **LEFT/RIGHT/FULL OUTER JOIN**, but it a good practice to be explicit and specify the **OUTER** keyword.

## Join Styles

- The joins presented in this document above are known as **`SQL 92 style or explicit joins`**.
- Many SQL engines, also support the **`SQL 89 style or implicit join`** syntax, where we specify the comma separated list of tables in the **`from`** clause and the **`join condition`** in the **`where`** clause. eg:

```sql
SELECT *
  FROM toys, makers
    WHERE toys.maker_id = makers.id;
```

- **`SQL 89 style`** joins can only preform **`inner join`**, for other type of joins like **`left, right and full outer join`**, we have to use **`sql 92 style syntax`**.

## Using Keyword

- In some join queries, the names of the corresponding columns in the join condition are identical, for example the **`office_id`** column in both the **`office & employees`** table, in such cases, instead of using the **`ON`** keyword followed by equality expression, we can use the **Using** keyword, followed by the **`common join key column name`** in **`paranthesis`**.

## Natural Joins

- When the corresponding columns, in the join condition have identical names, **`some sql engines allow us to omit the join condition`** and will automatically join the tables on all pair of columns that have identical names in the left and right tables, to make the sql engine do this, we have to specify the **`NATURAL`** keyword before **`JOIN`**, for example:

```sql
SELECT *
  FROM employees e NATURAL JOIN office o;
```

- MySQL and PostgreSQL support natural joins, whereas Hive and Impala don't.
- SQL engines that support natural joins, also allow **`NATURAL LEFT/RIGHT/FULL OUTER JOIN`**.

## Cross Join

- When we don't specify a **`join condition`** or **`natural`** keyword before **`join`** keyword, sql engines perform **`cross join`**.
- PostgreSQL doesn't support cross join and throws an error, whereas Hive, Impala and MySQL do.
- In Cross Join, the sql engine combines every row in the left table with every row in the right table.
- The number of rows in the result set is the product (multiplication) of the number of rows in the left table and the number of rows in the right table.
- In most sql dialects we can explicitly specify **`cross join`** in our sql statement.

## Multi-Table Join

- To join 3 or more tables, we use the same join syntax as 2 tables, but with more joins added at the end of the from clause, Each join should have its own **`ON keyword and join condition`**.
- To join the customers, orders and employees tables, we use the following query:

```sql
SELECT c.name AS customer_name,
 ​   ​   o.total AS order_total,
 ​  ​    e.first_name AS employee_name
​   FROM customers c
​      JOIN orders o ON c.cust_id = o.cust_id
​   ​   JOIN employees e ON o.empl_id = e.empl_id;
```

- In multi-table join, we can specify **`any type of join, in any combination`**.
- > Each join condition can refer to any join key columns in any of the tables mentioned earlier in the **FROM** clause.
