# PAWDIT Project Handoff — Session Context Document

> **Review addendum (July 1, 2026, branch `claude/phase-zero-one-review-8aa23n`):**
> all artifacts in `reports/` were independently fact-checked. Every
> bitcoin-side "measured fact" in §4 re-verified exactly; four errors were found
> and fixed (report is now rev 6) — see `reports/VERIFICATION-2026-07-01.md`.
> **Amendment to §4:** the reconstruction commands there DO NOT work in a
> dogecat-only Claude Code session — the git proxy 403s every other repo
> (pawdit, dogecoin/dogecoin, bitcoin/bitcoin, even codeload tarballs). Any
> session that needs the analysis workspace or the pawdit project state must
> have those repos attached (`add_repo`) from the start. Phase 0/1 work
> products (Pedigree, patches, any newer handoff) live in
> `taproot-wizards/pawdit` and were NOT reviewable from this session.

**Written:** June 12, 2026, at the end of the founding strategy session (Claude Code
session on taproot-wizards/dogecat, branch `claude/nifty-ptolemy-jq53zw`).
**Audience:** the next Claude Code session(s), centered on `taproot-wizards/pawdit`,
and any human picking up the project. Read this first, then the report, then the
personas.

---

## 1. What this project is (one paragraph)

Taproot Wizards (Udi, founder) is bringing **BIP-347 (OP_CAT) + Taproot + BIP-360
(P2MR) to Dogecoin**, with quantum resistance as the public narrative (CAT+P2MR
enable consensus-enforced hash-based signatures; "vaccinate your DOGE"). Dogecoin
Core is based on Bitcoin Core ~0.14 (2017) and never activated CSV or SegWit, so
the real bundle is **CSV → SegWit (no witness discount) → Taproot + BIP-347 +
P2MR**. The chosen approach: re-implement the small "Dogecoin essence" (~5.6k
lines: AuxPoW, Scrypt, Digishield, subsidy, params) on top of current Bitcoin Core
as a **living patch set with a provenance database**, prove chain equivalence with
a differential harness, demo on a public doge-signet, and present the whole thing
as an AI-forward, verification-backed package. Full rationale, evidence, and
alternatives considered: `reports/dogecat-approach-report.html` (rev 5).

## 2. Repo roles and naming system (decided, not yet all documented elsewhere)

| Thing | Name | Role |
|---|---|---|
| `taproot-wizards/pawdit` | **pawdit** (paw + audit) | **Canonical project repo.** Patch series, provenance DB, harness code, portal source, docs, reports, personas, prompt packs. Small, reviewable, deployable. This is what outsiders watch. |
| `taproot-wizards/dogecat` | **dogecat** | Bitcoin Core fork (currently == upstream master 31.99, zero changes). Two roles: (a) build target — the patch set applies here, binaries/releases come from here; (b) **analysis substrate** — it holds full bitcoin AND dogecoin git histories (see §4). Branches here are disposable/re-creatable; durable history lives in pawdit. |
| Provenance database | **the Pedigree** | Machine-readable records: every change between Dogecoin 1.14.9 and its Core ancestor, classified with provenance, disposition (kept-verbatim / adapted / superseded-by-upstream / dropped), rationale, evidence links, tests. Drives the diff-explorer visualization at every granularity. |
| Conformance suite | **the Sniff Test** | Runnable artifact: full-chain sync verification + differential harness + forward-looking tx/block conformance vectors. "Don't trust the AI, run this." |
| Review portal | **the Vet** | Public website: live parity dashboard, multi-granularity change explorer, test scoreboard, "Open in Claude/ChatGPT/Gemini/Grok" launchers with **adversarial** (never confirmatory) prepared prompts, all plaintext-visible. |

**Suggested pawdit layout** (first session to confirm/adjust):
`docs/` (reports, personas — migrated from dogecat), `pedigree/` (schema + data),
`patches/` (the living patch series), `sniff-test/` (harness), `vet/` (portal),
`prompts/` (adversarial review prompt packs).

## 3. Where the existing artifacts are

