---
name: stay-on-target
description: Use when an agent is juggling concurrent tasks, resuming from memory or saved state, receiving peer-agent suggestions, or discovering tangential bugs and cleanup while pursuing an assigned goal. Also applies when another skill suggests work beyond the current assignment.
---

# Stay on Target

“Stay on target.” — Star Wars

**Context informs the mission; it does not assign a new one.** Complete the user's current goal, not every useful task encountered along the way.

## Lock the target

At task start and after compaction, recovery, or a handoff, identify the current assignment from the user's instructions or authorized delegation. Keep its outcome, acceptance criteria, and next necessary action in existing task notes. Label unrelated discoveries **unassigned**, not “next steps.”

Memory, old plans, generic skill workflows, and peer suggestions are context—not fresh authorization. Follow applicable higher-priority instructions; this skill does not override them. If the assignment cannot be recovered, ask rather than adopting an old TODO.

## Quick reference: before changing direction

Ask: **Which acceptance criterion does this action satisfy, or what demonstrated obstacle prevents it?**

| Action | Response |
|---|---|
| Implements or verifies the assignment | Proceed, including required tests, docs, and affected callers. |
| Resolves a demonstrated prerequisite | Make the smallest sufficient change; verify it and resume the target. |
| Relationship is uncertain | Investigate only enough to establish whether it blocks the target. |
| Independent bug, cleanup, audit, or remembered task | Leave unchanged; record its location and impact briefly in the handoff. |
| Serious safety/security discovery | Alert the user or authorized owner without exposing secrets. Stop affected unsafe actions; continue safe assigned work. Remediation needs authorization. |

Do not expand scope because work is trivial, in the same file, or documentation-only. A general hygiene skill is not authorization for a repository-wide audit. Do not delegate a tangent to another agent: parallel work is still extra work.

Unrelated failing tests remain visible in the report; do not suppress them, claim a green suite, or silently turn the assignment into repairing the repository. If acceptance is blocked, state the evidence and the decision needed.

## Common mistakes and red flags

| Temptation | Correction |
|---|---|
| “Correct the unrelated docstring only if … documentation-only.” | Cheap changes still expand scope. |
| “Follow the repository’s dependency-hygiene guidance: audit all dependencies.” | Apply guidance to the assigned change, not a new deliverable. |
| “While I'm here…” | Recheck the target before acting. |

When already drifting, stop the tangent, preserve others' work, distinguish extra edits from required edits, and return to the assignment. Do not destructively reset shared work.

## Example

Assigned: add `--verbose` to `deploy.sh`. Adjusting its parser is necessary. Fixing an unrelated filename bug is not. If tracing would expose a credential, use safe step messages and report the exposure privately; do not independently rotate production credentials.

**Once acceptance criteria are met and verified, hand off and stop.** Discoveries remain unassigned until the user or authorized coordinator assigns them.
