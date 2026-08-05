# Local-First Proof Pipeline Prompts

Use these templates for the local-first pipeline. Manual GPT Pro handoff is the
default escalation route. Do not operate a browser or spend API credit unless
the user explicitly asks Codex to do so for the current run.

## Local Proof Prompt

```text
Use $proof-orchestrator to attempt this proof locally before preparing any GPT
Pro handoff.

Run directory:
mindflows/<YYMMDDHH-num>/

Target:
[exact theorem, lemma, disproof, or diagnosis]

Inputs:
- [source path]: [authoritative role]

Requirements:
- Check assumptions, domains, boundary cases, quantifiers, and imported theorem
  hypotheses.
- For a research-level, open-ended, or repeatedly stalled obligation, apply
  references/portfolio-search.md and maintain approach-registry.md before
  promoting one route as a proof candidate.
- Treat an expected answer as a search direction, never as a proof premise.
- Write the complete attempt to local-proof.md.
- If it succeeds, audit correctness and edit the proof for clarity and minimal
  notation before writing final.md.
- Apply references/notation-audit.md and record its required metrics in
  audit.md. Undefined symbols and symbol collisions must be zero.
- If induction is used, expose the base case, induction hypothesis, and
  induction step wherever needed for verification.
- Present every nontrivial derivation top-down: state the target, justify its
  reduction to immediate subgoals, derive each subgoal from named inputs, and
  recombine them to close the target.
- If it fails, isolate the smallest unresolved obligation. Do not call GPT Pro
  or create remote project state. Classify whether the blocker is weaker,
  comparable, equivalent, stronger, or unknown relative to the target.
```

## Portfolio Search Prompt

Use this template only for a research-level, open-ended, or repeatedly stalled
obligation. It works sequentially; use independent workers only when the user
has authorized them and the current environment makes them available.

```text
Use $proof-orchestrator to run a conclusion-neutral proof-search portfolio for
this difficult obligation. Work locally and do not call GPT Pro, control a
browser, upload files, or spend API credit.

Run directory:
mindflows/<YYMMDDHH-num>/

Frozen target contract:
[exact target, assumptions, quantifiers, object conventions, accepted outputs,
and results that do not count as completion]

Requirements:
- Read references/portfolio-search.md and create approach-registry.md.
- Begin with genuinely different mathematical mechanisms, not paraphrases of
  one favored reduction.
- Preserve early independence among authorized workers and cross-pollinate only
  after concrete results and blockers are recorded.
- Require each route to return a lemma, construction, equation, proof fragment,
  counterexample, or exact blocker.
- Classify every central blocker as weaker, comparable, equivalent, stronger,
  or unknown relative to the frozen target.
- Block theorem-strength routes without a new mechanism. Reopen one only with a
  valid novelty key.
- Run task-specific adversarial checks and small counterexample searches where
  feasible.
- Promote only an exact-target CANDIDATE to local correctness audit.
- If no route remains actionable, report the strongest rigorous result and the
  exact smallest blocker. Record Equivalent-strength blocker: YES, NO, or
  UNKNOWN in audit.md or codex-ledger.md.
```

## Continuation Prompt

```text
Use $proof-orchestrator to continue this proof project locally from the prior
run.

Prior run directory:
mindflows/<YYMMDDHH-num>/

Continuation request:
[one narrow obligation]

Requirements:
- Read the prior final.md, audit.md, local-proof.md, codex-ledger.md,
  source-manifest.md, handoff.md, and any next/redo/continuation files that
  exist. Read approach-registry.md when present.
- Create a new run directory and record the prior run ID and exact files read.
- Inherit only audited claims; treat raw GPT Pro output as evidence.
- Reuse stable local sources without overwriting the prior run.
- Inherit a blocked route only with its blocker-strength classification and a
  valid new novelty key; otherwise keep it blocked.
- Attempt the current obligation locally before preparing a new GPT Pro prompt.
- If escalation is still needed, use a new browser prompt and a fresh GPT Pro
  conversation.
```

## Manual GPT Pro Handoff Prompt

