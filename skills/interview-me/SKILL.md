---
name: interview-me
description: "Use when the user says /interview-me, or when you must elicit their decisions, check your own earlier output against what they actually want, or settle open questions before writing rules, ADRs, PRs or designs. Structured neutral-question interview using the ask tool; not for questions you can answer from code, docs or tools."
---

# Interview me

Elicit the user's real decisions through neutral questions, then validate or invalidate your earlier output against the answers. The user finds this the best way to get their thinking across, so run it as a deliberate loop, not as a few clarifying questions.

## When to use

- The user invokes `/interview-me`.
- You produced a rule, ADR, PR text, design or plan and are unsure it matches what they want.
- Several viable choices exist and the choice is theirs: policy, scope, wording that binds agents, user-facing behavior.
- A previous draft may contain claims, reasons or wording the user never gave.

Do not use it for facts you can get from code, config, docs, git or tools. Look those up first. Never ask the user for information the repo or tools provide.

## The loop

1. **Inventory.** List every claim in the output under review. Mark each as *the user said it*, *a tool showed it*, or *I assumed it*. Only the third kind needs a question.
2. **Collect open questions.** One decision per question. Include questions that could invalidate things you already wrote, not only gaps. If a prior draft used your own rationale, ask whether it is right instead of defending it.
3. **Ask in one batch** with the `ask` tool. Ask only the genuinely open questions: one is fine, and a batch is usually at most 7. Never pad to reach a number, and split into a second round rather than exceed the cap. Order from most consequential to least.
4. **Fold the answers in.** The user's free-text notes on an answer outrank the option label, and often add the real rule (limits, tone, priorities). Apply them in full.
5. **Restate what changed.** Say which earlier claims the answers confirmed, which they invalidated, and what is still open. Then fix the output.
6. **Repeat** only for what is still open. Stop when nothing is open. Do not interview for its own sake.

## Timed-out answers are not answers

The `ask` tool can time out and auto-select an option, reported as "(auto-selected after timeout)". These are NOT the user's decisions.

- Never fold an auto-selected answer into the design or treat it as settled.
- Keep every answer the user actually gave, including their free-text notes.
- Re-ask only the timed-out questions, with the same wording and options, so the user can finish.
- If the user says they didn't get to answer everything, re-ask what they request (all questions if they ask for a full reset), and drop the earlier auto-selections.
- Large batches time out more often. Prefer smaller batches when questions need thought (scenario questions, corner cases).

## Writing the questions

- **Neutral.** Options are balanced and none is pre-argued. Do not word an option so the "right" one is obvious. Set `recommended` only when the user asked for a recommendation or the convention clearly settles it.
- **Concrete scenario first.** Anchor the question in a specific situation ("an agent is partway through a command when I stop it") rather than an abstraction. Real data from the user's own history works well.
- **Always cover the escape hatches.** Include a "depends" or "something else" style option where honest. The UI adds "Other".
- **Multi-select when options are not exclusive.** Set `multi: true` whenever more than one answer can be true (kinds of example, which items to keep). A single-choice question forced the user to type a note explaining that; avoid that.
- **Short option labels**, tradeoffs in `description`.
- **No leading premise.** Do not smuggle a conclusion into the question text.
- **Name things completely.** Always give a PR or issue's title with its number. Never rely on the user remembering numbers.
- **Disambiguate with examples.** When a phrase like "ordered by date" has several readings, show each reading as a concrete example list rather than asking in the abstract.

## Validating your own output

- Check each rationale, "rejected because" line and consequence against what the user actually said. If you wrote the reason, say so, and offer to change it.
- Do not present your inference as their reasoning. Do not write "you answered..." in a deliverable.
- Anything the answers leave open stays open and is listed as open. Do not fill it with a plausible rule.
- Prefer deleting an unsupported claim to softening it.
- If you used a simulation or demo to get acceptance, the written rule must match what the simulation actually did. Re-check before writing it down.

## Keeping the interview out of the deliverable

The interview is the process; the artifact records the outcome.

- ADRs, rules and PR descriptions state the decision and its standalone rationale. No transcript, no journal of how it was reached, no retrospective. An ADR may carry examples (synthetic or from real cases, framed as applications of the rule), but the rule body must stand without them.
- A PR description states why and what changes, not the history.
- If the history is worth keeping, it goes in a separate lessons note, only as confirmed facts.

## After the answers

- Approval to draft is not approval to publish. Write drafts where the user can review them (a local file, or a draft PR), say what is and is not final, and wait for their go before merging or filing.
- When the output binds other agents or changes a user-facing interface, follow the user's design-approval rules before implementing.
- Never pass off a decision the user did not make as made. Record who decided and when only for decisions they actually made.

## Failure modes

| Mistake | Instead |
|---|---|
| Asking what a tool could tell you | Look it up, then ask only the residue |
| Padding a batch to reach a number | Ask only what is genuinely open |
| One giant open question | Split into single-decision questions |
| Treating the note on an answer as decoration | Apply it as the rule |
| Restating your inference as their answer | Mark it as yours and ask |
| Treating an auto-selected (timed-out) option as a decision | Re-ask it, keeping the real answers |
| Asking again after the user said it is settled | Proceed |
