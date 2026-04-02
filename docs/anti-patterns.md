---
title: "Anti-Patterns (Critical Section)"
description: Trust, bulk generation, rewrites, model misuse, ignored edge cases, and over-engineering—common failure modes in AI-assisted development.
---

# Anti-Patterns (Critical Section)

AI amplifies both good and bad engineering practices. When used without discipline, it can accelerate mistakes, propagate flawed logic, and degrade system quality at scale. The following anti-patterns represent the most common—and most costly—failure modes in AI-assisted development.

---

## Blindly Trusting AI Output

AI-generated code often appears complete and confident, which creates a false sense of correctness. However, the model does not validate its output against your system’s actual constraints, data, or business logic.

Blind trust leads to:

- Logical errors in production
- Hidden edge-case failures
- Security and data integrity risks

**Example**

```sql
SELECT SUM(amount) FROM ledger WHERE purpose = 'CasinoBet';
```

This looks correct but may:

- Ignore rollback entries
- Miss required filters (user, date)
- Produce incorrect financial results

Correct usage requires treating AI output as a **draft requiring verification**, not a final implementation.

---

## Large, Unreviewed Code Generation

Generating large systems or entire files in a single prompt introduces compounded risk. Errors are harder to detect because multiple concerns—logic, structure, edge cases—are mixed together.

Common consequences:

- Inconsistent architecture
- Missing validations
- Difficult debugging due to unclear boundaries

**Example**

```
Build a complete jackpot system with APIs, DB schema, and cron jobs
```

This typically results in:

- Partial correctness across components
- Poor integration between layers
- Hidden assumptions that break under real conditions

A controlled, incremental approach is always more reliable.

---

## Rewriting Stable Systems

AI often suggests “improved” or “cleaner” versions of existing code. While these suggestions may look better, rewriting stable, production-tested systems introduces unnecessary risk.

Risks include:

- Regression bugs
- Loss of implicit business logic
- Increased testing overhead

**Example**

```
Refactor this entire wallet service for better structure
```

If the current system is stable, such changes should be avoided unless there is a **clear, measurable benefit**.

Refactoring should be:

- Incremental
- Justified by performance or maintainability gains
- Carefully validated

---

## Overusing High-Cost Models

Using high-end models for all tasks increases cost without proportional benefit. Many tasks do not require deep reasoning and can be handled by simpler models.

Consequences:

- Increased operational cost
- Slower response times
- Reduced iteration speed

**Example**

Using a high-end model for:

```
Convert JSON to TypeScript interface
```

This provides no additional value compared to a low-cost model.

Model selection should always match task complexity.

---

## Ignoring Edge Cases

AI defaults to common scenarios unless explicitly instructed otherwise. Ignoring edge cases leads to fragile systems that fail under real-world conditions.

Common missed cases:

- Null or missing data
- Concurrent requests
- Partial failures
- Boundary conditions

**Example**

```
Calculate winnings for a bet
```

Without edge cases:

- What if the match is canceled?
- What if selections are invalid?
- What if data is incomplete?

Failing to define these leads to incorrect outcomes in production.

---

## Over-Engineering Solutions

AI tends to introduce abstractions, patterns, or optimizations that are not required. While these may appear sophisticated, they increase complexity without improving functionality.

Symptoms of over-engineering:

- Unnecessary design patterns
- Excessive abstraction layers
- Generic solutions for specific problems

**Example**

```javascript
function calculate(input, strategy) {
  return strategy.execute(input);
}
```

If there is only one calculation path, this abstraction adds no value and makes the code harder to follow.

Prefer simple, direct implementations unless there is a clear need for extensibility.

---

## Recognizing and Avoiding Anti-Patterns

These anti-patterns share a common root: **lack of control over AI usage**. Avoiding them requires:

- Structured prompting
- Incremental development
- Rigorous validation
- Conscious decision-making

AI is a powerful multiplier, but without discipline, it multiplies mistakes just as effectively as it multiplies productivity.

[← Back to table of contents](index.md)
