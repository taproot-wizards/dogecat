# Verification Log — July 1, 2026 review pass

**Session:** Claude Code on taproot-wizards/dogecat, branch
`claude/phase-zero-one-review-8aa23n`.
**Scope:** independent fact-check of the June 12, 2026 founding artifacts
(`dogecat-approach-report.html` rev 5, `personas/*`, `HANDOFF.md`) before
continuing Phase 1. Methods: this repo's full bitcoin git history, in-tree files
(CMake, CONTRIBUTING.md, SECURITY.md, trusted-keys), and web search for
post-June external claims.

**Session-scope constraint discovered (important for future sessions):** a
dogecat-only session **cannot reach any other GitHub repo — not
`taproot-wizards/pawdit`, not `dogecoin/dogecoin`, not `bitcoin/bitcoin`, not
even public tarballs via codeload.github.com** (the proxy returns 403 "use
add_repo"). The HANDOFF §4 workspace-reconstruction commands only work in a
session with those repos attached. Consequently the Dogecoin-side measured
facts could not be re-verified here (they were measured in the June 12 session
and remain plausible; marked "unverifiable this session" below).

## Verified correct (independently re-measured or confirmed)

| Claim | Result |
|---|---|
| Merge-base v1.14.9 ↔ bitcoin master = `9828f9a99…`, Feb 2017, Core ~0.14 era | ✅ commit exists, dated 2017-02-17 |
| 14,539 bitcoin commits from `9828f9a99` to master (as of Jun 12 sync) | ✅ exactly 14,539 (`git rev-list --count`) |
| 1.21-dev merge-base = `831675c8d…`, Nov 2020, Core 0.21 era | ✅ commit exists, dated 2020-11-17 |
| Kernel C API `src/kernel/bitcoinkernel.h` in master; merged Nov 2025 via PR #30595 | ✅ merged 2025-11-04 |
| Kernel header in v31 line, **not** in v30 | ✅ present on `origin/31.x`, absent on `origin/30.x` |
| `BUILD_KERNEL_LIB` experimental, default OFF | ✅ defaults to `BUILD_UTIL_CHAINSTATE` which is OFF |
| trusted-keys = exactly 5 maintainers | ✅ 5 fingerprints on master |
| sedited added to trusted-keys Jan 8, 2026 | ✅ commit `d1b227f3a` |
| glozow removal merged Jan 28, 2026 (PR #34517) | ✅ — nuance added: removal commit *authored* Sep 18, 2025 |
| dergoegge added to SECURITY.md May 6, 2026 | ✅ commit `0651a1fc1`, exact date |
| sipa/fanquake/achow101/dergoegge are SECURITY.md contacts | ✅ in-tree SECURITY.md |
| OP_CAT = 0x7e (OP_SUCCESS126), not 80 | ✅ `src/script/script.h:136` |
| Core CONTRIBUTING.md quotes (understanding requirement, focused patchsets, review-effort filter, consensus bar) | ✅ all present verbatim (line-wrapped) |
| BIP-347 status "Complete" | ✅ (bips.dev/347, bitcoin/bips) |
| BIP-360 = P2MR, merged as Draft Feb 11, 2026; Heilman co-author | ✅ (bip360.org, Bitcoin Magazine, Gate) |
| Dogecoin Foundation PQ commit-reveal mainnet experiment, Apr 2026, Michi Lumin / Stebbing | ✅ (coinpaper, DL News, u.today) |
| Namecoin nc31.0 exists (essence-port precedent) | ✅ released Apr 20, 2026 |
| Floresta v0.9.0 switched script validation to libbitcoinkernel | ✅ (~15× faster validation; Optech #401) |
| Bitcoin Core 6-month cadence, v31.0 April 2026 | ✅ release announced Apr 19, 2026 |

## Errors found and fixed (this pass → report rev 6)

1. **Litecoin MWEB conflation.** Rev 5 said "~85k LTC inflation exploit +
   13-block reorg, March 2026" as one event. Reality: **March 2026** fake-pegout
   inflation exploit (85,034 LTC; recovered minus an 850-LTC bounty; Core
   0.21.5), and a **separate April 2026** second exploit attempt that caused the
   13-block reorg (~11k LTC loss to NEAR Intents; fixed in 0.21.5.4). Fixed in
   §5·B; the argument it supports is unchanged (arguably strengthened).
2. **"~615k changed lines in src/" included Qt translation churn.** Re-measured:
   excluding `src/qt/locale`, the 9828f9a→master drift is **363k insertions /
   90k deletions ≈ 453k changed lines**; ~615k only when translation files are
   included. Report's own Dogecoin-side figure (71k) excludes locale, so the
   comparison was inconsistent. Fixed in §1 card and §5·D.
3. **v31.0 date.** Stated "Apr 15, 2026"; the release was announced **Apr 19,
   2026** (tag date not verifiable from this session). Softened in report §E and
   HANDOFF §4.
4. **glozow resignation nuance** (corrections file): removal commit authored
   Sep 18, 2025, merged Jan 28, 2026. Both dates now recorded.

## Plausible but unverifiable from this session (needs a session with dogecoin/bitcoin attached)

- Dogecoin-side counts: 14,418 commits (5,051 merges); ~71k insertions in src/
  excl. locale; 1.21-dev curated essence = 168 files, +5,622/−3,484; per-file
  essence line counts (auxpow ~374, scrypt ~455, pureheader ~180, …).
- Dogecoin chainparams facts (SegWit/CSV disabled comments, AuxPoW from block
  371,337, chain ID 0x62, BIP65 at height 3,464,751) — consistent with public
  record, not re-read from source here.
- All dogecoin/dogecoin discussion quotes (#3684 bytes==vBytes, #3509, #3418,
  #2264, per-PR review behavior) — the persona docs' load-bearing quotes.
- bitcoin-inquisition PR #39 diff size (+34/−6).
- v32.0 schedule (branch-off Sep 10 / target Oct 10, 2026, issue #35122) — not
  yet indexed by search; consistent with the strict 6-month cadence pattern.

## Not fact-checked (out of scope)

The Bitcoin persona doc's per-PR reviewer quotes (§6) beyond what the
corrections file already covers; its eval-results claim ("80% hit rate"); press
characterizations of Lodder's Reddit statements.
