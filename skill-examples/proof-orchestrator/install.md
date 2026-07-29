# Installation

## Copy

- Source path: `skill-examples/proof-orchestrator/`
- Installed name: `proof-orchestrator`
- Destination: `$CODEX_HOME/skills/proof-orchestrator`, or
  `~/.codex/skills/proof-orchestrator` when `CODEX_HOME` is unset.

## Dependencies

- Required companion skills: none for the default local-proof and manual
  handoff route.
- Optional companion skill: `call-gpt-pro`, only when the user explicitly asks
  Codex to perform the GPT Pro dispatch.
- Optional reviewers: `proof-checker-v2` and `deepseek-agent`; they do not
  replace Codex's local correctness and exposition passes.
- External CLIs, Python packages, and connectors: none for the default route.
- Credentials or accounts: none for the default route. Web access or API
  credentials may be required by the explicitly selected `call-gpt-pro` route.

## Install Steps

1. Copy this directory into the active Codex skills root.
2. Confirm the installed directory contains `SKILL.md`,
   `agents/openai.yaml`, `references/dispatch-prompts.md`, and
   `references/notation-audit.md`.
3. Install optional companion skills only for the routes the user intends to
   use.
4. Restart Codex so the skill registry can reload.

## Update Steps

1. Back up the existing installed directory.
2. Replace it with the current source directory, or run `skill-installer` with
   `--update` when installing from a GitHub source.
3. Run the verification steps below.
4. Keep the backup until both the local-proof and manual-handoff smoke tests
   pass without opening a browser or spending API credit.

## Verification

From the MetaSkill repository root, run:

```powershell
python .\scripts\validate_muxing_install.py --source-only --skill proof-orchestrator
```

After installing into the active skills root, run:

```powershell
python .\scripts\validate_muxing_install.py --installed-only --skill proof-orchestrator
```

Local-proof smoke test:

```text
Use $proof-orchestrator to prove uniqueness of fixed points for a contraction
mapping. Work locally first. Audit the proof and simplify its exposition.
```

The run should attempt and finish the standard proof locally, explicitly check
the contraction argument, avoid creating a GPT Pro handoff, report zero
notation blockers, organize the nontrivial implication from the uniqueness
target down to the contraction subgoal, and produce a concise verified final
artifact.

Manual-handoff smoke test:

```text
Use $proof-orchestrator on this deliberately incomplete research lemma. If the
local attempt cannot close the stated gap, maintain the supplied sources and
prepare the exact prompt I can paste into GPT Pro. Do not call GPT Pro for me.
```

The run should isolate the local blocker, create `source-manifest.md`,
`browser-prompt.md`, and `handoff.md`, reach `READY_FOR_MANUAL_GPT_PRO`, and
perform no browser or API action.

## Rollback

1. Remove the failed installed directory.
2. Restore the backup created during update.
3. Restart Codex.

## Notes

- Invoking `proof-orchestrator` authorizes local proof work and run-local file
  maintenance, not browser control, source uploads, or API spending.
- The default escalation route is a user-operated browser handoff with stable
  local sources and a copy-ready prompt.
- After any GPT Pro answer returns, Codex audits correctness first and then
  edits for clarity, target-first top-down derivations, explicit induction
  structure where needed, and minimal notation without hiding non-obvious
  steps.
- The notation audit treats undefined symbols and same-glyph/different-meaning
  collisions as blockers and uses `references/notation-audit.md` for measurable
  warning thresholds.
