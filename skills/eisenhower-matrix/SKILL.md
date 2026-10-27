---
name: eisenhower-matrix
description: Use when faced with two or more competing tasks where order or inclusion is a choice, such as a multi-item request, backlog, TODO list, or review queue, or when a user asks for help sorting or prioritizing their own to-do list or feeling overwhelmed. Also when new tasks turn up while other work is underway.
---

# Eisenhower Matrix

Sort a collection of tasks into four quadrants so the order of work, and what to drop, is decided deliberately instead of by list position or loudness.

Skip it for a single task and for steps of one plan that depend on each other. For three or fewer tasks that are all clearly Q1, or where only one order is possible, give the order in one line.

## Placing tasks

Place each task by what you can observe, not by tone:

- **Urgent**: a deadline, a blocker, or someone waiting makes delay costly.
- **Important**: it advances the stated goal or the intended outcome of the request.

If the evidence doesn't settle a placement, make your best placement and record it under Assumptions.

## Output

Every input task appears exactly once. Use these headings verbatim, including the parenthetical, so each quadrant stays named by its urgent/important combination:

```
**Q1 Do first** (urgent + important)
1. <task>: <evidence for placement>
**Q2 Schedule** (important, not urgent)
- <task>: <when or trigger>; <evidence>
**Q3 Delegate or minimize** (urgent, not important)
- <task>: <who or how>; <evidence>
**Q4 Drop** (neither urgent nor important)
- <task>: dropped because <reason>
**Assumptions:** <placement>: <missing evidence>
**Deferred:** <new tasks not yet sorted>
```

Order within a quadrant: tasks that unblock others first, then cheapest first. Start Q3 handoffs early so they run in parallel. Omit Assumptions or Deferred when empty.

## What happens next

**Agent's own work.** Start the first Q1 task right after the matrix. If Q4 or Assumptions is non-empty, stop after the matrix and ask for confirmation first, because a wrong drop is hard to see later.

**The user's own priorities.** Nothing is executed: no drafts, agendas, or checklists. Q3 means delegate to a person. After the matrix, ask one consolidated batch of questions covering every Assumptions item.

**New task discovered during work.** Discovery never redirects you. Keep working the current task, list the new one under a `Deferred:` heading, and offer the user a choice: *re-run the matrix and re-sort, or keep it deferred.* If it looks urgent, say so immediately in one brief aside; otherwise raise it at the next task boundary. With no answer it stays under `Deferred:`, and you list that heading again at the end.

## Example

Request: "(a) fix login 500 since deploy, (b) rename `utils2`, (c) a teammate needs your approval on their PR today, (d) CEO wants dark mode someday."

```
**Q1 Do first** (urgent + important)
1. Fix login 500: users are failing now.
**Q2 Schedule** (important, not urgent)
- Dark mode: when the user sets scope; advances product goal, no deadline.
**Q3 Delegate or minimize** (urgent, not important)
- Approve teammate's PR: they are waiting; give a quick pass once login is fixed, or hand to another reviewer.
**Q4 Drop** (neither urgent nor important)
- Rename `utils2`: dropped because nothing depends on it and no one is waiting.
**Assumptions:** Dark mode is Q2, not Q4: "someday" came from the CEO, not the user.
```

Q4 and Assumptions are non-empty, so confirm with the user before starting.
