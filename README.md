# 🔍 clinerules-bank

**clinerules-bank** is a curated collection of best practice rules for Apache Spark, Trino, Flink, Iceberg, Delta Lake, and more.  
Built for use with [Cline](https://github.com/ClineHQ/cline), it helps developers catch anti-patterns, misconfigurations, and performance issues early in the development cycle.

Use this rule bank to improve query quality, accelerate development timelines, and enforce consistency across teams — whether you're building batch pipelines, streaming apps, or lakehouse systems.

---

## 🧠 About the Project

**clinerules-bank** is an open-source repository of curated best practice rules designed for modern data systems. It provides structured `.clinerule` files for popular frameworks, query engines, and table formats such as Apache Spark, Trino, Flink, Iceberg, Delta Lake, and more.

Each rule encapsulates expert guidance for detecting performance bottlenecks, validating configurations, enforcing coding standards, and catching common anti-patterns early in development. These rules are written to be directly usable with [Cline](https://github.com/cline/cline), a command-line assistant for intelligent code and config analysis.

By adopting `clinerules-bank`, engineering teams can significantly reduce debugging time, improve data pipeline reliability, and accelerate onboarding of new developers. The rule engine acts like an automated reviewer, proactively flagging issues before they reach production.

This repository helps teams shift left on data quality and operational excellence, reducing rework, review overhead, and technical debt. Whether you're optimizing Spark SQL joins, validating Iceberg partitioning, or enforcing security best practices in Trino, `clinerules-bank` brings consistency, speed, and confidence to your development workflow.

---

## 📦 Repository Structure

```text
rules/
├── spark/
│   └── performance/
├── trino/
├── flink/
├── iceberg/
├── delta/
└── common/
```
---

## ✅ Contributing

We welcome contributions! If you have domain knowledge or production experience with any supported system, feel free to submit a new rule or improve an existing one.  

---

## 📄 License

Licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
