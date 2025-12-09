# Simple Jupiter Earn Treasury (Solana) – Docs Only

A basic treasury that stakes stablecoins in Jupiter's Earn protocol and distributes rewards proportionally to participants. No artificial scaling, no wage guarantees - just honest supplemental gaming rewards with market-rate yields.

- **Emergency withdrawals allowed**; principal protected but accessible if needed.
- **Market-rate rewards only** (~3% APR) distributed proportionally to participants.
- **Stablecoin focus**: Conservative approach with USDC/USDT for safety.
- **Simple distribution**: Equal shares among active participants, no caps or milestones.
- **Eligibility**: Open participation with transparent criteria.
- **Cadence**: Weekly reward distributions with compounding.

Read next:
- `docs/OVERVIEW.md` – concept and flow
- `docs/TOKENOMICS.md` – economics and math
- `docs/MATHEMATICAL_ANALYSIS.md` – scenario testing and proofs
- `docs/SPEC.md` – program accounts, instructions, and invariants (no code)

## Why this model
- **Honest economics**: No false promises of "minimum wage" - provides realistic supplemental income.
- **Simple transparency**: Easy to understand proportional distribution with public dashboards.
- **Sustainable growth**: Conservative yields ensure long-term viability, not short-term hype.

## High-level flow
1. Participants deposit stablecoins into treasury.
2. Treasury stakes in Jupiter's Earn protocol for market yields (~3% APR).
3. Weekly harvest and distribution of rewards proportionally.
4. 70% of rewards compound back into treasury for growth.

## Status
- Critically evaluated and simplified for realistic implementation.
- Ready for development: Solana (Anchor), Jupiter's Earn protocol.
- Focus: Simple, sustainable gaming rewards over complex wage guarantees.

## Contributing
See `CONTRIBUTING.md`. Open design questions and proposals via Issues/Discussions.

## License
MIT – see `LICENSE`.


