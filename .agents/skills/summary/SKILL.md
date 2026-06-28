---
name: summary
description: Summarize, condense, recap, give an overview, or extract main points from text, documents, conversations, meeting notes, articles, code, or technical material. Use when the user asks for a TL;DR, key points, executive summary, conversation recap, technical summary, action items, results, assumptions, confidence, or structured findings.
---

# Summary

Produce a clear, structured summary tailored to the content type and the user's needs. Preserve the source meaning, important attributions, results, assumptions, confidence level, and open items when relevant.

## Process

1. **Identify the content type** - article, conversation, document, code, meeting notes, technical design, etc.
2. **Identify the desired output** - key points, executive summary, TL;DR, structured breakdown, results, assumptions, confidence, or action items.
3. **Determine the audience** - technical vs. non-technical, detail-oriented vs. time-constrained.
4. **Extract required signals** - main points, results, assumptions, evidence, limitations, confidence, decisions, and next steps when present.
5. **Generate the summary** using the appropriate format below.

## Choosing the Right Format

| Situation | Format to use |
|---|---|
| Long document / report | Executive Summary |
| Chat / conversation thread | Conversation Recap |
| Article / blog post / paper | Key Points |
| Quick read / informal ask | TL;DR |
| Code or technical content | Technical Summary |
| Analytical or decision-support request | Structured Summary |
| Tasks, decisions, owners, or follow-ups | Action Items |

Default to **Key Points** if the user does not specify a format. Use **Structured Summary** when the user asks for results, assumptions, confidence, limitations, or decision support.

---

## Required Signals

Include these when the source or user request supports them. Do not invent unsupported details.

- **Results** - outcomes, findings, conclusions, decisions, or observable effects.
- **Assumptions** - stated assumptions, implied dependencies, or conditions required for the summary to hold.
- **Confidence** - High, Medium, or Low, with a short reason based on source clarity, completeness, ambiguity, and evidence quality.
- **Evidence** - source details, examples, metrics, or claims that support the main points.
- **Limitations** - missing context, unresolved contradictions, weak evidence, or scope boundaries.
- **Open items / next steps** - unresolved questions, action items, owners, deadlines, and follow-ups.

---

## Output Formats

### TL;DR

One to three sentences. Captures the single most important idea.

```markdown
**TL;DR:** [One to three sentence distillation of the core message.]
```

Add confidence only when the source is ambiguous or the user asks for it.

---

### Key Points

Ordered list of the most important facts, findings, or arguments. Each point is a single, self-contained sentence.

```markdown
## Key Points

- [Most important point]
- [Second most important point]
- [Third most important point]

**Confidence:** [High / Medium / Low] - [short reason, when useful]
```

Limit to 7 points. If more exist, group related ones or promote only the highest-signal items.

---

### Structured Summary

Use when the user asks for results, assumptions, confidence, limitations, or a decision-support summary.

```markdown
## Summary

**Result:** [Most important outcome, conclusion, or answer.]

**Key points:**
- [High-signal point]
- [High-signal point]
- [High-signal point]

**Assumptions:**
- [Assumption or dependency, if present]

**Evidence:**
- [Supporting detail from the source]

**Limitations / open questions:**
- [Missing context, uncertainty, or unresolved question]

**Confidence:** [High / Medium / Low] - [reason]
```

Omit sections that are truly not applicable, except keep `Confidence` for analytical summaries.

---

### Executive Summary

For documents, reports, or proposals. Structured prose; suitable for stakeholders who won't read the full source.

```markdown
## Executive Summary

**Context:** [What this is and why it matters - 1 sentence.]

**Results / key findings:**
- [Finding 1]
- [Finding 2]
- [Finding 3]

**Assumptions / constraints:**
- [Assumption, dependency, or constraint]

**Conclusion / recommendation:** [What should happen next, or what decision this supports - 1-2 sentences.]

**Confidence:** [High / Medium / Low] - [reason, when useful]
```

---

### Conversation Recap

For chat logs, meeting transcripts, or multi-turn discussions.

```markdown
## Conversation Recap

**Topic:** [What was discussed]
**Participants / context:** [Who or what system was involved, if relevant]

**Results / decisions:**
- [Decision or outcome 1]
- [Decision or outcome 2]

**Assumptions:**
- [Assumption or dependency, if relevant]

**Open items / next steps:**
- [Action item or unresolved question]

**Confidence:** [High / Medium / Low] - [reason, when useful]
```

