# Corrections & Addendum: Bitcoin Core Reviewer Persona

**Applies to:** `bitcoin-core-reviewer-persona.md` (externally authored, April 2026)
**Fact-checked:** June 12, 2026, against `contrib/verify-commits/trusted-keys`,
GitHub activity, and news coverage. Sources inline. The original doc's *norms*
sections (§3–§13: PR format, commit hygiene, ACK vocabulary, consensus bar) check
out against CONTRIBUTING.md and observed practice — keep them. Its *people*
sections are substantially wrong — use the roster below instead.

---

## 1. The actual maintainer roster (5 people, not 9)

Authoritative source: `contrib/verify-commits/trusted-keys` on master contains
exactly five fingerprints (June 2026):

| Maintainer | Notes |
|---|---|
| **fanquake** (Michael Ford) | Build/release; longest-tenured current keyholder |
| **hebasto** (Hennadii Stepanov) | Build, cross-platform (key added Apr 2021) |
| **achow101** (Ava Chow) | Wallet; also a SECURITY.md contact (key added Dec 2021) |
| **ryanofsky** (Ryan Ofsky) | Validation/wallet interfaces (key added May 2023) |
| **sedited** (TheCharlatan) | **Added Jan 8, 2026** — kernel/libbitcoinkernel specialist. The original doc lists him as a rank-and-file ACK-giver. |