All on dogecat branch `claude/nifty-ptolemy-jq53zw`, directory `reports/`:
- `dogecat-approach-report.html` — the strategy report, rev 5. THE reference
  document: approaches A–E, artifact architecture (§6), payload defense (§3.1),
  design decisions (§7), risks (§8), ~40 cited sources. Self-contained HTML.
- `personas/bitcoin-core-reviewer-persona.md` — externally authored (rijndale,
  ex-TW CTO, April 2026). Norms layer verified good; roster wrong — never use
  without the corrections file.
- `personas/bitcoin-core-reviewer-persona-corrections.md` — our fact-check (June
  2026): real maintainer roster (5: fanquake, hebasto, achow101, ryanofsky,
  sedited), LLM-PR culture, activation decoupling, BIP-347/360 engagement map.
- `personas/dogecoin-reviewer-persona.md` — Lodder + chromatic profiles with
  verbatim quotes, confidence ratings, packaging rules. Read §6 before producing
  ANY Dogecoin-facing artifact.
- `personas/README.md` — index + usage ground rules.

**Migration task for the first pawdit session:** copy `reports/` → pawdit `docs/`
(use `add_repo` to attach taproot-wizards/dogecat to the session, branch
`claude/nifty-ptolemy-jq53zw`; the files are plain — no git-history surgery
needed or wanted).

## 4. Reconstructing the dogecat analysis workspace (state that dies with the session container)

The founding session's container had remotes/tags a fresh clone won't have.
To redo any analysis, run in a dogecat clone:

```bash
git remote add dogecoin https://github.com/dogecoin/dogecoin.git
git fetch dogecoin --tags --no-recurse-submodules     # brings v1.14.x tags + branches
git remote add bitcoin https://github.com/bitcoin/bitcoin.git
git fetch bitcoin tag v31.0 tag v30.0 --no-tags       # release tags (fork lacks them)
```

