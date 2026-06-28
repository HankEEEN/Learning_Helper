---
name: prompt-rewrite
description: Rewrite, refine, improve, clarify, or strengthen a user-supplied prompt so it is clearer, more specific, and better structured for LLM consumption. Use when the user asks to rewrite a prompt, improve intent clarity, define expected output, add persona or role framing, tighten scope, add constraints, or improve tone while preserving the user's original intent and preferences.
---

# Prompt Rewrite

Rewrite the user's prompt so it communicates intent precisely, sets clear expectations for the model, and removes ambiguity. Preserve the user's original intent, preferred style, persona or role framing, examples, constraints, and output expectations.

## Process

1. **Read the original prompt** - understand the user's core goal.
2. **Identify weaknesses** - note vagueness, missing context, undefined scope, unclear output format, missing persona, weak constraints, or conflicting instructions.
3. **Preserve preferences** - keep any stated persona, tone, audience, format, constraints, examples, exclusions, and model/tool preferences.
4. **Rewrite** - produce a refined version following the principles below.
5. **Explain changes** - briefly note what was improved and why.

## Refinement Principles

### Clarity of intent

State *what* the model should do, not just *what topic* to address.

- Weak: `Tell me about caching.`
- Strong: `Explain the trade-offs between in-memory caching (e.g. Redis) and CDN-level caching for a high-traffic REST API, focusing on latency, cost, and cache invalidation complexity.`

### Defined output format

Specify the expected structure, length, or style when it matters.

- Weak: `Write a summary.`
- Strong: `Write a 3-sentence executive summary suitable for a non-technical stakeholder. Use plain language and avoid jargon.`

### Scoped context

Include only the context the model needs - no more, no less.

- Weak: `Here is my entire codebase. Fix the bug.`
Strong:
```text
The function `calculateTotal()` in `cart.ts` returns `NaN` when the cart is empty. Fix it so it returns `0` instead.
```

### Actionable constraints

Encode real constraints explicitly.

| Constraint type | Example |
|---|---|
| Audience | "Explain to a junior developer with no prior knowledge of OAuth." |
| Format | "Return a JSON object with keys: `title`, `summary`, `tags`." |
| Length | "Keep the response under 200 words." |
| Tone | "Use a professional but approachable tone." |
| Exclusions | "Do not suggest solutions that require a database migration." |
| Tools | "Use only built-in Python libraries." |

### Persona / role (when beneficial)

Adding a role frame helps steer style and depth. Preserve a persona supplied by the user. Add one when it improves evaluation criteria, domain depth, or tone.

- Strong: `You are a senior security engineer. Review this authentication flow for vulnerabilities.`

### Preference preservation

Do not polish away the user's intended behavior. Preserve explicit requirements even when rewriting the wording. This includes role framing, output sections, response tone, examples, audience level, formatting constraints, and any stated exclusions.

## Output Structure

Present the rewritten prompt clearly, then briefly note the changes:

```markdown
## Rewritten Prompt

[Rewritten prompt text here]

---

**Changes made:**
- Added [specific improvement]
- Clarified [ambiguous part] by [how]
- Preserved [persona, output format, constraint, tone, or other user preference]
```

Use `Refined Prompt` instead of `Rewritten Prompt` if the user explicitly asks for refinement wording.

## Examples

**Example 1 - Vague request**

Original:
> Make my code better.

Rewritten:
> Review the following Python function for readability, performance, and edge-case handling. Suggest specific, actionable improvements with brief explanations. Preserve the existing function signature.

---

**Example 2 - Missing output definition**

Original:
> Write a cover letter.

Rewritten:
> Write a professional cover letter for a Senior Product Manager role at a B2B SaaS company. The tone should be confident and direct. Length: 3 paragraphs (opening, value proposition, closing). Do not use generic phrases like "I am excited to apply."

---

**Example 3 - Underspecified technical request**

Original:
> Help me write a SQL query.

Rewritten:
> Write a PostgreSQL query that returns the top 10 customers by total order value in the last 90 days. The schema has: `orders(id, customer_id, total, created_at)` and `customers(id, name, email)`. Return columns: `customer_name`, `email`, `total_spent`. Sort descending by `total_spent`.

---

## When to Ask for Clarification

If the original prompt lacks enough signal to infer intent, ask **one focused question** before rewriting:

- "What output format do you need - prose, bullet points, or code?"
- "Who is the intended audience?"
- "What constraints should the model follow (length, tools, style)?"

Avoid asking multiple questions at once. Infer what you can; ask only when the gap is too large to bridge.