---

### Technical Summary

For code, architecture discussions, or technical documentation.

```markdown
## Technical Summary

**What it does:** [One sentence describing purpose or function.]

**How it works:**
- [Approach, key component, or logic flow]
- [Approach, key component, or logic flow]

**Notable constraints / trade-offs:**
- [Trade-off or limitation]

**Relevant dependencies / requirements:** [If any.]

**Confidence:** [High / Medium / Low] - [reason, when useful]
```

---

### Action Items

For extracting tasks, decisions, owners, and follow-ups.

```markdown
## Action Items

- [Owner, if known]: [Task] - [Due date, if known]

**Open questions:**
- [Unresolved question]

**Confidence:** [High / Medium / Low] - [reason, when useful]
```

Use `Unassigned` only when a task is clearly present but no owner is provided.

---

## Confidence Guidance

Use confidence to describe reliability of the summary, not subjective certainty.

- **High** - source is clear, complete, and internally consistent.
- **Medium** - source is mostly clear but has minor gaps, ambiguity, or missing details.
- **Low** - source is incomplete, contradictory, heavily implied, or lacks enough evidence for firm conclusions.

When confidence is Medium or Low, include the main reason in plain language.

## Quality Rules

- **Be faithful** - do not introduce information not present in the source.
- **Be selective** - prefer signal over completeness. Omit filler, repetition, and tangents.
- **Match the register** - mirror the formality level of the source unless the user specifies otherwise.
- **No editorializing** - summarize what was said, not what you think should have been said.
- **Preserve attributions** - in conversations, attribute decisions or claims to the right party when it matters.
- **Separate results from assumptions** - make clear what is known versus inferred.
- **Preserve uncertainty** - keep limitations, disagreements, and open questions visible when they matter.

## Examples

**Example 1 - Article to Key Points**

Source: A 2,000-word article on remote work productivity trends.

Output:
```markdown
## Key Points

- Remote workers report 13% higher productivity on focused tasks compared to office environments (Stanford, 2024).
- The main drivers are reduced commute time and fewer in-person interruptions.
- Collaboration and mentorship suffer most - junior employees show slower skill development.
- Hybrid models (2-3 days in-office) score highest on both productivity and employee satisfaction metrics.
- Companies that mandate full return-to-office see attrition rates 18% above industry average.

**Confidence:** Medium - The summary depends on the article's reported study design and does not include the underlying methodology.
```

---

**Example 2 - Conversation to Recap**

Source: A Slack thread debating which database to use for a new service.

Output:
```markdown
## Conversation Recap

**Topic:** Database selection for the notifications service.

**Results / decisions:**
- PostgreSQL chosen over MongoDB due to existing team expertise and ACID requirements.
- Redis will be added as a caching layer for high-frequency read queries.

**Assumptions:**
- The service requires ACID guarantees.
- The team can operate PostgreSQL and Redis with existing expertise.

**Open items / next steps:**
- [@alex] to benchmark read latency under 10k concurrent connections by Friday.
- Schema design review scheduled for next Tuesday's architecture sync.

**Confidence:** High - The decisions and next steps are explicit in the thread.
```

---

**Example 3 - Document to Executive Summary**

Source: A 15-page vendor proposal for a new analytics platform.

Output:
```markdown
## Executive Summary

**Context:** Vendor X proposes replacing the current analytics stack with their platform at $120k/year.

**Results / key findings:**
- Estimated 40% reduction in report generation time.
- Requires 6-month migration with 3 FTE months of engineering effort.
- Vendor offers SLA of 99.9% uptime with dedicated support.

**Assumptions / constraints:**
- The projected time savings depend on the vendor estimate being accurate.
- The migration requires engineering capacity for several months.

**Conclusion / recommendation:** Proceed to a 30-day proof-of-concept before committing. Validate the migration timeline estimate with the data engineering team first.

**Confidence:** Medium - The recommendation is supported by the proposal, but vendor claims need independent validation.
```

---

## When to Ask for Clarification

Ask **one focused question** if the required output format is ambiguous:

- "Do you need a quick TL;DR or a detailed executive summary?"
- "Who is the audience - technical team or business stakeholders?"
- "Should I include confidence, assumptions, results, and limitations in every summary?"