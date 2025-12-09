# Perpetual Jupiter Earn Treasury (Solana) – Docs Only

A non-withdrawable, perpetually compounding Treasury PDA that deposits stablecoins into Jupiter's Earn protocol (powered by Fluid) and streams only the staking rewards to players. Principal can never be withdrawn; rewards scale funded player slots at $50k TVL milestones. Focuses on stablecoin safety with optimal yields.

- **No human withdrawals**; principal is locked permanently.
- **Jupiter Earn rewards only** (from Fluid-powered stablecoin staking) are distributed to active players.
- **Stablecoin focus**: Safety-first with USDC, USDT, and other Jupiter Earn supported stables.
- **Scaling**: +1 funded player per $50k TVL milestone; optional cap/rotation.
- **Eligibility**: via staking or owning verified NFTs.
- **Cadence**: harvest and distribute rewards every few minutes via a keeper.

Read next:
- `docs/OVERVIEW.md` – concept and flow
- `docs/TOKENOMICS.md` – economics and math
- `docs/MATHEMATICAL_ANALYSIS.md` – scenario testing and proofs
- `docs/SPEC.md` – program accounts, instructions, and invariants (no code)

## Why this model
- Aligns value with players: sustainable “near minimum wage” target funded by yield.
- Principal compounds, increasing durable yield over time.
- Deterministic scaling via TVL milestones; optional rotation for fairness.

## High-level flow
1. Game/service routes stablecoins to the Treasury PDA.
2. Treasury deposits stablecoins into Jupiter's Earn protocol (Fluid-powered).
3. Keeper harvests Earn rewards every few minutes.
4. Rewards are split among active players; surplus compounds; principal remains locked.

## Status
- Design draft for dev review; no code in this repo.
- Implementation targets: Solana (Anchor), Jupiter's Earn protocol powered by Fluid.

## Contributing
See `CONTRIBUTING.md`. Open design questions and proposals via Issues/Discussions.

## License
MIT – see `LICENSE`.


