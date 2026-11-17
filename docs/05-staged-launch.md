# 05 — Staged launch: value last, proof first

The mistake this chapter exists to prevent: deploying everything
at once, with real assets, on the strength of local tests. The
alternative is a staged path where each phase proves one new
thing, costs little, and risks nothing — and where **value is the
last thing that arrives, after an independent audit, never
before.**

Machine's plan has four phases. Adapt the shape, keep the gates.

## Phase 0 — Public proof of work ($0)

Everything in the open: code, tests, SBF build hashes, runbooks,
honest status docs. Free security passes you can run today —
`cargo audit`, clippy, `cargo-fuzz` over your parsers, a
documented self-audit. Audit quotes requested; grant applications
for audit funding submitted.

*Proves:* the code exists, builds reproducibly, is test-covered.
*Value at risk:* none.

## Phase 1 — Public testnet (~$20/mo)

One modest VPS: validators, public RPC, faucet — on a **test
genesis** whose tokens are worthless by definition. Bridge
programs deployed between your testnet and Solana **devnet**;
crossings executed in both directions with worthless amounts,
plus pause and relayer-outage drills, all written up publicly
(failures included). Your launch standard, if you have one, can
take real registrations here at zero financial risk.

Non-negotiable labeling: the cluster is named *Testnet*
everywhere, its token is a different genesis from any future
livenet token, is not convertible or redeemable, and will never
be "swapped" for the real thing. Say it in every public mention.
Anyone selling test tokens is scamming; say that too.

*Proves:* multi-validator consensus, signed transfers, public
RPC, the full bridge path — end to end, live.
*Value at risk:* none (test tokens).

## Phase 2 — Distributed livenet, still no bridged value (~$60/mo)

Three servers in different regions, one validator each; public
RPC; monitoring and reconciliation running for real. Community
and stake onboarding. The bridge stays **undeployed** on livenet
in this phase. A custody-holding standard (Machine's LaunchGuard
custodies locked creator tokens) deploys here only after its own
review pass.

*Proves:* the network runs distributed, in public, over time.
*Value at risk:* none bridged.

## Phase 3 — Audited bridge deployment (grant/budget-gated)

Only now: the independent audit of the bridge scope (programs,
shared library, relayer, key management), findings fixed and
re-reviewed, report published in full. Then deployment under the
gate below. Worthless-amount crossings on mainnet first, drills
again — and only then real assets, under caps.

*Proves:* the value path is independently reviewed.
*Value at risk:* capped, audited, monitored.

## Phase 4 — Harden and grow

A funded bug bounty sized to the value actually held, caps
raised only with operating history — never ahead of it.

## The deployment gate

The checklist that binds at every phase. No phase may do what
its gate forbids, and the gate does not bend for deadlines,
launches, or announcements:

- [ ] Independent audit complete for the scope being deployed;
      findings fixed or accepted in writing, in public
- [ ] Fresh production keys generated **on the deployment
      hosts** (chapter 03) — never before, never elsewhere
- [ ] SBF binaries built from the audited commit and deployed;
      hashes recorded
- [ ] Initialization with production relayer/admin sets and
      absolute caps — and post-init verification of the stored
      config's *contents* before anything is funded or announced
- [ ] Worthless-amount crossings in **both directions**, plus
      pause and relayer-outage drills, on the production
      deployment
- [ ] Reconciliation and monitoring live before the first real
      crossing, not after

Two refusals worth adopting verbatim:

- **No phase skips the audit because the code "looks fine."**
  Your own tests passing is Phase 0 evidence. It is not an audit,
  and your docs should say so as plainly as Machine's do
  ("unaudited" appears in its README, its status docs, and its
  limitations page).
- **No token sale to fund the audit.** Funding comes from grants
  and budget, in that order. A launch that needs a sale to afford
  its own security review has its phases in the wrong order.
