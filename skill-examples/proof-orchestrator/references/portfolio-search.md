# Proof Search Portfolio

Use this gate for research-level, open-ended, or repeatedly stalled proof
obligations. Skip it for routine arguments with a direct proof path. The gate
organizes search; it does not weaken the correctness audit or make an expected
answer an admissible premise.

## Task Contract

Freeze a conclusion-neutral contract before opening a portfolio:

- exact target, assumptions, quantifiers, and allowed sources;
- boundary cases and object conventions that a candidate must cover;
- outputs that count as completion;
- reductions, special cases, empirical checks, or unproved imports that do not
  count as completion.

If the user supplies an expected outcome or asks the search to pursue an
affirmative proof, record that as a search direction. Do not use it as a proof
assumption, suppress a valid counterexample, or relax the final audit.

## Approach Registry

Create `approach-registry.md` only when this gate applies. Group routes by their
mathematical mechanism, not by wording or worker identity. Use one entry per
family:

```text
Approach family:
Core mechanism or invariant:
Current concrete obligation:
Concrete result obtained:
Dependencies:
Smallest unresolved blocker:
Blocker strength: weaker | comparable | equivalent | stronger | unknown
Attempted falsification:
Status: EXPLORING | CANDIDATE | BLOCKED | REJECTED | REOPENED
Novelty key for reopening:
Next action:
```

The blocker-strength comparison is relative to the frozen target. A renamed
statement, global compatibility lemma, representation theorem, or construction
that would immediately settle the original target may be equivalent or
stronger even when it looks syntactically narrower.

## Portfolio Operation

1. Start with genuinely different mechanisms or formulations. Keep several
   incompatible routes alive long enough to expose their actual dependencies.
2. Allocate effort dynamically toward underexplored families and concrete
   falsification tests. Do not use a fixed worker count or let one elegant
   reduction dominate before its missing lemmas are classified.
3. When independent workers are explicitly authorized and available, preserve
   early independence: give each the frozen task contract but normally withhold
   the currently favored route. Cross-pollinate only after their concrete
   results and blockers are recorded.
4. Require concrete returns: a lemma, construction, equation, proof fragment,
   counterexample, or exact blocker. Reject vague progress reports and claims
   that an unproved compatibility step is routine.
5. Mark a route `BLOCKED` when it reaches a comparable, equivalent, stronger, or
   otherwise theorem-strength missing obligation without a new mechanism.
6. Reopen a blocked route only with a `Novelty key`: a new invariant,
   decomposition, construction, boundary analysis, falsification result, or
   independently justified theorem that directly changes the blocker.
   Rephrasing the route or assigning it to another worker is not novelty.
7. Promote a route to `CANDIDATE` only when it claims the exact frozen target
   and exposes every imported result and remaining dependency.

Use the resources actually available in the current environment. Never claim
unperformed parallel work or substitute elapsed wall-clock time for concrete
search events.

## Adversarial Search

Build a task-specific attack list from the frozen contract. At minimum check:

- every quantifier, boundary case, and degenerate object convention;
- whether each reduction preserves all hypotheses and the required conclusion;
- existence and well-definedness of every constructed object;
- hidden use of the target in a sublemma or circular equivalence;
- unproved global compatibility, compactness, convergence, or selection steps;
- whether a weaker representation is being mistaken for the required object;
- small counterexamples to each proposed intermediate lemma when feasible.

Record falsification results in the relevant registry entry. A computational
check may reject a sublemma or guide search, but it proves the target only when
the frozen contract explicitly permits that inference.

## Portfolio Review And Exit

At each portfolio review, merge duplicates by mechanism, rebalance neglected
families, and choose one of these actions for every active route:

- promote a complete candidate to the normal local correctness audit;
- continue with one concrete next obligation;
- reject it with a proof or counterexample;
- block it with an exact blocker-strength classification.

Continue only while at least one route has a concrete next action or a blocked
route has acquired a valid novelty key, subject to user and environment
budgets. If no route remains actionable, report the strongest rigorously proved
result and the exact smallest blocker. Do not invent a completion, hide an
equivalent-strength gap, or wait merely to satisfy a duration target.

For any unresolved run, record this exact line in `audit.md` or
`codex-ledger.md`:

```text
Equivalent-strength blocker: <YES|NO|UNKNOWN>
```

Explain the classification immediately after the line.
