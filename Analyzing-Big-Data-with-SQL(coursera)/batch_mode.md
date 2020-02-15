# Batch Mode in Hive and Impala
- **`Batch mode`** is also known as **Non-Interactive mode**.
- In this mode we save all our commands into a single file and then execute it by giving the file to hive or impala.
- To execute a **`HiveQL script`**, the following command can be used.
```
> beeline -u jdbc:hive2://localhost:10000 -e 'SELECT * FROM fun.games'
> beeline -u jdbc:hive2://localhost:10000 -e 'USE fun;SELECT * FROM games;'
> beeline -u jdbc:hive2://localhost:10000 -f cmds.sql
```
- To execute the same thing in Impala the following command is used:
```
> impala-shell -q 'SELECT * FROM fun.games'
> impala-shell -q 'use fun;SELECT * FROM games;'
> impala-shell -f cmds.sql
> impala-shell --quiet -f cmds.sql
```
- command line options for impala shell are case-sensitive.
# Comments
- Hive and Impala both support single line comments
- Comments start with **`--`** and reach up to the end of the line.
- Hive doesn't support multi-line comment, whereas Impala does.
- To put a multi-line, start with **`/*`** and end with **`*/`**.
# Output formats
## Hive
- To print the output of hive in different format, use the **`--outputformat`**.
- The available formats are:
    - **`csv2`**
    - **`tsv2`**
- To suppress printing the header, use **`--showHeader=false`**.
## Impala
- Impala by default doesn't print the header, to print it, use the **`--print_header`**.
- To output result in other format use **`delimited`**, Impala by default uses **tab separated values**.
- To output comma separated values use **`--output_delimiter=','`**