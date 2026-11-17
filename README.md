# The Agave Fork Guide

**Fork Agave, run your own Solana-derived L1 — the guide we wish
existed when we started.**

This guide documents, step by step, how to take the stock Agave
validator, give it your own genesis, your own native token, and your
own identity, and end up with a chain that votes and finalizes —
plus the honest map of what that does and doesn't prove, and a
launch path that never puts real assets ahead of an independent
audit.

The worked example throughout is
[Machine](https://github.com/yourmememachine/mememachine): a
Solana-derived L1 built by one person on Agave 2.3.13, with
$MACHINE as native gas, a bridge back to Solana, and LaunchGuard, a
chain-native anti-rug launch standard. Every claim in this guide
about Machine can be checked in its repo — status docs, genesis
record, patches, and test results are all public.

## The chapters

| # | Chapter | What it covers |
|---|---|---|
| 01 | [Choose your base](docs/01-choose-your-base.md) | Picking the Agave version; stock prebuilt binaries vs building from source; what a fork does and doesn't make yours |
| 02 | [The three patches](docs/02-the-three-patches.md) | Client id, startup banner, snapshot interval — the small source patch set, and how to keep patches honest against upstream |
| 03 | [Genesis and keys](docs/03-genesis-and-keys.md) | Custom genesis, native supply and allocation, validator identity + vote accounts, the vote-authority gotcha, key hygiene |
| 04 | [First validator](docs/04-first-validator.md) | Single-node bring-up; what "voting and finalizing" proves and what it doesn't; the environment traps (UDP, fd limits) |
| 05 | [Staged launch](docs/05-staged-launch.md) | Localnet proof → public testnet → livenet without bridged value → audited bridge deployment; the deployment gate |
| 06 | [Making it yours](docs/06-making-it-yours.md) | Where originality actually lives: programs and standards on top; keeping the upstream rebase possible; honesty as a feature |

## What this guide is — and isn't

**It is** the stock-binaries path: run the released Agave validator
unmodified, differentiate through genesis, configuration, and your
own on-chain programs, and keep a small, verified patch set ready
for when you build from source. This is how Machine runs today, and
it is the fastest route from zero to a chain that is genuinely
yours in every way that matters at the start: its own genesis hash,
its own token, its own rules at the program layer.

**It isn't** a source-build saga, a consensus-modification manual,
or a mainnet launch checklist that ends with "ship it." Nothing in
here changes consensus rules, and the staged launch chapter will
actively slow you down at exactly one point: no real asset crosses
a bridge before an independent audit. That gate is the guide's
strongest opinion.

## Honest limits, up front

- Machine today runs a **single-validator localnet**. It votes,
  confirms, and finalizes; multi-validator consensus, signed user
  transfers, and public RPC are **not yet proven** — they need a
  host where UDP send works and the file-descriptor limit can be
  raised, which a restricted dev VM doesn't provide.
- The three source patches are verified to apply cleanly against
  Agave 2.3.13 (and one is unit-tested), but the patched build has
  **not been run end-to-end** — the running validator is the stock
  binary and identifies as `client:Agave`.
- Machine's programs (bridge, LaunchGuard) are tested code —
  190 tests across the repo — and verified SBF builds. They are
  **not deployed and not audited**, and no asset has ever crossed
  the bridge.

If a guide about forking a chain won't tell you what isn't proven
yet, it's marketing. This one will, in every chapter.

## License

Apache-2.0. See [LICENSE](LICENSE). The guide is a rewrite of
Machine's own public documentation (also Apache-2.0) into general
form; the worked-example specifics belong to that project.