**glozow (Gloria Zhao) resigned as maintainer Jan 28, 2026** (trusted-keys removal
commit; [DL News](https://www.dlnews.com/articles/people-culture/gloria-zhao-steps-down-as-bitcoin-core-maintainer/)),
in the fallout of the OP_RETURN relay war and sustained harassment. Active as a
contributor at least through Feb 2026; current engagement unconfirmed.

**Listed as "maintainers" in the doc but are NOT (long-tenured contributors only):**
- **sipa** — removed his own trusted key July 7, 2022. Still active (cluster
  linearization commits, Mar 2026) and still a security contact. His reviews still
  carry enormous weight; his merge key does not exist.
- **maflcko** — key reverted Feb 21, 2023 ([Bitcoin Magazine](https://bitcoinmagazine.com/technical/bitcoin-core-maintainer-marco-falke-to-step-down)).
  Still a very active commenter (583 issue/PR comments matched since Mar 2026).
- **vasild, sdaftuar, theuni** — never in the trusted-keys ledger. All still active
  contributors (May 2026, Jan 2026, Apr 2026 respectively).

## 2. Roster errors

- **jnewbery** — inactive since **July 2022**. Remove.
- **practicalswift** — inactive since **Oct 2021**. Remove. (The doc's fuzzing
  questions are still asked — by different people; see §3.)
- **purpleKarRot** — ⚠️ **likely fabricated or misattributed.** The only GitHub user
  of that name is Daniel Pfeifer, a CMake/Boost developer with no visible Bitcoin
  activity. The "implementation gravity well" quote the original doc attributes to
  them on #34495 could not be sourced. Treat that quote as unverified (the *insight*
  remains useful; the citation does not).
- **optout21** — a rust-lightning/LDK developer, not a Core consensus reviewer.
- **Confirmed active from the doc's reviewer list:** l0rinc, stickies-v, fjahr,
  pinheadmz, hodlinator, instagibbs (all with commits May–June 2026).

## 3. Important reviewers the doc misses

- **darosior (Antoine Poinsot, Wizardsardine)** — biggest omission. Champion of
  Consensus Cleanup (BIP-54); actively writing deployment/versionbits code (June
  2026). Co-discoverer of btcd's CVE-2024-38365. Consensus-safety-first, long written
  rationale. If dogecat work ever reaches Core's orbit, he is a likely reviewer of
  anything touching activation machinery.
- **dergoegge (Niklas Gögge, Brink)** — fuzzing/security authority; added as an
  official SECURITY.md contact **May 6, 2026**. Concept NACK'd the AGENTS.md
  proposal. Found both 2024 btcd consensus CVEs.
- **instagibbs (Greg Sanders)** — underweighted by the doc: central mempool/policy
  author-reviewer of 2025–26 (TRUC, ephemeral dust, package relay) and key figure
  in the CTV/CSFS technical track.
- **ismaelsadeeq, marcofleon, brunoerg** (fee estimation; fuzzing/type-safety;
  fuzzing/bitcoinfuzz) — the active "next generation" of reviewers.
- **ajtowns (AJ Towns)** — still influential in activation-process debates and
  inquisition methodology; relevant to us specifically because dogecat's deployment
  strategy copies his playbook.

## 4. Culture updates since the doc was written

1. **LLM/AI-assisted PRs.** No AGENTS.md, no disclosure mandate — Sjors' PR #33662
   proposing `Assisted-by:` trailers was **closed in 4 days** (Oct 2025) with
   Concept NACKs (l0rinc: like disclosing your IDE; dergoegge: DrahtBot already
   flags suspected LLM PRs heuristically; kanzure: in-tree agent files are a
   prompt-injection risk). The operative policy is CONTRIBUTING.md's understanding
   requirement: *"Pull request authors must fully and confidently understand their
   own changes"* — with immediate closure otherwise. **Implication for us: AI
   provenance is not the issue; defensibility is. Every line we submit anywhere
   must have a human (or at minimum, an accountable author) who can defend it
   live. Our classification database — where every change carries a recorded
   rationale — is exactly the right counter-artifact.**
2. **OP_RETURN war fallout.** The June 2025 relay statement (31 signatories),
   Core-vs-Knots split, and the harassment campaign that preceded glozow's
   resignation have made reviewers **acutely hostile to anything resembling
   brigading or social-pressure campaigns.** A meme army pointed at Core's repo
   would backfire catastrophically. Meme energy belongs on the Dogecoin/community
   side; the Core-facing surface must be sober engineering.
3. **Quarkslab audit (Nov 2025).** First third-party audit: zero critical/high/
   medium findings across p2p/mempool/consensus. External contributions in those
   areas are now measured against an audited baseline.
4. **Activation is procedurally decoupled from Core.** CTV's activation push lives
   in a separate activation client (bit 5, start Mar 30 2026, timeout Mar 2027,
   90% threshold) after Core declined to bundle review with activation; OP_CAT's
   own PR disclaims being an activation signal. **Conflating implementation review
   with activation advocacy in a Core PR is a fast NACK.** (Dogecoin note: we
   should mirror this separation — dogecat the codebase vs. the activation
   proposal as cleanly distinct artifacts.)

## 5. BIP-347 / BIP-360 engagement map (June 2026)

- **PR #29247 (OP_CAT):** still open as draft; **arminsabouri** is the active
  champion (last push May 16, 2026, functional tests, CI green); Ethan Heilman
  publicly shifted focus to quantum research (Apr 2026) but intends to return.
  Technical debate on record: moonsettler's "neutered CAT" (80-byte result limit)
  vs. bigspider's counter. **No recorded ACKs from sitting maintainers or the
  heavyweight consensus reviewers** — engagement remains covenant-community-driven.
  (Full thread census incomplete — API limits; needs an authenticated pass.)
- **BIP-360 (P2MR):** merged as Draft Feb 11, 2026; acknowledged reviewers include
  **achow101 (a sitting maintainer)**, Jon Atack, Jameson Lopp, Antoine Riard.
  Companion BIP-361 covers migration of vulnerable coins. **Heilman bridges 347
  and 360** — the same person co-authors both halves of our quantum story.

## Unverified items carried over

Full OP_RETURN-statement signatory list; complete #29247 ACK/NACK roster;
maflcko's 2026 commit volume; sr-gi/tdb3 2026 review activity; the original doc's
per-PR quotes (#34124, #34495 et al.) were not independently re-verified except
where noted above.
