# 06 — Making it yours

A fork is an afternoon. Chapters 01–05 get you a running chain
that is technically yours. This chapter is about the part that
makes it *actually* yours — because "we forked Agave" describes
dozens of forgotten repos, and the difference is never the fork.

## Originality lives at the program layer

Consensus is a solved, battle-tested commodity — that's why you
forked it instead of writing it. Your chain's reason to exist
has to be built **on top**: programs and standards that use the
chain's sovereignty for something a program on someone else's
chain cannot do.

Machine's example is **LaunchGuard**, a launch standard with a
simple claim: a launch registered with it *cannot* be rugged in
the usual ways, because the guarantee is enforced by the chain's
own program, not by a team's promise. Registration requires the
mint and freeze authorities **revoked**; the creator locks **≥10%
of supply for ≥30 days** (slot-based: 6,480,000 slots) in a
program-PDA vault; locks can only be extended, never shortened;
there is no admin key. A creator-reputation account accrues only
by completing locks. The standard is small (one program), fully
tested, and — this matters — it's the piece of Machine that is
not a fork of anything.

Your version of this will differ. The shape to copy:

- **One original mechanism**, explainable in two sentences, that
  needs to be a chain-level rule rather than an app.
- **Small enough to audit.** A standard an auditor can read in a
  day gets audited; a platform doesn't.
- **Proven in public at zero value first** — a demo launch on
  localnet/testnet where the rug attempts are executed and fail
  on the record. Machine's demo launch includes four failed rug
  attempts (mint more → fixed-supply error; early release →
  still-locked error; shorten the lock → rejected; stranger
  release → not-the-creator error), each with the real program
  error. That transcript is worth more than any pitch deck.

The bridge teaches the complementary lesson: where you *must*
build something dangerous (asset movement), build it from the
post-mortems of everyone who got it wrong before you —
attestation verification, nonce replay protection, absolute *and*
dynamic caps, automatic tightening after an unpause, a receipt
for every crossing — and gate it behind an audit (chapter 05).

## Keep the upstream rebase possible

Every change you make to the fork is a tax on every future
upstream upgrade, and upstream security fixes are the main reason
to be on Agave at all. So:

- Keep the source patch set tiny and non-consensus (chapter 02).
  Three patches rebase in an hour; three hundred are a life
  sentence.
- Put your originality in programs and tooling, which upstream
  releases never touch.
- Track upstream releases. When a new Agave version ships, your
  first move is evaluating the upgrade, not your roadmap —
  a fork that can't take upstream security patches is a fork
  with an expiry date.

## Honesty is a feature, not a disclaimer

Machine's repo keeps a limitations page that says, in plain
language: not audited, not deployed, localnet only, these proofs
not yet run. Its status docs are the single source of truth, and
marketing claims are not allowed to outrun them.

This reads like humility. It's strategy:

- **Reviewers can verify you.** Every claim in the docs is
  checkable in the repo — test counts, hashes, genesis record.
  People who build on chains *check*; a project that survives
  checking gets recommended by the checkers.
- **Auditors and grant programs price honesty.** An audit firm
  scoping from accurate docs quotes the real work. A grant
  reviewer who finds your stated limitations *match* the code
  trusts the next claim too.
- **It compounds.** Six months of status docs that never
  overclaimed is a reputation no launch announcement can buy.

The fork made your chain possible. The programs make it
interesting. The honesty makes it believable. Build all three,
in that order of effort — which is to say, in reverse order of
where most forks stop.
