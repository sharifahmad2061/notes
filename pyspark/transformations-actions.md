# Transformations
- They are a core data structure in Spark and are **`immutable`**, this can be a filter or distinct operation.
- When we perform a transformation, Spark adds that operation to a **`Direct Acyclic Graph or DAG`**.
- Spark applies the transformations in DAG when we perform an action. This is known as **`Lazy Evaluation`**.