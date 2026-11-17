# 02 — The three patches

Even if you run stock binaries (chapter 01), keep a small,
verified patch set against the upstream source. It documents
exactly how your fork *would* differ at the source level, it keeps
the future source build a known quantity instead of a surprise,
and — kept deliberately tiny — it's reviewable in one sitting.

Machine's set is three patches against Agave 2.3.13. None of them
touches a consensus-critical parameter.

## The patches

### 01 — Client id

Upstream Agave identifies itself in `--version` output and in
gossip as `client:Agave`. The patch adds a new client id —
Machine's is **4**, named `Machine` — and makes it the default, so
a binary built from the patched source reports `client:Machine`.

This is an identity label. It changes no behavior, which is
precisely why it's patch #1: it's the patch you can fully verify
(including compiling and unit-testing the touched crate) on modest
hardware, and it proves your whole patch pipeline works.

### 02 — Snapshot interval

The default incremental snapshot interval goes from **100 to 500
slots**. An operational default, chosen for a young chain's disk
and restart profile. One constant, one line, no logic.

### 03 — Startup banner

One log line at validator startup:

```
Chain: Machine (Agave 2.3.13 fork)
```

When you are staring at logs at 2 a.m., knowing *which* chain a
binary thinks it is — in the first lines, not inferred from ports —
is worth one line of diff.

## What's deliberately not patched

Ticks per slot, slots per epoch, vote and shred rules, the fee
schedule, rent, bank-hash computation, the full-snapshot interval.
The discipline is the point: **a patch you can't explain in one
sentence doesn't go in.** Every entry in the set should be
classifiable as identity, operations, or cosmetics — never
consensus.

## Patch discipline against upstream

- **One directory, one patch per file, numbered.** Machine keeps
  them in `patches/` as unified diffs against the tagged upstream
  release, with a README recording what was verified and how.
- **Verify application, byte for byte.** For each patch:
  `patch --dry-run` and `git apply --check` against a pristine
  source tree, then a real apply in a scratch tree, then a diff of
  the result against the intended patched file. Machine's set
  passed all of this against the checksum-verified v2.3.13
  tarball; the client-id patch additionally compiled and passed
  its crate's unit tests (2 passed).
- **Record what you did *not* verify.** Machine's docs state
  plainly: the full Agave release build was not run, and no
  patched binary has been run. Patches 02 and 03 are verified by
  clean application plus a syntax parse, not by compiling their
  crates. A patch set with an honest verification ledger is worth
  more than one with inflated claims — the first auditor question
  is always "how do you know?"
- **Keep the build scripted.** A `build-from-source.sh` that
  downloads the source, verifies the checksum, applies the
  patches, builds, and stages binaries in a *separate* directory
  (never overwriting the stock `bin/`) turns "someday we'll build
  it" into one command on a big enough machine. Swapping a patched
  binary in remains a separate, deliberate operator step.
- **On upgrade, re-derive, don't re-apply blindly.** When you move
  to the next Agave release, regenerate each patch against the new
  source and re-run the verification. A patch that no longer
  applies cleanly is upstream telling you the code moved — read
  why before forcing it.
