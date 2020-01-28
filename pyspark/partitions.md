# Spark Partitions
- If we want Spark workers to work in parallel, we need to allow spark to divide or partition the data into chunks.
- Spark can't parallelize jobs if there is only 1 partition.
- Similarly Spark can't parallelize if there is only 1 worker node available.