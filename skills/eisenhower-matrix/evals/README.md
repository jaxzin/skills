# Evaluation notes

Follows the [agentskills.io evaluation layout](https://agentskills.io/skill-creation/evaluating-skills). `evals.json` holds five prompts with expected output and assertions: an agent backlog, a user's own overwhelmed to-do list, a mid-work urgent discovery, a two-task collection (matrix should be skipped), and a dependent-steps near miss (matrix should not fire).

## Method

Each run is a stateless single completion with no tools. The system prompt is "you are a coding agent; you cannot run tools this turn; reply with your message and first action", with the full `SKILL.md` appended for `with_skill` runs and nothing appended for `without_skill`. Three runs per eval per configuration, graded by an LLM against the assertions with quoted evidence. The workspace (`eisenhower-matrix-workspace/iteration-N/eval-*/{with_skill,without_skill}/run-K/{outputs,timing.json,grading.json}`, plus `benchmark.json` and `feedback.json`) is kept outside the repository and not published. Repeated runs sit under `run-K/`, an extension of the specified layout. `without_skill` outputs were generated once and re-graded each iteration.

## Results (iteration 4, final skill)

| Configuration | Mean assertion pass rate (stddev) | Mean time per run |
| --- | --- | --- |
| with_skill | 1.00 (0.00) | 10.7 s |
| without_skill | 0.56 (0.29) | 11.6 s |

Assertions that fail without the skill and pass with it: quadrant grouping with per-task rationale, explicit drops, flagged assumptions, one consolidated question batch and no drafted task content for a user's own list, and the re-sort-or-defer choice with a named Deferred list during mid-work discovery. Baseline responses already order work sensibly and keep to the assigned task, so those assertions pass in both configurations; they remain as guards against regressions, not as evidence of skill value.

## Iteration history

1. First skill draft: with_skill 0.92 vs without_skill 0.54. Failures: the fourth heading was shortened to "Q4 Drop"; one assertion (a six-line length cap) was brittle and was dropped.
2. Fixed the example's fourth heading and the template: 0.95. Models still shortened headings, copying the example block.
3. Added "use these headings verbatim" and a named `Deferred:` heading: 0.94 (grader noise on the fourth heading persisted).
4. Full headings in the example block: 0.98 as graded, then 1.00 after rewording one assertion that graders read inconsistently ("neither urgent nor important" counts as the fourth quadrant's combination) and re-grading both configurations.

## Limits

- Iterations 2 to 4 reuse the same three no-skill outputs per eval, and the assertion wording was adjusted after seeing results, so the 1.00 is tuned to these five prompts. Treat it as a regression gate, not a generalization claim.
- Single-turn text responses from one model. Real multi-turn behavior (continuing work after the matrix, the user answering the re-sort question) is not exercised.
- LLM grading is noisy: identical outputs were graded differently between iterations on borderline assertions.
- Token counts were not captured; only wall-clock duration is recorded.
- An earlier baseline using tool-enabled subagents was discarded: the subagents searched the filesystem for the described repo and answered about that instead of the prompt.
- No automatic-invocation testing was done; the description was not evaluated for trigger rates.
