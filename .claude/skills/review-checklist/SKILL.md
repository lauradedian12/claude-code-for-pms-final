---
name: review-checklist
description: Reviews a product brief against a fixed six-point checklist (owner, how we'll know it worked, scope start vs end, problem before fix, success metrics with baselines, open questions). Use when the user says "review this brief", "run the review checklist", "check this brief", or points at a brief file before it goes any further.
---

# Review checklist

Run the same six checks on any brief, every time. Do not edit the brief. Report only.

## Input

The brief is a file path or pasted text. If none is given, ask for it in one line. Read the whole brief before judging anything.

## The six checks

1. **Owner.** The brief names who owns it, by name or role. A team name alone does not count.
2. **How we'll know it worked.** The brief says what outcome would show it worked, in plain terms.
3. **Scope matches.** Compare the scope stated at the start with the scope at the end (what is proposed, built or promised). Flag anything that appears in one and not the other, in either direction.
4. **Problem before fix.** The problem is explained before any solution is proposed. Flag a fix that shows up first, and a problem section that is only a restated solution.
5. **Success metrics.** Each metric has a baseline (a current number and where it came from) or an explicit placeholder such as "baseline: TBD, ask <who>". A metric with neither fails.
6. **Open questions.** If the brief has unknowns, assumptions or unconfirmed numbers, they are listed as open questions. If it has none listed but the text hedges ("probably", "assume", "unconfirmed"), flag that. If it truly has none, a pass is fine.

## Rules

- Judge only what is written in the brief. Do not fill gaps from memory or other files, and do not invent owners, numbers or baselines.
- Every Pass or Fail cites the brief: quote a short phrase or name the section. For a Fail on a missing item, say "not found".
- Use Partial when the item is there but weak (for example, a metric with no baseline or placeholder).
- Checks 2 and 5 overlap. Check 2 is the outcome in words; check 5 is the numbers. Score them separately.

## Output

Use this format, in this order:

```
Brief: <title or filename>
Verdict: <Ready to go on | Fix first | Not ready>

| # | Check | Result | Evidence |
|---|-------|--------|----------|
| 1 | Owner | Pass / Partial / Fail | <quote or "not found"> |
| 2 | How we'll know it worked | ... | ... |
| 3 | Scope matches | ... | ... |
| 4 | Problem before fix | ... | ... |
| 5 | Success metrics | ... | ... |
| 6 | Open questions | ... | ... |

Fix first:
1. <the single most important fix, one line>
2. <next, up to 5 total>
```

Verdict rule: all Pass = Ready to go on. Any Fail = Not ready. Otherwise (Partial only) = Fix first.

End with one line: the first fix to make, and who could answer it if the brief names them.
