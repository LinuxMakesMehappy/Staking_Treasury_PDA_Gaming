# Program Specification (no code)

## Simple Accounts
- Treasury PDA
  - Holds stablecoins and Jupiter Earn positions
  - Tracks total deposits and rewards earned
  - Allows emergency withdrawals

- Rewards Vault
  - Accumulates weekly rewards for distribution
  - Distributes proportionally to participants

- Player Registry PDA
  - Entries: { wallet, eligibility_kind (NFT/STAKE), weight_bps, active, last_paid_ts }

- Crank State PDA
  - last_harvest_ts, last_distribution_ts, anti-front-run flags

## Simple Rules
- Only USDC/USDT deposits accepted
- Proportional reward distribution
- Emergency withdrawal option available
- 70% compounding, 30% distribution

## Simple Instructions
- deposit_stablecoins(amount)          // USDC/USDT deposits only
- withdraw_emergency()                 // emergency exit option
- harvest_weekly_rewards()             // weekly distribution
- claim_rewards()                      // participant reward claims

## Weekly Process
- Harvest Jupiter Earn rewards
- Calculate proportional shares for all participants
- Distribute 30% as rewards, compound 70%
- Update public dashboard

## Simple Math
- Daily yield = capital × 0.03 ÷ 365
- Weekly rewards = daily yield × 7
- Per participant = weekly rewards ÷ participants

## Governance
- Community multisig for parameter changes
- Transparent public dashboard
- Emergency withdrawal always available


