# 04 — First validator

Goal of this chapter: one validator, on your genesis, voting and
finalizing. Then the harder skill: knowing exactly what that
proves and what it doesn't.

## Bring-up, in outline

1. Install the pinned Agave release (chapter 01) — the stock
   binaries.
2. Generate your keys and genesis (chapter 03), including the
   vote-authority fix if you use `solana-test-validator`.
3. Start the validator against your ledger/genesis with a start
   script you keep in the repo — Machine's is
   `start-validator.sh`, and the important property is that the
   exact command that boots the chain is public and re-runnable,
   not a shell history entry.
4. Watch the three numbers that matter: **processed, confirmed,
   finalized** slots, and vote credits. On a healthy single node
   they advance together, finalized trailing by a few dozen slots.
   Machine's localnet record: slot 1,180 and advancing, finalized
   at 1,149, vote credits growing, snapshots being produced.

## What "voting and finalizing" proves

- Your genesis is well-formed and your keys are configured
  correctly (including the chapter 03 gotcha).
- The validator executes, votes, and reaches finality under its
  own stake — the full consensus *code path* runs.
- Your operational basics work: ledger, snapshots, RPC answering
  locally.

Say exactly that, in those words, in your status docs. Machine's
docs say "local single-node testnet voting/finalizing" — that
phrasing has survived grant applications and audit inquiries
because it claims precisely what was measured.

## What it does not prove

- **Multi-validator consensus.** One node agreeing with itself is
  not a network. Leader rotation, gossip, and fork choice under
  disagreement are untested until node two exists.
- **User transfers.** This one surprises people: a chain can vote
  and finalize while ordinary signed transfers fail to confirm —
  votes are created in-process, while user transactions travel
  the RPC → TPU path, which needs working UDP (below). Machine's
  checklist lists the two-fresh-keypairs transfer as explicitly
  *not yet verified* on its dev VM.
- **Anything about your programs under load, or your bridge at
  all.** Programs are tested in program-test environments; the
  bridge is a separate system with its own gates (chapter 05).

## The environment traps

Restricted VMs and containers fail in characteristic ways. Learn
to recognize them so you debug the host, not your chain:

- **UDP send blocked.** Sandbox egress policies often block
  outbound UDP. Symptom: validator votes and finalizes, RPC
  answers, but submitted transactions never land. Machine's dev
  VM has exactly this (`EPERM` on UDP send). There is no
  validator flag that fixes a host firewall — you need a host
  where UDP works.
- **File-descriptor limit.** Agave wants a limit around
  **1,000,000** open files. A dev VM capped at **4,096** cannot be
  raised from inside (`ulimit -Hn` is a hard ceiling) and the
  validator will tell you so at startup. Believe it.
- **CPU/RAM.** A single local validator is forgiving; a full
  Agave *source build* is not (chapter 01). Don't confuse the two
  budgets.

Write a `check-host` script that tests these up front — fd limit,
UDP, disk, architecture — and fails loudly. Machine ships one
(`ops/check-host.sh`) and its testnet runbook requires it to PASS
before anything else runs. On the dev VM it fails *by design*;
that failure is the script working, and it is the difference
between "the chain is broken" and "this host can't run it," which
are very different afternoons.

## Milestone discipline

When your node finalizes its first slots, write the record down:
slot heights, vote credits, balances, snapshot slots, date,
host. Machine's GENESIS.md carries a "live verification" table
from its first run. That table is what turns "it worked once, I
think" into evidence — and it's the first thing you'll be asked
for when someone technical evaluates the project.