```text
Use $proof-orchestrator to prepare a manual GPT Pro handoff for the blocker in
this run. Do not call GPT Pro, control a browser, upload files, or spend API
credit.

Run directory:
mindflows/<YYMMDDHH-num>/

Required inputs:
- local-proof.md
- task.md and materials.md when present
- relevant files under sources/

Outputs:
- source-manifest.md
- browser-prompt.md
- handoff.md

Requirements:
- Ask only for the smallest unresolved proof obligation.
- If the blocker is comparable, equivalent, or stronger than the target, say so
  explicitly and ask for a concrete new mechanism rather than calling the
  missing step routine.
- Give every source a stable browser-visible filename and state whether it must
  be uploaded separately.
- Make browser-prompt.md the exact self-contained text the user can paste. Keep
  local paths and route bookkeeping out of it.
- Ask GPT Pro to label added assumptions, imported results, conjectures, and
  unsupported claims.
- Require END_GPT_PRO_OUTPUT as the final output line.
- In handoff.md, list the upload order, then the prompt-copy step, then where the
  user should return or save the output.
- Mark READY_FOR_MANUAL_GPT_PRO and wait for the user.
```

## Explicit Codex Dispatch Prompt

Use this template only after the user explicitly asks Codex to perform the
current GPT Pro call:

```text
The user has explicitly authorized Codex to dispatch this prepared GPT Pro
handoff for the current run.

Run directory:
mindflows/<YYMMDDHH-num>/

Requirements:
- Mark READY_FOR_CODEX_DISPATCH.
- Load $call-gpt-pro and confirm the selected web or API route, source-upload
  scope, and any API spending authority.
- Use browser-prompt.md as the exact model-facing prompt and source-manifest.md
  as the source contract.
- Do not silently switch routes if browser access fails.
- Save the complete answer to gpt-pro-output.md and follow the requested end
  marker and completion checks.
```

## Returned Proof Audit and Edit Prompt

```text
Use $proof-orchestrator to audit and edit the returned GPT Pro proof.

Run directory:
mindflows/<YYMMDDHH-num>/

Inputs:
- gpt-pro-output.md
- local-proof.md
- source-manifest.md and the authoritative local sources

Outputs:
- audit.md
- final.md only if the audited result is usable

Correctness pass:
- Check hidden assumptions, quantifiers, constants, boundary cases, imported
  theorem hypotheses, and whether each conclusion follows from the sources.
- Label proved, imported, conjectural, repaired, and unsupported claims.

Exposition pass after correctness:
- Lead with the conclusion and retain every non-obvious logical step.
- Present nontrivial derivations top-down: say why the target follows from the
  immediate subgoals before deriving them, state where each subgoal comes from,
  and explicitly recombine them to conclude the target.
- Make induction structure explicit where needed.
- Remove redundant or immediate steps only when no dependency is lost.
- Delete unused notation, collapse needless aliases, and simplify subscripts.
- Prefer the shortest clear proof, not the shortest-looking proof.
- Identify the theorem's core state, policy or distribution, operator,
  objective, and dependency direction before simplifying. Coordinates may
  shorten calculations but must not replace core objects in main conclusions.
- Apply references/notation-audit.md and copy its exact seven-line scorecard into
  audit.md. Do not rename or replace the metrics with an informal summary.
- Do not write READY_FOR_USER while a notation blocker remains. Fix or justify
  every warning threshold in audit.md.

If a central gap remains, mark NEEDS_GPT_PRO_REDO and prepare a focused manual
redo prompt. Do not dispatch it through Codex without new explicit user
authorization.
```

## GPT Pro Output Repair Prompt

```text
Use $proof-orchestrator to repair copy corruption in gpt-pro-output.md before
the correctness audit.

Requirements:
- Preserve proof order, labels, claims, constants, assumptions, and theorem
  status.
- Confirm END_GPT_PRO_OUTPUT when it was requested.
- Balance display-math delimiters and repair only unambiguous escaped-brace,
  operator, separator, or Markdown corruption.
- Flag ambiguous damage in audit.md instead of guessing.
- Put substantive clarity and notation edits in final.md after correctness has
  been audited.
```

## Focused Redo Prompt

```text
Prepare a manual GPT Pro redo package from the audited gap.

Inputs:
- local-proof.md
- gpt-pro-output.md
- audit.md
- source-manifest.md

Requirements:
- Ask only for the audited missing step or invalid inference.
- Preserve the original theorem and assumptions.
- Update browser-prompt.md, source-manifest.md, and handoff.md.
- Default to READY_FOR_MANUAL_GPT_PRO.
- Do not let Codex dispatch the redo unless the user explicitly asks it to do
  so for this turn.
```
