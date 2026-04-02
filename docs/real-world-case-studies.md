---
title: "Real-World Case Studies"
description: Bug resolution, query optimization, prompt-driven cost savings, and failures from uncontrolled AI—patterns that scale.
---

# Real-World Case Studies

Case studies provide concrete evidence of how AI impacts development when applied correctly—and how it fails when used without discipline. The following scenarios illustrate both effective usage patterns and common failure modes.

---

## Faster Bug Resolution

**Context**  
A production issue caused incorrect GGR calculation when rollback entries were present. The existing query handled bets and wins but ignored rollback adjustments.

**Traditional Approach**

- Manually inspect query
- Trace ledger entries
- Identify missing conditions
- Rewrite and test

This process typically required significant time, especially under pressure.

**AI-Assisted Workflow**

1. Reproduce the issue:

```
This query produces incorrect GGR when rollback entries exist.
List possible failure scenarios.
```

2. Analyze root cause:

```
Identify why rollback entries are not accounted for correctly.
```

3. Apply minimal fix:

```
Fix the query to include rollback handling.
Do not change structure unnecessarily.
```

4. Validate:

```
Provide test cases including rollback and edge scenarios.
```

**Outcome**

- Root cause identified quickly (missing rollback subtraction)
- Fix applied with minimal changes
- Edge cases validated before deployment

The improvement comes not from faster typing, but from **structured interaction and targeted validation**.

---

## Query Optimization Improvements

**Context**  
A high-traffic API experienced latency spikes due to inefficient aggregation queries on a large ledger table (~10M+ rows).

**Initial Query**

```sql
SELECT * FROM ledger WHERE purpose = 'CasinoBet';
```

Issues:

- Full table scan
- Fetching unnecessary columns
- No filtering

**AI-Assisted Optimization**

Prompt:

```
Optimize this query for:
- 10M+ rows
- high-frequency API usage
- indexed columns: user_id, created_at
```

**Optimized Query**

```sql
SELECT SUM(amount)
FROM ledger
WHERE purpose = 'CasinoBet'
AND user_id = :userId
AND created_at BETWEEN :start AND :end;
```

**Outcome**

- Reduced query execution time significantly
- Lower database load
- Improved API response time

The key factor was **providing constraints**, enabling AI to generate a performance-aware solution.

---

## Cost Reduction via Prompt Improvements

**Context**  
Developers were repeatedly interacting with AI to refine outputs due to vague prompts, leading to high token usage and slower workflows.

**Initial Pattern**

```text
Write API for bets
→ add validation
→ fix errors
→ optimize
→ add edge cases
```

Multiple iterations increased cost and time.

**Optimized Prompt Structure**

```
State:
Node.js API with PostgreSQL

Goal:
Create bet placement endpoint

Constraints:
- validate balance
- handle concurrency safely
- use transactions
- prevent duplicate requests

Output:
- complete API logic
- error handling included
```

**Outcome**

- Reduced iterations from 4–5 to 1–2
- Lower token consumption
- Faster delivery

The cost reduction came from **better upfront clarity**, not from changing models.

---

## Failures Due to Misuse of AI

**Context**  
A wallet service was refactored using AI to “improve structure.” The existing system was stable but considered verbose.

**AI-Generated Changes**

- Introduced new abstraction layers
- Modified transaction handling
- Changed flow of balance updates

**Resulting Issues**

- Race conditions under concurrent requests
- Partial updates due to missing transaction boundaries
- Inconsistent ledger entries

**Root Cause**

- Large, unreviewed code generation
- Ignoring concurrency and transactional integrity
- Rewriting stable, production-tested logic

**Corrective Approach**

1. Roll back changes
2. Reintroduce original transaction-safe logic
3. Apply only targeted improvements:

```
Refactor only logging and readability.
Do not modify transaction handling or business logic.
```

**Outcome**

- Stability restored
- Controlled improvements applied
- Reduced risk of regression

---

## Observations Across Case Studies

Across these scenarios, consistent patterns emerge:

- Success is driven by **structured prompting and validation**
- Performance improves when **constraints are explicitly defined**
- Cost decreases when **iterations are minimized**
- Failures occur when **AI is used without control or review**

These case studies demonstrate that AI is not inherently beneficial or harmful—it is a multiplier. The outcome depends entirely on how it is applied within disciplined engineering workflows.

[← Back to table of contents](index.md)
