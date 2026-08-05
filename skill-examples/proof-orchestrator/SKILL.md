---
name: proof-orchestrator
description: "Coordinate local-first proof runs: attempt and refine proofs with Codex, audit correctness and notation, maintain run-local sources, prepare copy-ready manual GPT Pro handoffs when local proof stalls."
---

# Proof Orchestrator

## Role

Run proof work as a local-first pipeline. Codex first attempts the proof, checks its correctness, and edits it for clarity and economy. Escalate the remaining hard obligation to GPT Pro.

Default escalation is manual: maintain the sources locally and give the user an exact browser-ready prompt. Invoking this skill does not authorize Codex to operate a browser, upload files, or spend API credit. Use `call-gpt-pro` only when the user explicitly asks Codex to perform the GPT Pro call for the current run.

## Run Directory

Keep each run under:

```text
mindflows/<YYMMDDHH-num>/
```

Use only the files needed by the run:

```text
task.md              # precise theorem or proof obligation
materials.md         # definitions, givens, notation, and source excerpts
local-proof.md       # Codex's proof attempt or isolated blocker
approach-registry.md # optional route portfolio for difficult proof searches
sources/             # stable local source snapshots
source-manifest.md   # source role, browser-visible name, and upload status
browser-prompt.md    # exact text the user can paste into GPT Pro
handoff.md           # manual/automated route, upload order, and status
gpt-pro-output.md    # returned GPT Pro answer, kept as raw evidence
audit.md             # correctness and source-alignment audit
final.md             # verified, simplified, user-facing proof
codex-ledger.md      # run state and provenance, optional
next.md              # next narrow obligation, optional
```

Do not create `browser-prompt.md`, `handoff.md`, or remote project state before the local attempt unless the user explicitly skips local proof or asks for a handoff package.

## Continuing a Project

Treat an existing run, `next*.md`, `redo*.md`, or continuation artifact as a project continuation. First read the prior `final.md`, `audit.md`, `local-proof.md`, `codex-ledger.md`, `source-manifest.md`, `handoff.md`, `approach-registry.md` when present, and any next/redo/continuation files that exist. Use `gpt-pro-output.md` only as raw evidence unless its audit accepts the relevant claims.

Always create a new run directory for new proof work. Record the prior run ID, the exact files read, inherited proved/conjectural/rejected claims, preserved sources, and the single current obligation. Treat completed run artifacts and prior GPT Pro conversations as append-only evidence; do not overwrite them.

When inheriting an approach registry, preserve family identities and blocker-strength classifications. Keep a blocked route blocked unless the new run records a valid novelty key under `references/portfolio-search.md`.

If a continuation reaches manual GPT Pro escalation, prepare a new `browser-prompt.md`. The user may reuse a matching ChatGPT Project, but the prompt should go into a fresh conversation so old context does not silently alter the task.

## Status Labels

Use these labels in `codex-ledger.md`, `audit.md`, or `handoff.md`:

- `LOCAL_ATTEMPT`
- `LOCAL_PROVED`
- `LOCAL_BLOCKED`
- `ASK_USER`
- `READY_FOR_MANUAL_GPT_PRO`
- `WAITING_FOR_USER_GPT_PRO_OUTPUT`
- `READY_FOR_CODEX_DISPATCH`
- `WAITING_FOR_GPT_PRO_OUTPUT`
- `NEEDS_GPT_PRO_REDO`
- `AUDIT_FAILED`
- `READY_FOR_USER`

## Notation Gate

When the user asks about notation or symbols, when the proof is theorem-heavy, or when one proof step contains at least five nonstandard symbols, read `references/notation-audit.md` and include this exact scorecard in `audit.md` or the user-facing audit:

```text
Core semantic objects retained: <retained>/<declared> (<percent>)
Undefined symbols: <count>
Symbol collisions: <count>
One-use definitions: <count>/<all new symbols> (<percent>)
Maximum parallel representations of one object: <count>
Maximum alias-chain depth: <count>
Maximum active nonstandard symbols in one proof step: <count>
```

Do not rename, merge, omit, or replace these lines with other useful findings. Report logical gaps, domain errors, and irrelevant notation after the fixed scorecard. Core-object retention must be 100%, and undefined symbols and collisions must both be zero before `READY_FOR_USER`.

Never improve the scorecard by inventing a definition, domain, assumption, identity, or relation that the source does not supply. If an undefined symbol or missing implication cannot be resolved from authoritative material, keep it in the audit, mark the proof `AUDIT_FAILED` or `ASK_USER`, and rewrite only the valid fragment or the diagnosis.

## Derivation Structure Gate

For every nontrivial derivation, organize the user-facing proof from the target downward, even if the proof was discovered bottom-up:

