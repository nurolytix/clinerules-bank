# Apache Spark & PySpark Project Guidelines

## Documentation Requirements

- Document all new features and changes in the `/docs` directory.
- Use Jupyter notebooks or example scripts to explain complex transformations or pipelines.
- Include clear inline comments, especially for custom UDFs, joins, and performance-critical logic.
- Keep `README.md` and `CHANGELOG.md` up to date with setup, dependencies, and workflow changes.

---

## Code Style & Patterns

- Follow the [Spark Coding Style Guide](https://github.com/apache/spark/tree/master/style-guide) for Scala, Python, and Java.
- For PySpark, reference the [Palantir PySpark Style Guide](https://github.com/palantir/pyspark-style-guide).
- Prefer the DataFrame and Dataset APIs over RDDs for new code.
- Use explicit schema definitions for DataFrames whenever possible.
- Use snake_case for Python code, camelCase for Scala/Java.
- Avoid magic numbers and hardcoded values—use named constants or configuration files.
- Name variables, columns, and Spark jobs clearly and descriptively.
- Modularize code: break large jobs into reusable functions or modules.
- Place shared utilities in a common location (e.g., `/src/utils`).

---

## PySpark & Spark Best Practices

### Joins

- Prefer `broadcast()` joins for small DataFrames: `from pyspark.sql.functions import broadcast`.
- Filter and reduce datasets before joining.
- Use join keys with high cardinality to minimize skew.
- Avoid joining large DataFrames together if possible; pre-filter or sample.
- Specify join types explicitly (`inner`, `left`, etc.) and avoid cross joins unless necessary.

### Aggregation

- Use DataFrame APIs like `groupBy().agg()` for aggregations, leveraging Spark SQL optimizer.
- On RDDs, prefer `reduceByKey`/`aggregateByKey` over `groupByKey` for efficiency.
- Avoid aggregating on skewed data; use salting or custom partitioning if needed.

### Filtering

- Apply `.filter()`/`.where()` as early as possible to minimize data volume.
- Combine filter conditions when possible for efficiency.
- Prefer built-in SQL/DataFrame functions for filtering, not UDFs.

### Actions

- Avoid `collect()`, `toPandas()`, or similar actions on large datasets; use `show()`, `take()`, or `head()` for sampling.
- Use `count()` only when necessary and never for existence checks (prefer `limit(1).count() > 0`).
- Write outputs using columnar formats (`parquet`, `orc`) for large-scale jobs.

### Transformations

- Chain DataFrame transformations for clear, optimized pipelines.
- Use built-in functions instead of UDFs wherever possible for performance.
- Cache or persist only when reused multiple times, and always unpersist when finished.

### Sampling

- Use `.sample()` or `.limit()` for sampling large datasets.
- Set a random seed in `.sample()` for reproducibility.

---

## Shuffling, Partitioning & Data Skew

- Minimize shuffles by filtering early, using narrow transformations, and joining carefully.
- Repartition only when necessary (e.g., after filtering or before large joins/aggregations).
- Use `repartition()` to increase and `coalesce()` to decrease partitions.
- Monitor partition sizes—aim for 100–200MB per partition for large jobs.
- Address skew using salting, custom partitioning, or Spark’s built-in skew handling options.

---

## Anti-Patterns & Don’ts

- Do **not** use `collect()`, `toPandas()`, or similar methods on large datasets—risk of out-of-memory errors.
- Do **not** use `count()` on full DataFrames for presence checks; use `.limit(1).count() > 0` or `df.rdd.isEmpty()`.
- Do **not** use `groupByKey` for aggregations—prefer `reduceByKey` or `groupBy().agg()`.
- Do **not** cache or persist unless the dataset is reused.
- Avoid UDFs when built-in functions suffice—UDFs are less efficient and non-optimizable.
- Avoid cross joins unless absolutely necessary.
- Do **not** hardcode paths, credentials, or cluster configuration.

---

## Memory and Performance Optimizations

- Use columnar file formats (Parquet, ORC) for efficient storage and IO.
- Never load large datasets into the driver with `collect()`, `toPandas()`, etc.
- Broadcast small lookup tables in joins; avoid shuffling large datasets.
- Cache/persist reused DataFrames; always unpersist when done.
- Tune partition sizes (100–200MB each for large jobs); adjust with `repartition()` or `coalesce()`.
- Avoid wide transformations (like `groupByKey`, wide joins) on large data.
- Control shuffle partitions via `spark.sql.shuffle.partitions` (tune for your workload).
- Set appropriate parallelism—avoid too many or too few partitions.
- Filter and reduce datasets before joins/aggregations.
- Enable Adaptive Query Execution (AQE) with `spark.sql.adaptive.enabled` for dynamic query plan optimization.
- Use efficient serialization (`KryoSerializer`).
- Monitor Spark UI for skew, long tasks, memory usage, and disk spills.
- Tune executor memory, cores, and overhead for workload and cluster.

---

## Best Practices for Large-Scale Data

- Use storage-efficient formats and partitioned tables.
- Optimize partitioning to avoid small files and maximize parallelism.
- Avoid unnecessary shuffles and data movement across the cluster.
- Monitor for and resolve data skew (e.g., with salting or custom partitioning).
- Leverage built-in Spark features for distributed computation and fault tolerance.

---

## Leveraging New Spark Features

- **Adaptive Query Execution (AQE):** Enable with `spark.sql.adaptive.enabled=true` for dynamic query plan optimization.
- **Pandas API on Spark:** Use `pyspark.pandas` for scalable pandas-like operations.
- **Structured Streaming:** Employ for real-time, fault-tolerant stream processing.
- **Dynamic Partition Pruning:** Reduces unnecessary data reads in joins.
- **Python Vectorized UDFs:** Use `@pandas_udf` for high-performance custom functions.

---

## Testing Standards

- Write unit tests for all business logic (`pytest`, `unittest`, `ScalaTest`, `JUnit`).
- Use sample/synthetic data for testing; never use production data.
- Implement integration tests for end-to-end pipelines.
- Validate schema and data types explicitly in tests.

---

## CI/CD & Version Control

- Enforce linting and automated testing in CI (e.g., GitHub Actions, Jenkins).
- All code must be peer-reviewed.
- Use semantic versioning and tag releases.
- Keep dependencies up to date; use dependency management tools (e.g., `sbt`, `pip`, `maven`).

---

## Security & Compliance

- Never log or output sensitive data.
- Validate and sanitize all external data sources.
- Follow organizational data governance and compliance policies.

---

## Operational Best Practices

- Document cluster configurations and job submission scripts in `/ops` or `/infrastructure`.
- Use versioned configuration files for different environments (dev, staging, prod).
- Set up monitoring and alerting (e.g., with Prometheus, Grafana) for production jobs.

---

## Additional Resources

- [Official Spark Documentation](https://spark.apache.org/docs/latest/)
- [Palantir PySpark Style Guide](https://github.com/palantir/pyspark-style-guide)
- [PySpark Coding Standards & Best Practices](https://blogs.perficient.com/2022/09/30/pyspark-coding-standards-best-practices/)
- [Databricks Spark Best Practices](https://docs.databricks.com/en/spark/best-practices.html)
- [Code Optimization in PySpark (Databricks Community)](https://community.databricks.com/tmcxu86974/attachments/tmcxu86974/Databricks-Women-In-AIforum-board/20/1/1721110284226.pdf)