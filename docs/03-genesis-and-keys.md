# 03 — Genesis and keys

Genesis is where the fork becomes a chain. Everything in this
chapter is configuration, not source — and it is most of what
makes the chain *yours*.

## What genesis decides

- **Native token and supply.** Machine's $MACHINE: 1,000,000,000
  total, fixed, **no inflation** (0.0 initial / terminal / taper).
  Fixed-supply is a one-line genesis choice and a permanent
  property — choose it like one.
- **Allocation.** Machine's genesis: the founder identity holds
  998,999,000; a stake account holds 1,000,000 staked to the vote
  account; a faucet holds 1,000 for test airdrops; ~0.25 MACHINE
  across system accounts is rent-exempt minimums, inherent to
  every Solana genesis. Publish your allocation table in the repo.
  "Who holds what at block 0" is the first thing anyone evaluating
  your chain will look for, and finding it in your docs — with
  addresses — beats them finding it in the ledger.
- **Fee schedule.** The FeeRateGovernor parameters (lamports per
  signature: current / target / minimum / maximum, target
  signatures per slot, fee and rent burn percentages) are genesis
  configuration. Machine's are published in its GENESIS.md;
  whatever you choose, publish yours the same way.
- **Timing.** Ticks per slot, slots per epoch, warmup epochs.
  Development clusters can tick fast (`sleep` instead of hashing);
  say so if yours does.

Record the resulting **genesis hash** everywhere — repo README,
docs, announcements. It is your chain's fingerprint; two networks
with the same name and different hashes are different networks,
and the hash is how anyone tells.

## Validator keys: identity, vote, stake

A bootstrap validator needs three keypairs with distinct jobs:

| Key | Job |
|---|---|
| **Identity** | The node's identity; also typically the fee payer and (initially) the fund holder |
| **Vote** | The vote account's keypair; votes are signed under its authority configuration |
| **Stake** | The stake account delegating to the vote account |

Generate them with `solana-keygen`, record the *public* keys in
your genesis doc, and keep the keypair files out of any public
repo the moment they guard anything real (see hygiene below).

### The vote-authority gotcha (read this before your first run)

`solana-genesis` sets a bootstrap vote account's **authorized
voter to the identity keypair**. `agave-validator` votes as the
identity, so it matches. But `solana-test-validator` votes as the
**vote keypair** — so on a stock genesis it can *never vote*, and
your chain never confirms a slot, with no error that points at the
cause.

Machine hit exactly this. The fix is a genesis-level patch:
rewrite the authorized-voter entry in the vote account's state
(from the identity pubkey to the vote pubkey), leaving
`node_pubkey` and the withdraw authority untouched. Machine's
repo ships the patch script, which verifies the account's
version, node, withdrawer, commission, and current voter before
writing and aborts if any check fails. The patch changes the
genesis hash — publish the **post-patch** hash as canonical, and
record the pre-patch one so the history is auditable.

Whichever route you take — patch, or generate genesis so the
voter is right the first time — learn from the shape of the bug:
*the chain silently not voting* is a key-configuration symptom
until proven otherwise.

## Key hygiene

Two regimes, and never blur them:

- **Development keys are throwaway.** Machine's `keys/` directory
  is labeled, in the repo, as dev keys that hold no value and must
  never be used on mainnet or reused for a real deployment. Label
  yours the same way, in the directory, so nobody — including
  future-you, tired, at a deadline — mistakes them for production
  material.
- **Production keys are generated on the deployment host, only.**
  Never on a dev machine, never committed, never transmitted.
  Fresh keys per deployment, role separation between them, and a
  multisig with a timelock for upgrade authority. This is written
  into Machine's deployment gate (chapter 05), and the habit
  starts now: even testnet keys in this guide's launch path are
  generated on the host and never leave it. *The habit is the
  point.*
