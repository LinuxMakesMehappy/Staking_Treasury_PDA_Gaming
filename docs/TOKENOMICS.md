# Tokenomics

## Variables
- C: principal capital (USD, value of stablecoins in Jupiter Earn)
- r: Jupiter Earn reward rate (annual percentage, Fluid-powered)
- y: daily Earn rewards = C * r / 365
- p: target base pay per player per day (USD), configurable
- S: sustainable players per day = floor(y / p)

## Milestones
- Milestone slots = floor(C / 50,000).
- Active funded players = min(cap, max(S, Milestone slots)) or use milestones as a hard cap for determinism.

## Distribution cadence
- Every N minutes (e.g., 10m), per-player target payout = p * (N / 1440).
- If available yield < target, pay pro-rata; deficit does not touch principal.
- Surplus compounds (minus a small buffer to reduce LP churn).

## Risk notes
- Jupiter Earn risk: Fluid protocol or Jupiter integration failures → mitigated by Jupiter's due diligence and stablecoin focus.
- Stablecoin depeg risk: mitigated by Jupiter's conservative stablecoin selection (USDC, USDT, etc.).
- Smart contract risk: audits and staged rollout; freeze upgrades after stabilization.
- Reward volatility: dynamic target protects principal; milestones add predictability.


