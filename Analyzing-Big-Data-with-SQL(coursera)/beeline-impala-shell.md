# Beeline

-   Beeline is a **`command line interface CLI`** for Hive **(Distributed SQL Engine)**.
-   It is based on an open source utility called **`SQLLine`**.
-   It uses JDBC to connect to hive.
-   We can use it **`interactively`** or in **`Batch Mode`**.

# Impala Shell

-   It the CLI tool for impala.
-   Impala shell does not use **ODBC or JDBC** to connect to Impala, It uses a different interface called **`Apache Thrift`**.
-   It can also run both in **Interactive** and **Batch Mode**.

# Variable Substitution

-   Instead of hardcoding values into hive or impala shell scripts we can use variables in there too.
-   The syntax for variable substitution in **`hive`** is:

```
SET hivevar:game=monopoly;
SET hivevar:game= monopoly ;
```

-   We don't need quotes inside the SET statment, hive will automatically trim white space after equal sign and after the literal string.
-   To use the variable in a statement use the following syntax:

```
SELECT list_price FROM fun.games WHERE name='${hivevar:game}'
```

-   Instead of setting the variable inside the script, we can also pass it as command line argument.

```
beeline -u ... --hivevar game="monopoly" -f games.sql
```

-   To pass multiple variables from the command line, we use the **--hivevar** option multiple times

```
beeline -u ... --hivevar red="255" \
                --hivevar green="255" \
                --hivevar blue="255" -f color.sql
```
