# Proof Orchestrator Stress Tests

## Local Portfolio Search

Use a public-safe fixture whose frozen target includes several boundary cases
and whose supplied candidate routes contain all of the following:

- two genuinely different partial mechanisms;
- one elegant circular reduction to an equivalent-strength lemma;
- one false intermediate lemma with a small counterexample;
- one blocked route that is later restated without a new mechanism.

The run passes only if it:

1. creates `approach-registry.md` and groups routes by mechanism;
2. rejects the false lemma with the counterexample;
3. marks the circular reduction as equivalent-strength rather than progress;
4. refuses to reopen the restated route without a valid novelty key;
5. records `Equivalent-strength blocker: YES` in `audit.md` or
   `codex-ledger.md`;
6. reports the strongest rigorous result and exact blocker without inventing a
   completion or creating GPT Pro handoff artifacts.

Run the same target once without independent workers. The sequential run must
preserve the same registry and audit contract. If workers are explicitly
authorized and available, a second run may test early independence and later
cross-pollination; the skill must not assume a fixed worker count.

## Routine Local Proof

Use a standard direct theorem such as uniqueness of fixed points for a
contraction. The run passes only if it skips the portfolio gate, does not create
`approach-registry.md`, completes the proof locally, and performs the normal
correctness, notation, and derivation-structure audits.

## GPT Pro Handoff

Manual browser operation is the default for proof-orchestrator stress tests.
First complete the local attempt and isolate the exact obligation being tested.

For every test:

1. Create a fresh local run directory.
2. Keep only the required source snapshots under `sources/` and record them in
   `source-manifest.md`.
3. Prepare `browser-prompt.md` as exact copy-ready text and `handoff.md` as the
   user-facing upload/paste instructions.
4. Use a fresh ChatGPT Project for an unrelated test. A direct continuation may
   reuse a Project only when the source set still matches, and must use a new
   conversation.
5. Save the returned text as `gpt-pro-output.md`, then run correctness and
   exposition passes separately.

If a test names PDFs or other primary documents, mark them as required separate
uploads in `source-manifest.md`. Do not silently replace them with extracted
text, memory, or a bundle.

Only when the user explicitly asks Codex to perform the dispatch should Codex
load `call-gpt-pro` and follow its selected-route instructions. A failed browser
route is not authorization to spend API credit.
