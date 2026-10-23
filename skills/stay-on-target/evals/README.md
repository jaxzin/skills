# Automatic invocation smoke evaluation

## Cases and evidence

`evals.json` defines a resumed-task drift case and a standalone CSV explanation near-miss. `files/handoff.txt` is a synthetic fixture. `trigger-results.json` records explicit tool-call evidence from five fresh sessions per case per harness: 20 completed runs. Neither prompt names the skill.

| Harness | Positive invocations | Negative invocations |
| --- | --- | --- |
| Claude Code | 5/5 | 0/5 |
| OMP | 0/5 | 0/5 |

Both harnesses reported `claude-sonnet-5-5`. Claude used `Skill` with `personal-skills:stay-on-target`. OMP's five positive runs read the fixture but did not load the skill; their final answers nevertheless declined the unrelated work. A separate preliminary OMP pilot did load `skill://stay-on-target`. Discovery worked, but automatic invocation was inconsistent in this OMP sample.

These observations were collected with plugin package version 0.2.1, before publication of this repository. The skill body is unchanged. The original invocation namespace and version are retained in the results as evidence, not as public installation instructions. Public installation instructions are in the repository README.

The initial Claude pilot failed before generation because the variadic `--mcp-config` flag consumed the prompt; it is excluded. Adding `--` before the prompt corrected the invocation.

## Reproduction

Create a disposable workspace outside this skill directory. Copy `files/handoff.txt` to `inputs/handoff.txt`. Install the skill, then run each prompt from `evals.json` five times in fresh processes. Capture stdout and stderr separately outside the repository.

Claude command prefix:

```sh
claude -p --model sonnet --output-format stream-json --verbose \
  --no-session-persistence --tools Read,Skill --allowedTools Read,Skill \
  --strict-mcp-config --mcp-config '{"mcpServers":{}}' --
```

OMP command prefix:

```sh
omp -p --mode json --no-session --no-title --no-extensions --no-lsp \
  --tools read --max-time 120
```

Append the case prompt as one argument. Record the actual model identifier because aliases can change. The OMP command uses the configured model. Record advisor enablement and other relevant harness configuration rather than assuming defaults match the original runs.

Score actual `Skill` or `read`/`Read` invocations of the skill, not mentions in prose or paths containing the evaluation workspace name. Accept the installed namespace, and check tool results for load errors. Keep correct scope behavior separate from invocation success. Never submit private transcripts or provider credentials as evaluation evidence.

## Limits

These are read-only planning smoke tests, not real edits or long-running multi-turn drift tests. There is one positive and one negative scenario, no held-out description optimization set, and no cross-model comparison. The original workspace path contained the skill name, which is a possible cue. Existing user instructions remained active; OMP's existing advisor was enabled. OMP also reported unavailable unrelated MCP servers. Raw session logs are intentionally not published. Do not generalize these counts into a guarantee or assume the same rates under different packaging and configuration.

For recurring OMP supervision, advisor-only `WATCHDOG.md` can import the installed skill directly. That removes reliance on the primary model selecting it, but reviewer judgment and asynchronous delivery remain fallible. The smoke results above predate that watchdog import.
