# Contributing

Every skill in this repo meets the same bar. `python3 scripts/validate.py` checks the mechanical parts, and CI runs it
on every push and pull request.

## Layout
```
skills/<name>/
  SKILL.md              frontmatter + the workflow
  references/*.md       the depth: tables, templates, checklists, worked examples
  evals/evals.json      2+ test prompts with 3+ assertions each
```

## SKILL.md
- Frontmatter: `name` (the folder name), `description` (250+ characters, written as "When the user ... Also use when
  the user mentions 'x,' 'y,' ..." with 5+ quoted trigger phrases, then which sibling skill it hands off to), and
  `metadata.version`. Keep apostrophes out of the quoted phrases.
- First section: **Before you start**. Read `.agents/charts.md` if it exists (the `chart-context` skill writes it) and
  ask only for what it does not cover.
- Then the workflow as numbered steps an agent can follow end to end, a **Data** section, a **Quality check** list, a
  **What to avoid** list, and **Related Skills** naming 2+ siblings with when to use each.
- The **Data** section names the live stats API (`https://www.pie-chart-generator.com/api/stats?days=30`) and says to
  read only `events.export`, `events.session_start` and `events.data_import`.

## Rules for content
- Every number carries its source and a YYYY-MM date in the same paragraph. No invented benchmarks, customers, quotes
  or reviews. If no source exists, say "ask" or "unknown".
- Examples with made-up data say so ("illustrative data").
- Every skill must be useful with no account and no paid plan.
- Plain words. Short sentences. No em dashes, no hype words, no "it is not X, it is Y" lines.

## evals/evals.json
```json
{"skill_name": "<name>", "evals": [{"id": 1, "prompt": "...", "expected_output": "...", "assertions": ["...", "...", "..."], "files": []}]}
```
`python3 scripts/run_evals.py [skill ...]` runs each prompt through the `claude` CLI with the skill loaded and
WebFetch available, grades every assertion with a second call, and writes `evals/results-<YYYY-MM-DD>.json` as
`{"pass_rate": 0.0-1.0, "cases": [...]}`.
