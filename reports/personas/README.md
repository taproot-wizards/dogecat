# Reviewer Personas

Research artifacts modeling how the maintainers/reviewers of target repositories
evaluate contributions. Used to stress-test our artifacts (spec docs, PRs,
activation proposals) before anything goes public.

| File | Subject | Provenance | Status |
|------|---------|------------|--------|
| `bitcoin-core-reviewer-persona.md` | bitcoin/bitcoin review culture | Authored externally (rijndale, ex-TW CTO, April 2026, "Hermes Agent"); received June 12, 2026; **not independently verified** | Norms layer (§3–§13) verified sound. Roster substantially wrong — **read the corrections file alongside it.** One listed reviewer (purpleKarRot) appears fabricated/misattributed. |
| `bitcoin-core-reviewer-persona-corrections.md` | Corrections + addendum to the above | This project, fact-checked June 12, 2026 against trusted-keys ledger + GitHub activity | Current. Real maintainer count: 5 (fanquake, hebasto, achow101, ryanofsky, sedited); glozow resigned Jan 2026; jnewbery/practicalswift long inactive; adds missing reviewers (darosior, dergoegge, et al.), 2025–26 culture changes (LLM-PR norms, OP_RETURN fallout, activation decoupling), BIP-347/360 engagement map. |
| `dogecoin-reviewer-persona.md` | dogecoin/dogecoin review culture (Lodder, chromatic, et al.) | This project, researched June 12, 2026, public artifacts only | Current. All claims carry High/Medium/Low confidence + source URLs. Caveat by design: ~2 maintainers who coordinate privately — public artifacts under-represent their process (see its §7). |

Usage caveats:
- Personas are evidence-based caricatures, not people. Use them to find weaknesses
  in our own artifacts, never to flatter or manipulate individuals.
- Every claim should carry a source URL; treat unsourced claims as hypotheses.
