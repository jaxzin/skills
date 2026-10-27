# interview-me evals

Follows the agentskills.io guide ("Evaluating skill output quality", "Optimizing skill descriptions") and the `skill-creator` file layout.

| File | Purpose |
|---|---|
| `evals.json` | 5 output evals: `prompt`, `expected_output`, `files`, `expectations`. The field is `expectations` (skill-creator's schema; agentskills.io calls the same thing `assertions`). |
| `files/` | Fixtures the prompts refer to. |
| `trigger_queries.json` | 20 trigger queries (10 should trigger, 10 should not; the negatives are near-misses). `split` is fixed: 12 train, 8 validation. Do not reshuffle between iterations. |

## Running the output evals

Run each eval twice from a clean context: with the skill, and without it (or with the previous `SKILL.md` snapshot). Put runs in a workspace outside the skills directory, `<workspace>/iteration-N/eval-<name>/{with_skill,without_skill}/`, with `outputs/`, `timing.json` and `grading.json`; aggregate into `benchmark.json`.

The skill ends a turn by calling `ask`, which a test run cannot answer. So each run is told: do not call `ask`; write the JSON arguments you would pass to `outputs/questions.json`, save the reply to `outputs/final-message.md`, then stop. The prompt paths are rewritten to a per-run `inputs/` copy of `files/`, so a run never sees this directory or the expectations. Baseline runs are told not to use any skill named `interview-me`.

Grade blind: copy both runs' `outputs/` under labels A and B, shuffle which is which, and keep the mapping aside. Require quoted evidence for PASS. Check mechanical expectations (JSON shape, counts, unchanged input hashes) with a script.

Expectations that need a tool-call log cannot be graded, because the harness keeps only the final reply. Write expectations that the outputs can show.

## Trigger evals

A query passes when its trigger rate is at least 0.5 for a should-trigger query, and below 0.5 for a should-not-trigger query. Use 3 runs per query. Select a revised description by its validation score, not its train score. Keep the description under 1024 characters.

## Iteration 1 (2026-10-21)

Output evals, 1 run per eval per configuration, blind-graded by subagents on the same model family as the executor:

| | pass rate | mean duration |
|---|---|---|
| with skill | 0.975 | 54.7 s |
| without skill | 0.753 | 51.0 s |

Tokens were not captured; the task notifications reported duration only. Two expectations were excluded in both configurations because they needed a tool-call log, and one was replaced by a hash check. All three were reworded afterwards, so `evals.json` no longer matches iteration 1's text for those.

What the skill bought, from the failures of the runs without it: it stayed within 7 questions (the baseline asked 10), used `multi: true` and escape options, anchored questions in scenarios, opened with the most consequential decision, did not set `recommended`, and flagged its own drafted reasons when revising. It did not help on the look-up-before-asking eval, where both runs passed everything, so that eval does not yet discriminate.

Trigger evals: a proxy, not the real thing. Each query was put to a model with the `interview-me` description and 31 other skill descriptions (a subset of the real skill list) and asked which single skill it would load, 3 times per query. Result: 12/12 train and 8/8 validation. A real run needs the harness's skill-invocation log. Because every query passed, the set may be too easy; add harder near-misses before trusting it to tune the description.

## Iteration 2 (2026-10-21)

Same method, fresh runs, with the reworded expectations and a harder look-up fixture (a comment says jobs are killed after 60 minutes and retried once; the file sets 30 and has no retry). Blind-graded again, 1 run per eval per configuration.

| | pass rate | mean duration |
|---|---|---|
| with skill | 0.975 | 57.5 s |
| without skill | 0.800 | 45.6 s |

Pooled over both iterations (2 runs per eval per configuration): with skill 0.975, without 0.777.

Where the skill held up across both iterations: it stays within 7 questions (the baseline asked 8 to 10 on the open-ended evals and 9 on the audit), anchors questions in scenarios, and keeps one decision per question. On the apply-answers eval it separated confirmed, changed and open, and flagged reasons the agent had written; the baseline did not either time.

Not yet discriminating:
- `look-up-before-asking`: both configurations passed everything again, and both caught the 30 vs 60 minute contradiction. The baseline already reads the file first. Needs a trap the baseline will not catch by reading, or should be dropped.
- `one-open-item-no-padding`: both passed 5/5.

Weak or contested assertions found by the graders:
- "ordered from the most consequential decision (whether self-merge is ever allowed)" failed for both configurations in iteration 2 and passed for the skill in iteration 1. Ordering is a judgment call; consider requiring only that the first question is a decision rather than a diagnosis.
- "multi-select among retry, fail and notify" is satisfied by bundled options; require it on the timeout-action question.
- "depends or something else option" is judged inconsistently when the option appears only in the final message, not in `questions.json`.

Open: tighten the three assertions above, repeat runs to get a meaningful stddev (still 1 run per cell), run the trigger evals against the real skill-invocation log, and capture tokens.
