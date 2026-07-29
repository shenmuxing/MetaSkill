# GPT Pro Stress Tests

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
