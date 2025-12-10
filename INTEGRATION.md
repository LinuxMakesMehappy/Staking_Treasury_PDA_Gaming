# GameStake Integration Guide

For game developers integrating GameStake rewards.

## Requirements

1. **Minimum TVL**: $36,500 USDC to activate (funds 1 player slot)
2. **Game NFT**: Players must hold your game's NFT to be eligible
3. **Activity Reporting**: Your game must report player activity on-chain or via oracle

## Quick Start

### 1. Initialize Your Treasury

```typescript
import { GameStake, DistributionMode } from '@gamestake/sdk';

const gamestake = new GameStake({
  rpcUrl: 'https://api.mainnet-beta.solana.com',
  gameId: 'your-game-id',
  adminKeypair: adminWallet,
});

// Create your game's treasury
await gamestake.initializeTreasury({
  gameNftCollection: yourCollectionMint,
  minimumActivityScore: 100,  // Weekly activity threshold
  
  // Distribution configuration - choose one:
  distributionMode: DistributionMode.GameToken,  // or Usdc, or Hybrid
  gameTokenMint: YOUR_GAME_TOKEN_MINT,           // Required for token mode
  tokenPercentage: 100,                          // For hybrid: % paid in token
});
```

### Distribution Modes

Choose how players receive rewards:

| Mode | Configuration | Players Receive |
|------|---------------|-----------------|
| `Usdc` | Default | USDC directly |
| `GameToken` | Requires `gameTokenMint` | Your game token (swapped via Jupiter) |
| `Hybrid` | Requires both + `tokenPercentage` | Split USDC + game token |

**Example: 70% Game Token, 30% USDC**
```typescript
await gamestake.initializeTreasury({
  gameNftCollection: yourCollectionMint,
  distributionMode: DistributionMode.Hybrid,
  gameTokenMint: YOUR_GAME_TOKEN_MINT,
  tokenPercentage: 70,  // 70% in game token, 30% in USDC
});
```

**Why use Game Token mode?**
- Creates consistent buy pressure on your token
- Aligns player incentives with game economy
- Tokens distributed are backed by real yield, not emissions

### 2. Fund Your Treasury

```typescript
// Deposit USDC to your treasury
await gamestake.deposit({
  amount: 36_500_000_000,  // $36,500 USDC (6 decimals)
  mint: USDC_MINT,
});

// Activate rewards (requires minimum TVL met)
await gamestake.activateTreasury();
```

### 3. Register Players

When a player purchases your game or mints your NFT:

```typescript
await gamestake.registerPlayer({
  playerWallet: player.publicKey,
  gameNft: playerOwnedNftMint,
  psg1Proof: optionalPsg1Verification,  // 10% bonus if verified
});
```

### 4. Report Activity

Weekly activity reporting (required for reward eligibility):

```typescript
await gamestake.reportActivity({
  playerWallet: player.publicKey,
  activityScore: 150,  // Your game's activity metric
  proof: activityProofSignature,
});
```

## Activity Score Guidelines

Your game defines what counts as activity. Examples:

| Game Type | Activity Metric | Suggested Threshold |
|-----------|-----------------|---------------------|
| RPG | Quests completed | 5/week |
| Battle Arena | Matches played | 10/week |
| Trading Card | Trades executed | 3/week |
| Open World | Hours played | 2 hours/week |

Players below threshold forfeit that week's rewards.

## PSG1 GameHub Integration

Optional but recommended. PSG1-verified players receive 10% reward bonus.

```typescript
// Check if player has PSG1 verification
const isPsg1Verified = await gamestake.checkPsg1Status(playerWallet);

// Register with PSG1 proof for bonus
await gamestake.registerPlayer({
  playerWallet: player.publicKey,
  gameNft: playerOwnedNftMint,
  psg1Proof: psg1VerificationProof,  // +10% rewards
});
```

Benefits:
- Stronger Sybil resistance
- Shared reputation with Block Stranding, Bonk Arena, etc.
- Reduced bot risk for your treasury

## Treasury Management

### Check Status

```typescript
const status = await gamestake.getTreasuryStatus();
// {
//   tvl: 365000000000,        // $365,000 USDC
//   playerSlots: 10,          // Funded player capacity
//   activePlayers: 8,        // Currently eligible
//   weeklyRewards: 21058000,  // ~$21 weekly distribution
//   isActive: true
// }
```

### Add Funds

```typescript
// Add more TVL to support more players
await gamestake.deposit({ amount: 36_500_000_000 });
// Now supports 11 players
```

### Withdraw (Emergency Only)

```typescript
// Emergency withdrawal - deactivates rewards
await gamestake.emergencyWithdraw({
  amount: fullBalance,
  reason: 'Protocol migration',
});
```

## Player Experience

### Claiming Rewards

Players claim via your game UI or directly:

```typescript
// Check claimable amount
const claimable = await gamestake.getClaimableRewards(playerWallet);
// Returns based on your distribution mode:
// USDC mode:  { token: 'USDC', amount: 3000000 }
// Token mode: { token: 'GAME', amount: 15000000 }  // Varies with token price
// Hybrid:     { usdc: 900000, gameToken: 10500000 }

// Claim to player wallet
await gamestake.claimRewards(playerWallet);
```

**Note**: In Game Token mode, the USDC value of rewards is constant (~$3/day equivalent), but the token amount varies based on current token price at distribution time.

### Reward Calculation

```
Weekly Pool = Treasury TVL × 0.03 ÷ 52
Player Share = Weekly Pool ÷ Eligible Players
```

Example with $365,000 TVL and 10 players:
```
Weekly Pool = $365,000 × 0.03 ÷ 52 = $210.58
Player Share = $210.58 ÷ 10 = $21.06/week = $3.01/day
```

## Fees

- **Protocol fee**: 0% (may change post-launch)
- **Jupiter Earn fee**: Built into APR
- **Solana transaction fees**: Standard network fees

## Security

### Required

- [ ] Audit your integration code
- [ ] Use multisig for treasury admin
- [ ] Implement activity reporting honestly
- [ ] Monitor for unusual claim patterns

### Recommended

- [ ] Integrate PSG1 verification
- [ ] Add secondary activity checks
- [ ] Rate limit claims in your UI
- [ ] Public dashboard for transparency

## Support

- GitHub Issues: [Staking_Treasury_PDA_Gaming](https://github.com/LinuxMakesMehappy/Staking_Treasury_PDA_Gaming)
- Email: LinuxMakesMeHappy@proton.me

## Example Games

Compatible with PSG1 GameHub ecosystem:
- Block Stranding
- Bonk Arena
- [Your game here]

---

*SDK and smart contracts under development. This document describes intended functionality.*

