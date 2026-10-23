---
name: technical-documentation
description: Use when writing or revising technical documentation, READMEs, runbooks, API guides, configuration references, or administrator instructions for a software system
---

# Technical Documentation

## Overview

Write for the reader’s next successful action. Describe the system as it exists now, using verified behavior and concrete examples; omit development history unless the reader explicitly needs a migration path.

This skill combines practical conventions from Google’s developer documentation guidance, Microsoft’s Writing Style Guide, the Diátaxis framework, and Keep a Changelog’s separation of user-facing change information from product documentation.

## Required workflow

1. **Identify the reader and task.** State who is using the document and what they need to accomplish. Choose the smallest document type that fits: tutorial, how-to, reference, explanation, or runbook.
2. **Establish the current contract.** Inspect the current source, configuration, deployed interface, generated output, or authoritative operator evidence. Record versions and dates only when they affect the reader’s action.
3. **Write the path to success.** Put prerequisites, the first command or action, expected output, configuration, failure recovery, and verification in the order the reader needs them.
4. **Validate the document.** Check every command, path, option, example, status code, default, and output shape against the current system. Remove unsupported claims.

## Current-state contract

The document describes only the presently supported system.

- Treat unverified details as unknown. Ask for evidence or label a specific gap; never fill it with a plausible value.
- Do not include “recent refactors,” implementation history, superseded designs, abandoned approaches, or narrative about how the system got here.
- Do not preserve historical context merely because it may be interesting or useful later.
- When historical information is requested but does not belong in this document, omit the section entirely. Do not add a section that says the history is unknown, unavailable, or intentionally excluded.
- Do not use “legacy,” “before v2,” “compatibility,” or similar historical framing in current-state instructions unless the document is explicitly a migration or upgrade guide.
- Include historical content only when the document is explicitly a migration or upgrade guide that tells users or administrators how to transition from an old version to the current version. A request to explain a refactor, framework change, or prior design does not change a current-state document into a history document.
- When a requester asks for that historical explanation inside a current-state document, leave it out of the document. Do not summarize it, disclaim it, or describe its operational impact there.
- If an old version matters to a migration, name its exact version or condition, explain the required transition, and end with the current state.

These are content rules, not suggestions. A changelog, release note, architecture decision record, or migration guide is a different artifact from current operating documentation.

## Style rules

- Lead with purpose and the first useful action.
- Use short sentences, active voice, concrete verbs, and consistent terms.
- Prefer “Run `command`” to “You should run `command`.”
- Use headings that answer a reader’s question: “Install,” “Authenticate,” “Troubleshoot 401 errors.”
- Explain an unfamiliar term at first use; do not repeat the explanation.
- Show one complete, realistic example before listing variations.
- Use tables for compact comparisons and reference data; use numbered steps for sequential work.
- Put warnings immediately before the action they affect.
- Preserve exact capitalization, punctuation, flags, paths, URLs, and response fields from the system.
- Keep examples safe: use placeholders for credentials and never expose secrets.

## Quick reference

| Need | Use |
|---|---|
| Teach a newcomer a complete path | Tutorial with a known result |
| Help an operator complete a task | Numbered how-to or runbook |
| State supported behavior | Reference table, syntax, defaults, errors |
| Explain why a design exists | Explanation, limited to current behavior and constraints |
| Move from an old version to this one | Separate migration guide with explicit old/current states |

## Minimal template

````markdown
# [System or task]

[What this enables and who should use it.]

## Prerequisites
[Access, tools, permissions, supported versions.]

## [First task]
1. [Action]
2. [Action]

Expected result:
```text
[Observed result]
````

## Configuration or reference
[Supported options, defaults, inputs, outputs, and errors.]

## Troubleshooting
[Symptom] → [Cause supported by evidence] → [Recovery and verification]
```

## Common mistakes

| Mistake | Correction |
|---|---|
| Documenting what source code suggests rather than what users can do | Verify the behavior at the authoritative boundary |
| Adding a history section “for completeness” | Remove it; create or link a separate history artifact only when requested |
| Hiding unknowns behind confident prose | Mark the gap and state how to verify it |
| Writing setup steps without a success check | Add observable expected output and a verification action |
| Mixing migration instructions into current reference material | Split the migration guide from the current-state document |

## Final check

Before publishing, ask: “Could a reader follow this today, on the stated version, without knowing how the system was developed?” If not, verify the claim, clarify the version boundary, or remove the historical detail.

Delete the draft or section and rewrite it if it contains any of these red flags in a non-migration document:

- “Recent Refactors,” “Framework Change,” “History,” “Context,” “Before v2,” “Legacy,” or “Compatibility”
- A statement that historical information is unknown, unavailable, or intentionally excluded
- Claims about an old design, prior behavior, or why an implementation changed