1. State the target and its role: "To prove A, it is enough to establish B, C, and D," together with the lemma, identity, or inference that makes those subgoals sufficient.
2. Derive each immediate subgoal and state where it comes from: an assumption, definition, prior lemma, or an explicitly shown calculation.
3. If a subgoal has its own dependencies, expand it in the same target-first form. Order dependent subgoals by their true dependency relation rather than presenting a misleading flat list.
4. Recombine the established subgoals and explicitly return to the original target.

This is an exposition rule, not a license to reverse an implication or hide a gap. Check that the dependency graph is acyclic, every reduction is justified, and no subgoal silently assumes the target. Do not force this scaffold onto a one-step argument where it would add more ceremony than clarity.

Record `Top-down derivation structure: PASS`, `FAIL`, or `NOT_APPLICABLE` in `audit.md`. A nontrivial derivation cannot be `READY_FOR_USER` while this gate is `FAIL`.

## Search Portfolio Gate

For a research-level, open-ended, or repeatedly stalled obligation, read
`references/portfolio-search.md` before committing to one proof route. Create
`approach-registry.md`, compare routes by their mathematical mechanism, classify
the strength of every central blocker, and run task-specific adversarial checks.
For a routine proof with a direct path, skip this gate and do not create the
registry.

An expected answer is a search direction, not a proof premise. Do not assume a
theorem is true merely because the user, benchmark, or prior model output says a
proof exists. Use independent workers only when they are explicitly authorized
and available; the gate must also work as a sequential local search.

## Workflow

Default route: freeze target -> maintain evidence -> open the search portfolio when needed -> local proof -> local correctness audit -> exposition edit -> final. If local proof stalls: maintain sources -> prepare a copy-ready manual GPT Pro handoff -> ingest returned text -> correctness audit -> exposition edit -> final.

1. Freeze the target.
   - Decide whether the request is new or a continuation.
   - State the exact theorem, assumptions, quantifiers, and allowed sources.
   - Do not broaden or repair the theorem silently.
2. Maintain local evidence.
   - Read only the files needed to understand the target.
   - Copy stable, directly relevant snapshots into `sources/` when the original may change or cannot be referred to reliably.
   - Keep private run materials in the run directory, never in the skill package.
3. Attempt the proof locally.
   - Apply the Search Portfolio Gate before committing to one route when the obligation is research-level, open-ended, or repeatedly stalled.
   - If the gate applies, maintain `approach-registry.md` and promote only an exact-target `CANDIDATE` to the normal proof audit.
   - Try to complete the actual proof, disproof, counterexample, or diagnosis; do not stop at a difficulty probe.
   - Check definitions, boundary cases, domains, support, topology, quantifiers, and imported theorem hypotheses.
   - Write `local-proof.md` with the conclusion, proof attempt, dependencies, and any unresolved gap.
   - If successful, mark `LOCAL_PROVED` and continue to local audit and editing.
   - If unsuccessful, mark `LOCAL_BLOCKED`, isolate the smallest hard obligation, and only then prepare the GPT Pro package.
4. Audit correctness locally.
   - Verify every theorem, lemma, reduction, equality, bound, constant, and quantifier against the stated assumptions and local sources.
   - Distinguish proved, imported, conjectural, repaired, and unsupported statements.
   - For an unresolved run, record `Equivalent-strength blocker: YES`, `NO`, or `UNKNOWN` in `audit.md` or `codex-ledger.md` and justify the classification.
   - Treat optional external or DeepSeek review as additional evidence, not a substitute for Codex's own audit, and do not trigger a paid or remote reviewer without authorization.
5. Edit the proof for exposition.
   - Always read `references/notation-audit.md` when the user asks about notation or symbols, when the output is theorem-heavy, or when one proof step contains at least five nonstandard symbols.
   - Lead with the conclusion and expose the main logical structure.
   - Apply the Derivation Structure Gate: state the target first, reduce it to sufficient immediate subgoals, explain the source of each subgoal, and recombine them to close the target.
   - Before deleting notation, identify the theorem's semantic center: its state variable, policy or distribution, operator, objective, and dependency direction. Preserve these objects in every main result.
   - Keep enough intermediate reasoning that a reader can verify every non-obvious transition.
   - For induction, state the base case, induction hypothesis, and induction step wherever omitting one would hide the argument.
   - Remove redundant or genuinely immediate steps only after confirming that no logical dependency is lost.
   - Simplify notation: delete unused symbols, avoid multiple names for the same object, shorten unnecessary subscripts, and introduce notation only when it reduces total complexity.
   - Use coordinates and abbreviations to compute with a core object, never to replace it. Map every coordinate-level conclusion back to the original theorem interface.
   - Copy the exact seven-line scorecard from `references/notation-audit.md` into `audit.md`; do not rename, merge, or replace its metrics with an informal summary.
   - Do not mark `READY_FOR_USER` unless core-object retention is 100% and no symbol is undefined or reused with a different meaning. Fix or explicitly justify all threshold warnings.
   - Prefer a short direct argument over repeated summaries or decorative formalism. Never polish an unresolved gap into an apparently complete proof.