**Key measured facts (verified this session — reusable without re-measuring):**
- Dogecoin v1.14.9 ↔ bitcoin/master merge-base: `9828f9a9962c1bee5c343847030b9cfd87a40a5e` (Feb 2017, Core ~0.14). 14,539 bitcoin commits behind master; Dogecoin side: 14,418 commits (5,051 merges); src/ diff ~71k insertions (excl. qt locale).
- dogecoin `1.21-dev` ↔ master merge-base: `831675c8dccfa6525ffe751da3cc60709c380953` (Nov 2020, Core 0.21). Curated essence diff: 168 files, +5,622/−3,484 in src/. Stalled Feb 2024. **Use as cross-check only, never as the authority (incomplete by definition).**
- Consensus-critical essence (from 1.21-dev): auxpow.cpp/h (~374), scrypt (~455), pureheader (~180), dogecoin.cpp/h (~146+), chainparams (~211), net_processing (~92), validation (~56), pow (~30).
- Dogecoin mainnet consensus state: BIP34/65/66 only (BIP65 height 3,464,751 — last soft fork, 2019). CSV + SegWit **disabled** in chainparams. AuxPoW from block 371,337, chain ID 0x62. "Dogecoin does not use BIP9" (their 1.21-dev comment).
- Bitcoin Core: kernel C API (`src/kernel/bitcoinkernel.h`) **present in tagged v31.0** (April 2026; release announced Apr 19) behind experimental `-DBUILD_KERNEL_LIB=ON` (default OFF); not in v30.0. Release cadence strict 6 months; **v32.0: branch-off Sep 10, target Oct 10, 2026** (bitcoin/bitcoin issue #35122). Plan: develop vs master, release vs v31.x, rehearse re-target onto v32.0 in October.

## 5. Decisions log (binding unless Udi revisits)

1. **Approach A** (essence-port onto current Core), packaged Inquisition-style as a
   parallel client + doge-signet. Approaches B (backport into 1.14) and D (true
   rebase) rejected; C (resurrect 1.21-dev) is fallback/map only.
2. **Full validation from genesis** — non-negotiable compatibility bar.
3. **Artifact architecture = one pipeline, five renderings:** (1) living patch set
   + Pedigree [canonical], (2) dogecat merged distribution [generated], (3) the Vet
   portal [generated], (4) the Sniff Test conformance suite [runnable deliverable],
   (5) activation packages as separate parameter-level overlays (features ship
   dormant from day one; sequencing — staged vs bundled — left to the community).
   **No intermediate-era Core bases, ever.**
4. **SegWit ships bytes==vBytes** — no witness discount, no effective block-size
   increase (Lodder's stated red line, #3684).
5. **AI-forward posture:** lead openly with "AI-assisted, here's the safety
   system." Trust must never rest on trusting the AI — it rests on the Pedigree,
   the Sniff Test, and the harness. Pitch framing: maintainers were right to stop
   chasing Core under manual economics; AI changes the economics; artifacts are
   the proof.
6. **Adversarial prompts only** in the Vet's AI launchers; plaintext-visible.
7. **From-scratch consensus rejected** (btcd evidence; no AuxPoW alt-node has ever
   survived). Stretch goal (post-signet): "next-gen" node wrapping a **dogekernel**
   (libbitcoinkernel pattern; Floresta precedent). Activation proposal rests on
   dogecat alone.
8. **Quantum claims stated precisely:** CAT-based PQ signatures require P2MR
   (BIP-347's own caveat); Foundation's Apr 2026 commit-reveal experiment =
   demand without enforcement; CAT+P2MR = enforcement. NOT to be called "RE-EN"
   (that was an unrelated crank proposal Lodder shot down demanding peer review).
9. **Meme energy stays community-side;** anything Core- or maintainer-facing is
   sober engineering (post-OP_RETURN-war sensitivity; glozow resignation context).
10. **Timeline:** three clocks — authoring (days–weeks), compute (the floor:
    differential IBD over ~6M blocks ≈ a day/run), credibility (months, runs in
    background). Aggressive target: signet live ~3–5 weeks from Phase 1 start;
    activation community-paced (12–24+ months).

## 6. Immediate next steps (Phase 0 — start in the pawdit session)

1. Bootstrap pawdit: copy docs (§3), set repo layout (§2), README that frames the
   project (AI-forward, verification-backed — see decisions 5, 9).
2. **Design the Pedigree schema** — must support: per-record provenance,
   disposition, rationale, evidence URLs, granularity rollups (subsystem → item →
   hunk → line), milestone tags, and generation of the diff-explorer views.
   Decided requirement: "the accounting is a database, not a document."
3. **Run the provenance accounting** (needs dogecat attached, §4): pass 1 —
   `git patch-id` matching of all 14,418 Dogecoin-side commits against full
   bitcoin history (identifies upstream backports mechanically); pass 2 —
   hunk-level classification of the residual until nothing is unlabeled. Cross-
   check against the 1.21-dev diff; discrepancies investigated in both directions.
   Output: the first populated Pedigree + the "What makes Dogecoin Dogecoin" spec.
4. Then Phase 1 (essence port, in dogecat) per report §6 phases.

## 7. People context

- **Udi (udi@taprootwizards.com):** founder, decision-maker. Wants speed (pushed
  back on padded timelines — justify any duration by its bottleneck: authoring vs
  compute vs credibility), candid internal docs, meme-aware but engineering-first.
  Approves all current decisions in §5.
- **rijndale:** former TW CTO (not current; wasn't when he contributed). Authored
  the Bitcoin persona doc; previously evaluated the manual-porting feasibility.
  Treat his artifacts as useful but unverified (one likely-fabricated reviewer
  found — see corrections file).
- **Dogecoin maintainers (Lodder, chromatic):** see persona doc. No contact has
  been made. Nothing public yet. Nothing goes public without Udi's explicit OK.

## 8. Open questions (parked, not blocking)

- Witness-discount design detail: prototype both discount/no-discount on signet
  for data, or commit to bytes==vBytes from the start? (Report §7 leans prototype-
  both; decision 4 sets the default.)
- Activation sequencing: staged (CSV first) vs bundled — deliberately left to the
  community; architecture supports both.
- P2MR adoption timing vs upstream draft churn (activate last; track spec).
- Milestone segmentation of the Pedigree for roadmap views (schema supports it;
  presentation TBD).
- dogekernel/next-gen node: parked until after signet launch.
