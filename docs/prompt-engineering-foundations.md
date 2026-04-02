---
title: "Prompt Engineering Foundations"
description: Why prompts matter for cost and quality, a four-part structure (state, goal, constraints, output), and clarity vs ambiguity.
---

# Prompt Engineering Foundations

AI systems do not “understand” intent in the way humans do; they infer it from the prompt. This makes prompt construction one of the most critical skills in AI-assisted development. The difference between a vague prompt and a well-structured one is not incremental—it is often the difference between unusable output and production-ready code.

## Why Prompt Quality Directly Impacts Cost and Output

Prompt quality determines three key outcomes: **accuracy, iteration count, and resource usage**.

A poorly written prompt introduces ambiguity. The AI fills these gaps with assumptions, which often leads to incorrect or incomplete results. This forces additional iterations—each requiring more prompts, more responses, and more time.

Each iteration has a cost:

- **Time cost**: Developer effort to re-explain or correct
- **Compute cost**: Tokens consumed across multiple interactions
- **Cognitive cost**: Context switching and re-evaluation

A well-structured prompt, by contrast, reduces the number of iterations required to reach a correct solution. Even if the initial prompt is longer, it is usually cheaper overall because it avoids repeated corrections.

**Example**

**Low-quality prompt:**

```
Write a query for GGR
```

Likely outcome:

- Missing rollback logic
- No filters
- Incorrect aggregation

This leads to multiple follow-ups.

**High-quality prompt (single pass):**

```
Write a PostgreSQL query to calculate GGR.

Context:
- Table: ledger
- Columns: amount, purpose, user_id, created_at

Goal:
- totalBetAmount = sum of CasinoBet
- totalWinAmount = sum of CasinoWin
- subtract CasinoBetRollback from bets

Constraints:
- Filter by user_id and date range
- Must be efficient for large datasets

Output:
- totalBetAmount, totalWinAmount, ggr
```

The second prompt is longer but typically produces a usable result in one iteration.

## Standard Prompt Structure

A consistent structure ensures that prompts are complete and unambiguous. The following four-part structure is sufficient for most development tasks.

### State

The **state** defines the current context. It answers: *What exists right now?*

This includes:

- Existing code or schema
- Tech stack
- Current problem scenario

Without state, the AI operates in a vacuum.

**Example**

```
We are using Node.js with PostgreSQL.
We have a ledger table with columns: amount, purpose, user_id, created_at.
```

### Goal

The **goal** defines the desired outcome. It answers: *What needs to be achieved?*

A vague goal leads to broad or misaligned outputs. A precise goal narrows the solution space.

**Example**

```
Calculate total bet amount, total win amount, and GGR for a given user and date range.
```

### Constraints

Constraints define the boundaries within which the solution must operate. They are essential for aligning output with real-world requirements.

Common constraints include:

- Performance limits
- Data size
- Architectural restrictions
- Business rules

**Example**

```
- Query must run under 100ms
- Must use indexes on user_id and created_at
- Avoid full table scans
```

Constraints prevent the AI from generating theoretically correct but practically unusable solutions.

### Output Format

The **output format** specifies how the response should be structured. Without this, the AI may include unnecessary explanations or omit required details.

Defining format improves usability and reduces post-processing effort.

**Example**

```
Return only the SQL query.
Do not include explanations.
```

or

```
Return:
1. SQL query
2. Brief explanation of optimizations
```

### Combined Example

A well-structured prompt integrates all four components:

```
State:
We are using PostgreSQL. The ledger table has amount, purpose, user_id, created_at.

Goal:
Calculate totalBetAmount, totalWinAmount, and GGR.

Constraints:
- Must filter by user_id and date range
- Handle CasinoBetRollback
- Optimized for large datasets (10M+ rows)

Output:
Return only the SQL query.
```

This structure minimizes ambiguity and produces consistent, high-quality output.

## Importance of Clarity and Specificity

Clarity is not about verbosity—it is about precision. A prompt should eliminate ambiguity without introducing unnecessary detail.

Specificity ensures that:

- The AI understands exactly what is required
- Assumptions are minimized
- Output aligns with real-world use cases

Ambiguous prompts force the AI to guess. Specific prompts constrain it to the correct path.

**Example**

**Ambiguous:**

```
Fix this API
```

**Specific:**

```
Fix this API to:
- Validate user balance before placing a bet
- Prevent duplicate requests (idempotency)
- Return proper error codes (400, 409, 500)
- Use transactions for DB operations
```

The second prompt clearly defines expectations, reducing the likelihood of incorrect or incomplete output.

Clarity also improves maintainability. Well-structured prompts can be reused, shared across teams, and standardized, leading to consistent results across different developers and use cases.

---

Prompt engineering is not a peripheral skill; it is central to effective AI usage. The ability to define state, articulate goals, enforce constraints, and demand structured output determines both the efficiency and reliability of AI-assisted development.

[← Back to table of contents](index.md)
