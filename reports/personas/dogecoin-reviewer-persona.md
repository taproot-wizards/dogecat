# Dogecoin Reviewer Persona
**Purpose:** Hand this to agents producing artifacts aimed at dogecoin/dogecoin
(spec docs, discussions, PRs, activation proposals). Stress-test our work against
the documented review behavior of the people who control merges.

**Prepared:** June 12, 2026, from public artifacts only (this project).
**Confidence ratings:** **High** = direct verbatim artifact; **Medium** =
secondhand/summary or single data point; **Low** = inference from behavior.

**Method caveat:** Dogecoin Core has ~2 active maintainers (patricklodder,
chromatic) plus a security/trust circle including Michi Lumin. They visibly
coordinate via a shared project board and almost certainly coordinate privately —
patricklodder's and xanimo's GitHub activity feeds are set to private — so public
artifacts materially under-represent their process. Everything below is the
*visible* fraction. See §7 for what we could not learn.

---

## 1. Patrick Lodder (patricklodder)

**Role:** De facto lead maintainer; release manager (authored v1.14.9 and its
release-notes PR [#3727](https://github.com/dogecoin/dogecoin/pull/3727));
triager-in-chief (labels; "Review & merge board" statuses: *needs review /
blocked / needs rework*); one of three PGP security contacts in
[SECURITY.md](https://github.com/dogecoin/dogecoin/blob/master/SECURITY.md);
Dogecoin Foundation dev-fund custodian alongside chromatic, Michi Lumin, Marshall
Hayner, Ross Nicoll. Professional background at Block.io. **High.**

### Core philosophy (verbatim)
- **Security as the terminal value:** *"The security of the chain is the most
  important thing there is because without it, there is no value."* —
  [discussion #2264](https://github.com/dogecoin/dogecoin/discussions/2264),
  Mar 2021. **High.**
- **No roadmap promises:** *"There's never a timeline. However, I know of multiple
  people that are working in the background to propose enhancements that could
  ultimately enable taproot."* —
  [#3684](https://github.com/dogecoin/dogecoin/discussions/3684), Nov 2024. **High.**
- **Use-case-first interrogation:** his first response to the Taproot request was
  *"What do you want to do with taproot?"* (#3684). **High.**
- **His stated acceptance condition for SegWit/Taproot-style changes — the single
  most important sentence in this document:** *"Most people...can agree with an
  outcome where we DO segregate the signatures off the tx body (to prevent
  multi-party malleability) but DO NOT provide any economic incentives or extra
  block size... As long as there is no economic incentive (as in bytes==vBytes and
  max_vBlockSize==max_blocksize)"* — #3684, Nov 25, 2024. He wants malleability
  fixes **without Bitcoin's witness discount**, explicitly to avoid subsidizing
  data/inscription spam. **High.**
- **Consensus conservatism re: AuxPoW:** in the 2021 1.21 plan he excluded BIP9
  versionbits as *"too risky to do at once... especially for a meta-feature"*,
  citing AuxPoW compatibility; bundled SegWit+CSV because *"it will save us all
  time when we do them at once instead of sequential"*; activation via 95%
  SuperMajority — [#1798](https://github.com/dogecoin/dogecoin/issues/1798) /
  [#2264](https://github.com/dogecoin/dogecoin/discussions/2264). **High.**
- **CSV framed as enabler + pedagogy:** *"This is more of an enabler than a
  feature. It could enable Lightning, or something more intricate"* (#2264);
  *"this could be an excellent learning opportunity for those devs that have thus
  far not gained any practical experience with protocol extensions"* —
  [#3509](https://github.com/dogecoin/dogecoin/discussions/3509). **High.**
- **Release engineering pragmatism:** authored the Feb 2024 pivot
  ([#3418](https://github.com/dogecoin/dogecoin/discussions/3418)) abandoning the
  Bitcoin-0.21-based "1.21" for a 1.15 built from 1.14.7 (drop Gitian, OpenSSL,
  BIP70, ancient OS support): *"Doing 1.15 would give people the opportunity to
  get more comfortable with the code and protocol as-is while touching it"*;
  *"We could even do the CSV soft-fork on a subsequent 1.15.x...protocol
  development doesn't have to be stalled."* Follow-through: Aug 2024 action plan
  [#3620](https://github.com/dogecoin/dogecoin/issues/3620). **High.**

### How he reviews (observed)
- **Architecture & sequencing over line-level nits.** On edtubbs' BIP39/44 import
  [PR #3870](https://github.com/dogecoin/dogecoin/pull/3870): demanded multi-wallet
  support land first to avoid *"mixing of addresses from multiple seeds inside a
  single wallet"*; rejected depending on *"the entirety of libdogecoin for
  something that can be expressed in maybe 400-600 lines of CPP"*; raised BIP39
  checksum concerns. **High.**
- **Future-proofing and privacy as review lenses.** On
  [PR #3722](https://github.com/dogecoin/dogecoin/pull/3722) (estimatefeereal):
  *"This will be broken the moment we implement non-deterministic input selection
  (and we should, because we've been warned about not having it allowing wallet
  identification.)"*; also API naming and integration concerns. **High.**
- **Tone-policing toward generosity.** On
  [PR #3853](https://github.com/dogecoin/dogecoin/pull/3853), rebuking another
  reviewer's NAK: *"It's a one-line change to make it acceptable - so why NAK?
  Please reserve that for things that are unfixable and unsalvageable. Now, your
  future NAKs lost a lot of it's value because you applied it loosely."* **High.**
- **Process transparency:** declared the 1.14.9 release-notes merge window
  explicitly (#3727); closes duplicates curtly; routes even trivial outsider
  cleanups through label+review-request process (#3996/#3997). **High.**
- **Skeptical of weakening tests:** moved
  [PR #3999](https://github.com/dogecoin/dogecoin/pull/3999) (relaxing a flaky
  `rescan.py` assertion) to "needs rework". **Medium/Low** on interpretation.

### Security track record
Shipped 1.14.9 (Dec 1, 2024) quietly containing the fix for the bug weaponized 11
days later as "DogeReaper" (attack by Andreas Kohl, co-founder of Bitcoin sidechain
Sequentia; crashed ~69% of nodes because 87% of operators hadn't upgraded;
underlying research credited to Tobias Ruck). Earlier: coordinated Halborn-disclosed
fixes in 1.14.6; led the 2021–22 fee-policy reduction. Favors PGP-encrypted private
disclosure. Secondhand (press): cautious on NFTs-on-Dogecoin, wary of things done
*"in the name of pumping the price"*; called PoS discourse *"really contentious"*
and the community *"hyper reactionary"*. **High** on facts; **Medium** on Reddit
material.

### What earns approval / triggers pushback
**Approval:** clear use case up front; small dependency footprint; sequencing that
respects wallet/protocol architecture; salvageable fixes being salvaged; security
relevance; backport-friendliness to 1.14.
**Pushback:** economic incentives for block-space data; meta-changes touching
AuxPoW without analysis; big dependency imports; "feature first, architecture
later"; loose NAKs / harsh review culture; weakened tests; timelines demanded of
him.

---

## 2. chromatic

**Identity — confirmed.** The dogecoin org member chromatic is the Perl-community
author: GitHub profile links onyxneon.com/outspeaking.com; pins
`modern_perl_book`, `Alien-Libdogecoin`, `dogecoin-tricks-book`; author of
*Modern Perl*, *Extreme Programming Pocket Guide*, co-author *Perl Testing: A
Developer's Notebook*; original author of **Test::Builder** — the foundation of
Perl's entire testing ecosystem. Wrote *Dogecoin Tricks* and a "Dogecoin Dev
Diary" blog. Dev-fund custodian; security contact. ~25 years of public writing on
TDD/XP — his Dogecoin behavior matches it exactly. **High.**

### How he reviews (observed)
- **Personally executes the test suite before every ACK.** Near-invariant approval
  formula: **"ACK; all tests pass on x86-64 Linux."** — verbatim on
  [#3996](https://github.com/dogecoin/dogecoin/pull/3996),
  [#3997](https://github.com/dogecoin/dogecoin/pull/3997),
  [#3978](https://github.com/dogecoin/dogecoin/pull/3978),
  [#3985](https://github.com/dogecoin/dogecoin/pull/3985), and #3727. **High.**
- **Demands a destination before committing to a direction.** On
  [PR #3972](https://github.com/dogecoin/dogecoin/pull/3972) (CFeeRate methods +
  "comprehensive tests"): *"I don't have strong feelings, but I'm not sure where
  we'd use this code yet... It seems like the first part of a multi-part
  development, so I'd like to know where you want to take this work."* Extra tests
  alone did **not** buy approval absent a use case. **High.**
- **Deliberate, staged commitment on consensus.** On CSV (#3509): *"👍 to bringing
  in CSV (it's mature and well understood...) ... I want to think about the
  specific implementation in more detail before I fully commit to it."* **High.**
- **Defers to Patrick on architecture:** *"I like this idea in general but want to
  reinforce Patrick's point on wallet support"* (#3870). **High.**
- **Courteous, Socratic; thanks contributors.** (#3853, #3978). **High.**
- **Wants cadence:** *"I think it's more likely to get us on a more frequent
  release cadence, which I always like"* (#3418). **High.**
- **Merge behavior:** performs most recent master merges. Fast lane = mechanical,
  verifiable, test-passing cleanups (2–5 days). Slow lane = anything whose purpose
  or end-state is unclear (sits indefinitely with a question posted). **High** on
  pattern, **Medium** on generalization.

**Earns approval:** locally-runnable green tests; small verifiable diffs; explicit
outcomes and the multi-PR trajectory; alignment with Patrick's architectural calls.
**Triggers stall (he rarely rejects outright):** unclear purpose; "part 1 of N"
without the map; anything requiring him to guess intent.

---

## 3. Secondary figures

- **xanimo** — trusted release/CI insider (GH Actions upgrades #3980, lief pin
  #3969, own GPG key rotation #3968 → implies a release-signing role). Activity
  feed private. Near-frictionless merges. **High** facts / **Medium** framing.
- **Michi Lumin (michilumin)** — Dogecoin Foundation principal engineer; leads
  libdogecoin and Dogebox. Not just a parallel power center: named security
  contact on dogecoin/dogecoin and dev-fund custodian — inside the trust circle.
  Foundation blessing ≠ fast lane in Core review (see edtubbs). **High.**
- **Ed Tubbs (edtubbs)** — libdogecoin dev; his Core PRs (#3870 BIP39/44; #3188
  open since Dec 2022) get rigorous architectural pushback and multi-year latency
  despite Foundation adjacency. Responsive to review (removed deps, vendored
  utf8proc, added sweep mechanism after objections). Lesson: *nobody* gets a fast
  lane on substantive code. **High.**
- **rnicoll (Ross Nicoll)** — historical maintainer; 2021-era PRs sit abandoned;
  still a dev-fund custodian; not an active gate in 2024–26. **Medium.**
- **pmzajaczkowski** — most active *outsider* of 2026 (cleanup/CI/QA): trivial PRs
  merged in days; CI-linting PR #3998 marked draft+blocked; test-relaxation #3999
  sent to "needs rework". A live case study of outsider onboarding. **High.**
- **inevitable360** — could not substantiate any gating role. **Low/unresolved.**

---

## 4. Collective culture and norms

**Written rules** ([CONTRIBUTING.md](https://github.com/dogecoin/dogecoin/blob/master/CONTRIBUTING.md)) — verbatim, **High**:
- Scope: *"a pull request could add a feature, fix a bug, or refactor code; but
  not a mixture."* Avoid large/complex PRs.
- Tests: bug fixes need *"unit tests demonstrating the bug and also proving the fix."*
- Acceptance: PRs must *"have a clear use case, fix a demonstrable bug or serve
  the greater good of Dogecoin."*
- Governance: maintainers weigh opinions with *"common sense judgement and also
  may weight based on meritocracy"*; merge requires a maintainer other than the
  author + **24-hour waiting period**.
- Consensus changes: *"Consensus rule changes, Policy changes, Maturing
  experimental features into production"* are *"expected to have significant
  discussion before approval and merge."*

**Observed practice:**
- **Two-tier latency.** ~98 open PRs, oldest from Jan 2021. Trivial verified
  cleanups: 2–8 days. Substantive features: months-to-years, review in bursts
  (#3722: reviewed in 3 days, then ~9 months silence). Review bandwidth, not
  hostility, is the bottleneck. **High.**
- **Large PRs effectively die.** The 1.21 rebase program stalled three years and
  was abandoned for incremental modernization of the 1.14 lineage they fully
  understand (#3418). The defining cultural event: **they chose comprehensibility
  over upstream parity.** **High.**
- **Attitude to Bitcoin Core:** pragmatic selective consumer — backports fixes,
  adopts mature primitives (CSV *"mature and well understood"*), rejected
  wholesale rebasing, explicitly rejects witness-discount economics. No tribal
  hostility found; no deference either. **High.**
- **AI/LLM contributions: no public debate found.** No policy; the May 2026
  cleanup wave was labeled, tested, and merged with zero AI-suspicion commentary.
  Their de facto filter (chromatic runs tests, Patrick checks scope) absorbed it.
  **High** on absence-of-evidence — which is not evidence of attitude.
- **Taproot Wizards / OP_CAT: no public maintainer reaction found, either way.**
  Adjacent signals: live community skepticism about JPEG/inscription block-space
  use (#3684); Patrick's bytes==vBytes condition is engineered to deny
  inscription-style discounts; and the most recent high-profile Bitcoin-world
  interaction with Dogecoin was a Bitcoin-sidechain founder crashing 69% of their
  nodes (DogeReaper). Outside-Bitcoin provenance currently has negative salience.
  **Medium (contextual inference, flagged as such).**

---

## 5. What they have explicitly said they WANT

From #3418, #3509, #3620, #3684 — **High**:
1. **1.15 modernization of the 1.14 lineage** (drop Gitian/OpenSSL/BIP70/old OS;
   Qt upgrade; code quality/perf); 1.14.x to security-only extended support.
2. **More frequent release cadence** (chromatic, verbatim).
3. **Versionbits repair** *"to support 6 concurrent protocol proposals"* (#3509) —
   activation *infrastructure* before activation *content*.
4. **CSV activation** as the first soft fork, BIP9-style, explicitly framed as a
   **training exercise** to grow protocol-capable developers.
5. **Developers who learn the codebase as-is.**
6. **Signature segregation without economic incentives** as the acceptable path
   toward Taproot.
7. **Multiwallet before seed-phrase features** (#3870).
8. Implicitly throughout: **review bandwidth and node-operator upgrade hygiene**
   are their scarce resources.

---

## 6. How to package a large outside proposal (evidence-grounded)

1. **Answer "what do you want to do with it" first.** Patrick's literal first
   question on Taproot; chromatic's blocking question on #3972. Open with concrete
   user outcomes on Dogecoin, not capability abstractions.
2. **Start in GitHub Discussions, not a PR.** Every consensus initiative they've
   embraced began as a discussion Patrick authored or co-shaped. Aim for the
   proposal to become *their* plan ("multiple people working in the background" is
   his stated normal mode).
3. **Map onto their declared sequence:** versionbits fix → activation tooling →
   CSV → signature segregation. Contributing to steps they already want buys
   standing faster than pitching a destination feature.
4. **Honor the bytes==vBytes red line.** Any SegWit/Taproot derivative ships with
   no witness discount and no effective block-size increase, plus an explicit
   anti-data-spam economics section. Importing Bitcoin's discount as-is is a
   documented non-starter.
5. **Include an AuxPoW compatibility analysis** for anything touching block
   versioning — their stated reason for past exclusions.
6. **Decompose ruthlessly.** "Feature, fix, or refactor — not a mixture"; atomic
   commits; each PR independently testable. The 1.21 monolith died; cleanup-sized
   PRs merge in days.
7. **Make chromatic's ACK cheap:** everything passes the full suite on x86-64
   Linux out of the box — he personally runs it before every approval. Bug fixes
   need failing-then-passing tests. Never relax flaky tests.
8. **Minimal dependencies.** "400-600 lines of CPP" beats importing a library.
9. **Commit reviewers, not just code.** Two maintainers, ~98-PR backlog,
   multi-year latency even for insiders. Offer named, sustained review/testing
   labor on *their* queue (CSV, 1.15 items) before and alongside the proposal.
   Plan in quarters, not weeks.
10. **Match tone norms:** no NAK brinkmanship, no timeline pressure, no price/pump
    framing, state non-goals explicitly, answer "where do you want to take this
    work" with the full multi-PR map up front.
11. **Provenance management (flagged inference):** the last famous Bitcoin-native
    actor in their world crashed 69% of their nodes. Expect heightened scrutiny of
    security claims; use the SECURITY.md PGP channel for anything sensitive; show
    visible respect for the 1.14/1.15 lineage rather than "Bitcoin does it"
    arguments — their defining 2024 decision was to *stop* chasing Bitcoin Core.

---

## 7. What we could NOT learn from public data

- **Private coordination:** patricklodder's and xanimo's activity feeds are
  private; the maintainers plainly sync off-channel. Actual decision dynamics are
  invisible.
- **The current merge-rights roster:** inferred from observed merges; dormant
  holders can't be ruled out.
- **Verbatim Reddit/X content:** u/patricklodder statements available only
  secondhand via press; chromatic's "Dogecoin Dev Diary" long-form views
  (outspeaking.com blocked).
- **Hidden inline review comments:** GitHub collapses resolved threads — we see
  dispositions ("blocked", "needs rework") but not always reasoning.
- **Quantitative latency statistics:** sampled, not measured (API rate limits).
- **Any private position on OP_CAT, Taproot Wizards, or covenants** — zero public
  artifacts either way.
- **Funding reality:** whether the dev fund pays anyone; whether money or review
  bandwidth is the binding constraint.
- **Whether AI-generated PRs have been privately discussed or filtered.**
