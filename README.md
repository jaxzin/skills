# Skills

Reusable agent skills by [jaxzin](https://github.com/jaxzin). This is the public collection, separate from project-local experiments and private operational skills.

## Install

Use the [Skills CLI](https://github.com/vercel-labs/skills) with Node.js 22.20 or later:

```sh
# List the available skills without installing them.
npx skills add jaxzin/skills --list

# Install one skill into the current project.
npx skills add jaxzin/skills --skill stay-on-target

# Install globally for Claude Code.
npx skills add jaxzin/skills --skill stay-on-target -g -a claude-code
```

The CLI prompts for a target agent when one is not specified. Installation locations depend on the selected agent; see the [supported-agent reference](https://github.com/vercel-labs/skills/blob/main/docs/agents.md).

## Available skills

| Skill | When to use it |
| --- | --- |
| [eisenhower-matrix](skills/eisenhower-matrix/SKILL.md) | Several competing tasks, or a user asking to sort their own priorities, where order and what to drop must be decided. |
| [stay-on-target](skills/stay-on-target/SKILL.md) | Memories, saved state, other skills, or discoveries are pulling an agent away from its current assignment. |
| [technical-documentation](skills/technical-documentation/SKILL.md) | Writing or revising technical documentation that must describe verified, current behavior. |

Skills guide model behavior; they are not enforcement mechanisms. Automatic invocation depends on the agent and its configuration. The [stay-on-target evaluation](skills/stay-on-target/evals/README.md) distinguishes observed invocation from on-task behavior and documents its limitations. This repository does not claim equivalent evaluation coverage for technical-documentation.

## Skill promotion: local → private → public

Promotion is deliberate, not an automatic sync between repositories. A useful local skill need not become public.

| Stage | Appropriate content | Promotion gate |
| --- | --- | --- |
| Project or harness local | Experiments tied to one task, project, or agent configuration. | Repeated use demonstrates a reusable technique; capture observable failures and test the guidance. |
| Private collection | Reusable personal or organizational workflows, including private infrastructure dependencies. | Remove private assumptions and data, establish redistribution rights, and demonstrate use outside the original environment. |
| Public collection | Self-contained guidance and safe supporting artifacts that others can install and use. | Review the exact public payload, validate discovery, and publish with an explicit license and honest evaluation evidence. |

Before promoting a skill here:

1. Select only the skill and required supporting files. Do not mirror private repository history or export a harness configuration directory.
2. Remove credentials, internal endpoints, personal paths, private repository references, customer data, and identifiable recordings or transcripts. Use synthetic evaluation fixtures.
3. Check permissions and third-party licenses. Retain required attribution; do not assign this repository's license to material you cannot redistribute.
4. Exercise behavior and discovery separately. Record the prompts, harness/model, repetitions, expected outcomes, results, and limitations under the skill's `evals/` directory. Keep raw private session logs outside the repository.
5. Scan the proposed files and Git history for secrets, manually review non-secret privacy details, and run the discovery-only install command above.
6. After promotion, maintain the public version here. Private adaptations remain private; upstream only deliberately reviewed, generalized changes. Update existing consumers explicitly rather than silently repointing them.

## Optional OMP scope review

For recurring supervision, an OMP advisor can import `stay-on-target` into its own prompt instead of relying on the primary model to invoke it. Installation alone does not configure an advisor.

OMP discovers shared skills under `~/.agents/skills/` when that source is enabled. To install this skill there through the CLI's Pi target:

```sh
npx skills add jaxzin/skills --skill stay-on-target -g -a pi
```

Add this to your user-level `~/.omp/agent/WATCHDOG.md`, preserving any existing guidance:

```markdown
# Scope review

@~/.agents/skills/stay-on-target/SKILL.md

Apply the imported skill as a reviewer of the primary agent.
At each review, check its actions against the current authorized task.
When it drifts, identify the goal, the concrete deviation, and the next
in-scope action. Use advise; do not perform the task yourself.
Stay silent when it is on target.
```

If your installed skill lives elsewhere, use its actual installed path. Prefer an installer-maintained stable path over a version-specific cache directory. Do not copy the skill's rules into the watchdog.

Enable the advisor and set `reviewMode: turn` and `reviewInterval: 1` for review after each primary tool-call round. Restart OMP after changing watchdog instructions, then inspect `/advisor dump raw` to verify the skill was expanded into the advisor prompt. Advisor review and delivery are fallible; asynchronous review can lag behind the primary agent, and the advisor is not an approval gate.

## License

[MIT](LICENSE).
