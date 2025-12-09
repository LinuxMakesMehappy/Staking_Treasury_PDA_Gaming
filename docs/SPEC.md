# Program Specification (no code)

## Accounts
- Config PDA
  - jupiter_earn_program_id, fluid_vault_id, oracle_pubkey, harvest_cadence_sec, target_pay_per_day_usd
  - active_cap, stablecoin_whitelist[], optional collection_mint/stake_mint

- Treasury PDA
  - Holds stablecoin deposits in Jupiter Earn (Fluid-powered positions)
  - Tracks `principal_floor` (sum of external inflows − cumulative authorized rewards)
  - Enforces non-withdrawal invariant

- Rewards Vault (program-owned ATA)
  - Temporary sink for harvested Jupiter Earn rewards prior to distribution

- Player Registry PDA
  - Entries: { wallet, eligibility_kind (NFT/STAKE), weight_bps, active, last_paid_ts }

- Crank State PDA
  - last_harvest_ts, last_distribution_ts, anti-front-run flags

## Invariants
- No instruction can reduce Treasury below `principal_floor`.
- No generic token transfer from Treasury to arbitrary accounts.
- Only Jupiter Earn protocol deposits/withdrawals; only Jupiter's stablecoin whitelist.
- Only Rewards Vault → player payouts, bounded by available Earn rewards.
- Earn position changes only within Jupiter Earn (Fluid); must preserve principal floor.

## Instructions (names illustrative)
- initialize_config(params)
- ingest_fee(amount)                   // CPI from game/service (stablecoins only)
- register_nft(proof) | stake(amount)  // eligibility paths
- update_active_set()                  // milestones/cap/rotation
- deposit_to_earn(amount, stable_mint) // deposit stablecoins to Jupiter Earn
- harvest_and_distribute()             // keeper-called every few minutes
- set_params(delta)                    // guarded, cannot weaken floor or add withdrawals

## Keeper cadence
- On each call:
  1) Harvest Jupiter Earn rewards to Rewards Vault
  2) Compute active set from TVL and rules
  3) Calculate period payouts vs available rewards
  4) Distribute to players; compound surplus; update accounting

## Oracles and USD math
- Use Pyth/Switchboard to value Jupiter Earn positions in USD for milestone calculations.
- Focus on Jupiter's stablecoin whitelist; maintain small buffer (e.g., 0.5–2%) for operational flexibility.

## Governance & Safety
- Multisig/timelock for parameter changes.
- No upgrade may introduce withdrawals or decrease floor enforcement.
- Recommended path to lock/burn upgrade authority post-audit.


