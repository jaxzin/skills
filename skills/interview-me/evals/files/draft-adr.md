---
title: Agents never force-push to a shared branch
status: evergreen
---
# Agents never force-push to a shared branch

**Decided 2026-10-14 by Sam.** Agents must not run `git push --force` on `main` or on any branch another person or agent has pushed to. They may use `--force-with-lease` on branches they created themselves.

## Why this and not the alternatives

| Alternative | Rejected because |
|---|---|
| Ban all force pushes | You answered that agents need them to tidy their own branches |
| Allow force-push after asking me | Asking every time is too much friction |
| Rely on branch protection alone | It only covers repos that have it switched on |

## Background

During the October migration an agent force-pushed over two commits on `release/2.3` (see #412). Sam noticed, told the agent to stop, and a second agent restored the commits from its reflog. We then agreed in chat to write the rule down.

## Consequences

- An agent that needs to fix a mistake on a shared branch opens a PR instead.
- Rewriting history on a branch an agent created is still allowed.
