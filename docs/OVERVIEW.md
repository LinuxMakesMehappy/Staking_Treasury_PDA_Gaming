# Overview

## Concept
A program-owned Treasury PDA deposits stablecoins into Jupiter's Earn protocol (powered by Fluid) to earn staking rewards. The **principal is permanently locked**, with **no human withdrawal paths**. Only harvested **Earn rewards** are distributed to players. Stablecoins provide safety while maximizing yields.

## Goals
- Provide near minimum-wage equivalent earnings for active players.
- Grow Treasury via fee inflows and compounding Jupiter Earn rewards.
- Scale funded players deterministically: +1 per $50k TVL milestone; optional cap/rotation.
- Maintain safety through Jupiter's curated stablecoin selection.

## Flow
1. Fees from a game/service route stablecoins by CPI into the Treasury PDA.
2. Treasury deposits stablecoins into Jupiter's Earn protocol (Fluid-powered).
3. Keeper harvests Earn rewards every few minutes, sending rewards to a rewards vault.
4. Rewards are split among active players; leftover compounds; principal never decreases.

## Safety rails
- No general `withdraw` instruction exists.
- Principal floor enforced across all transfers and Earn deposits/withdrawals.
- Only Jupiter Earn protocol (Fluid-powered) with Jupiter's stablecoin whitelist.
- Price oracles used for USD TVL calculations; focus on depeg-resistant stables.
- Upgrades gated (multisig/timelock) with path to freeze after audit.

## Eligibility
- Stake a specified token OR hold a verified NFT collection.
- Active set determined by TVL milestones, optional cap, and epoch rotation.


