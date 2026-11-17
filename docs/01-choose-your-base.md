# 01 — Choose your base

## Pick a released version, and pin it

Start from a **tagged Agave release**, not from `master`. Machine
uses **Agave 2.3.13** (upstream commit
`5466f4592b1983adb13ba0a5d53f41ea2de69fba`), and every document in
its repo that names the version names the same one. Pin three
things and never let them drift apart:

- the release version (2.3.13),
- the upstream source commit it was built from,
- the checksum of the source tarball, if you download source for
  patching (Machine's patch set records the SHA-256 of the v2.3.13
  tarball and verifies it on every build attempt).

Why so fussy: six months from now, "we forked Agave" will be
unanswerable unless the repo says *which* Agave, byte for byte.
Auditors, grant reviewers, and future-you all ask the same question.

## Stock prebuilt binaries vs building from source

There are two ways to get a validator:

| | Prebuilt release binaries | Build from source |
|---|---|---|
| Time to a running chain | An afternoon | Days, the first time |
| Machine needed | Modest VPS-class | Agave is a huge Rust workspace: many cores, lots of RAM, the exact pinned Rust toolchain |
| Your source changes | None — impossible | Anything, for better and worse |
| What identifies your chain | Genesis, keys, config | Same, plus your patches |
| Upgrade path | Download the next release | Rebase your patches onto it |

**Start with the prebuilt binaries.** Machine still runs them. The
reasons are not laziness:

1. **Consensus is not where your originality should live** (chapter
   06). The validator's job is to be boring and correct; Anza's
   release builds are exactly that, and your genesis is what makes
   the chain yours on day one.
2. **A full Agave build is a project of its own.** Machine's dev VM
   (1.5 shared CPUs, ~8 GB RAM, and not the toolchain version the
   source pins) could not run it — the patched build remains
   honestly listed as *not yet run*. If your hardware can't build
   it, a half-built fork is worse than a stock binary you
   understand.
3. **Every claim stays checkable.** "Stock 2.3.13, measured
   `--version` output in the repo" is a statement anyone can
   verify. It buys credibility that a fork with unclear provenance
   never gets.

Build from source when you have a reason a patch must *run* — not
before — and keep the patch set tiny and verified in the meantime
(chapter 02).

### The ARM caveat

Agave's prebuilt release binaries are x86-64. Several attractive
cheap hosts are ARM (notably the always-free ARM tiers from the big
clouds). On ARM, the prebuilt path doesn't exist: you're building
Agave from source, with everything that implies, or you're not
running. Check your host's architecture *before* falling in love
with its price — this one detail has stalled more than one launch
plan, including Machine's first hosting attempt.

## What a fork does and doesn't make yours

Forking Agave gives you:

- **Your own chain identity** — genesis hash, cluster, shred
  version. Machine's genesis hash is
  `32W8ouqrHbuhE3PM6ZsJrJcDbP7Tx9aC2zRvrEabkQSb`; nothing else in
  the world has it.
- **Your own native asset** — supply, allocation, and fee schedule
  set at genesis (chapter 03).
- **Sovereignty at the program layer** — your programs, your
  standards, your deployment gates.

It does not give you:

- **Different consensus.** Machine runs stock Agave consensus,
  voting, and fee rules, and says so. Changing those is a research
  project with a multi-validator testbed as the entry fee.
- **Solana's network.** Your fork cannot transact with Solana
  mainnet; assets move between the two only through a bridge —
  which is a separate, security-critical program suite, not a
  feature of the fork.
- **Instant credibility.** A fork is an afternoon of setup. What
  makes it a project is everything after: programs, tests, docs,
  and the discipline in chapters 05 and 06.

Choose the base with that clear-eyed view and the rest of the
guide is straightforward.