6. Prepare manual GPT Pro escalation when needed.
   - Narrow the request to the blocker exposed by `local-proof.md`.
   - Complete the source-maintenance contract below.
   - Write `browser-prompt.md` as the exact text the user can copy and paste.
   - Write `handoff.md` with source upload order and simple return instructions.
   - Mark `READY_FOR_MANUAL_GPT_PRO`, present the package, and wait for the user to return the answer.
7. Dispatch only with explicit authorization.
   - A request such as "use GPT Pro" does not by itself authorize Codex to operate the browser or spend API credit; keep the manual route.
   - Switch to Codex execution only when the user explicitly asks Codex to call or operate GPT Pro for this run.
   - Then mark `READY_FOR_CODEX_DISPATCH`, load `call-gpt-pro`, confirm the selected web/API route and any spending or upload authority, and follow that skill's completion protocol.
   - Do not reuse authorization from a prior run or infer an API fallback after a browser failure.
8. Ingest, audit, and edit the returned answer.
   - Save user-pasted or Codex-retrieved text as `gpt-pro-output.md`.
   - Apply only the formatting repairs allowed below before auditing.
   - Audit correctness and source alignment before using any claim.
   - Then perform the full exposition edit from step 5; `final.md` may be much clearer and shorter than the raw answer while preserving all necessary logic and epistemic labels.
   - If a central gap remains, mark `NEEDS_GPT_PRO_REDO` and prepare a focused manual redo prompt first. Dispatch the redo through Codex only after new explicit authorization.

## Manual Handoff Contract

For a manual GPT Pro handoff:

1. Keep authoritative copies under `sources/` with stable generic filenames.
2. Write `source-manifest.md` with, for each source:
   - local relative path;
   - browser-visible filename;
   - why it is needed;
   - whether it must be uploaded separately or is summarized in `materials.md`;
   - current status: `ready`, `missing`, `optional`, or `returned-by-user`.
3. Make `browser-prompt.md` self-contained with the exact target, assumptions, definitions, requested output, and source filenames GPT Pro will see. Do not include local absolute paths, route bookkeeping, or instructions meant only for Codex.
4. End the requested output contract with a distinctive marker such as `END_GPT_PRO_OUTPUT` so copied output can be checked for completeness.
5. Make `handoff.md` tell the user, in order, which files to upload, which text to paste, and where to paste the returned answer locally. Do not require browser automation.

If a required source is missing, mark the handoff blocked rather than silently replacing it with memory. Keep the prompt narrow: ask for one lemma, counterexample, assumption check, or proof obligation whenever the local audit has isolated one.

## GPT Pro Output Repair

Keep `gpt-pro-output.md` recognizable as raw GPT Pro evidence. Formatting repair may fix copy corruption but must not change claims, constants, assumptions, theorem status, or proof order.

Required checks:

- Confirm the requested completion marker is present.
- Balance display-math delimiters and inspect suspicious blank lines.
- Repair obvious escaped-brace corruption such as `\left{` to `\left\{` and `\right}` to `\right\}` only when the intended delimiter is unambiguous.
- Remove residual web-copy separators only when their intended role is clear; otherwise flag them in `audit.md`.
- Scan for malformed operators, stray Markdown markers, and broken right delimiters.

Record nontrivial repairs in `audit.md` or `codex-ledger.md`. Perform substantive clarity and notation editing in `final.md`, after the correctness audit, rather than rewriting the raw output.

## Guardrails

- Prefer a complete local proof over escalation, but label uncertainty honestly.
- Never invent missing citations, source statements, assumptions, or proof steps to avoid escalation.
- Never treat invoking this skill as authority for browser control, uploads, API spending, or a second GPT Pro turn.
- Do not ask GPT Pro for a full theorem when the local attempt has isolated a smaller blocker.
- Do not present a comparable, equivalent, or stronger missing lemma as routine progress. Reopen a blocked route only when `references/portfolio-search.md` supplies a valid novelty key.
- Audit before simplifying. Preserve any step whose removal would make a non-obvious inference unverifiable.
- Treat undefined symbols and same-glyph/different-meaning collisions as correctness blockers, not cosmetic issues. Apply the thresholds in `references/notation-audit.md` before finalization.
- Treat loss of a theorem's core state, policy, distribution, operator, objective, or dependency direction as a notation blocker even when the rewritten coordinate formulas are shorter and locally correct.
- Treat an unjustified target-to-subgoal reduction, a circular dependency, or a derivation that never returns to its stated target as an exposition blocker.
- If correctness and elegance conflict, preserve correctness and state the remaining exposition issue explicitly.
