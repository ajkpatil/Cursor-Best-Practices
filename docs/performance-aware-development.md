---
title: "Performance-Aware Development"
description: Query optimization, joins, index-aware SQL, and designing for 10× load when working with AI-generated code.
---

# Performance-Aware Development

Performance is not an afterthought—it is a design constraint. AI can generate correct implementations, but without explicit performance awareness, those implementations often degrade under real-world load.

Performance-aware development requires anticipating scale, understanding data access patterns, and making decisions that remain efficient as usage grows.

---

## Query Optimization Practices

Database queries are a primary source of performance bottlenecks. AI-generated queries often prioritize correctness over efficiency, which can lead to unnecessary scans, redundant computations, or poor execution plans.

Effective query optimization focuses on:

- Reducing the amount of data processed
- Leveraging indexes efficiently
- Avoiding repeated calculations
- Filtering early in the query

**Example**

Inefficient:

```sql
SELECT * FROM ledger WHERE purpose = 'CasinoBet';
```

Issues:

- Fetches all columns unnecessarily
- No filtering by user or date
- Potential full table scan

Optimized:

```sql
SELECT SUM(amount)
FROM ledger
WHERE purpose = 'CasinoBet'
AND user_id = :userId
AND created_at BETWEEN :start AND :end;
```

Improvements:

- Aggregation reduces data transfer
- Filters reduce scanned rows
- Aligns with indexed columns

Optimization is not about rewriting queries after issues appear—it is about designing queries with efficiency in mind from the start.

---

## Avoiding Unnecessary Joins

Joins are expensive operations, especially on large datasets. AI frequently introduces joins to “complete” a query, even when they are not required.

Each additional join:

- Increases execution time
- Expands intermediate result sets
- Complicates query plans

Avoid joins unless they are essential for retrieving required data.

**Example**

Unnecessary join:

```sql
SELECT u.id, SUM(l.amount)
FROM users u
JOIN ledger l ON u.id = l.user_id
WHERE l.purpose = 'CasinoBet'
GROUP BY u.id;
```

If `user_id` is already present in `ledger`, the join is redundant.

Optimized:

```sql
SELECT user_id, SUM(amount)
FROM ledger
WHERE purpose = 'CasinoBet'
GROUP BY user_id;
```

This reduces overhead and improves execution speed.

---

## Index Awareness

Indexes are the foundation of query performance. Without them, even well-structured queries can degrade into full table scans.

AI does not automatically know your indexing strategy, so it must be guided to align queries with existing indexes.

Key principles:

- Filter using indexed columns
- Avoid operations that prevent index usage (e.g., functions on indexed fields)
- Combine indexes strategically for common query patterns

**Example**

Index exists on `(user_id, created_at)`

Inefficient:

```sql
SELECT *
FROM ledger
WHERE DATE(created_at) = '2026-04-01';
```

This prevents index usage.

Optimized:

```sql
SELECT *
FROM ledger
WHERE created_at >= '2026-04-01'
AND created_at < '2026-04-02';
```

This allows the database to use the index efficiently.

Index awareness ensures that queries scale with data growth rather than degrading unpredictably.

---

## Scalability Thinking (10× Load Mindset)

A system that works at current scale may fail under increased load. Performance-aware development assumes that usage will grow and designs accordingly.

The “10× mindset” asks:

- What happens if data volume increases tenfold?
- What happens if request rate spikes?
- Which parts of the system become bottlenecks?

This mindset shifts focus from immediate correctness to long-term viability.

**Example**

Current implementation:

```
Fetch all bets and process in application memory
```

Works for small datasets, but fails at scale.

Scalable approach:

- Aggregate in database
- Use pagination or batching
- Precompute results where necessary

Prompting AI with scalability constraints:

```
This query runs on 50M+ rows and 10k requests/min.
Optimize for scalability and avoid full table scans.
```

This forces the model to consider performance as a primary requirement.

---

Performance-aware development ensures that systems remain efficient not only when they are built, but as they grow. By focusing on query optimization, minimizing unnecessary joins, aligning with indexing strategies, and designing with a 10× load mindset, developers can prevent performance issues before they emerge rather than reacting to them later.

[← Back to table of contents](index.md)
