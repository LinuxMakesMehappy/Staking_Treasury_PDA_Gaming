# GameStake Protocol Whitepaper

Version 0.1 – Concept Draft

## Abstract

GameStake enables sustainable player rewards by staking game treasury stablecoins through Jupiter Earn. This document provides mathematical proof of the model, acknowledges its limitations, and specifies implementation requirements.

## 1. Problem Statement

Web3 gaming reward models fail because they violate basic economics:

**Token Emission Models**: Print tokens → distribute to players → inflation → value collapse → players exit. This is a death spiral, not an economy.

**Play-to-Earn**: Requires constant new player inflows to pay existing players. This is a Ponzi structure with extra steps.

**High APR Promises**: 100%+ APRs require either inflation or unsustainable treasury depletion. The math doesn't work.

GameStake proposes a model constrained by real yield, not fantasy economics.

## 2. Core Model

### 2.1 The Equation

```
Daily Reward = TVL × APR ÷ 365
```

For $3/day target with 3% APR:

```
$3 = TVL × 0.03 ÷ 365
$3 × 365 = TVL × 0.03
$1,095 = TVL × 0.03
TVL = $36,500
```

**One player earning $3/day requires $36,500 in staked TVL.**

### 2.2 Scaling

The relationship is linear:

| TVL | Players | Daily/Player | Monthly/Player | Annual/Player |
|-----|---------|--------------|----------------|---------------|
| $36,500 | 1 | $3.00 | $90 | $1,095 |
| $73,000 | 2 | $3.00 | $90 | $1,095 |
| $365,000 | 10 | $3.00 | $90 | $1,095 |
| $1,000,000 | 27 | $3.00 | $90 | $1,095 |
| $3,650,000 | 100 | $3.00 | $90 | $1,095 |
| $36,500,000 | 1,000 | $3.00 | $90 | $1,095 |

### 2.3 Yield Source

Jupiter Earn (powered by Fluid) provides:
- Historical APR: 2-4% range
- Conservative estimate: 3% APR
- Asset: USDC/USDT stablecoins
- Risk profile: Low (no impermanent loss, battle-tested protocol)

## 3. Critical Analysis

### 3.1 What This Model Does NOT Provide

- **Living wage in developed nations**: $3/day is $1,095/year. This is supplemental income, not employment replacement.
- **Guaranteed returns**: APR fluctuates. 2-4% range means $2-4/day variance.
- **Risk-free rewards**: Smart contract risk, platform risk, and stablecoin risk exist.
- **Instant scale**: Growing from 1 to 1,000 players requires $36.5M TVL.

### 3.2 What This Model DOES Provide

- **Mathematical sustainability**: Rewards come from real yield, not emissions.
- **Predictable economics**: Linear TVL-to-player relationship.
- **Global accessibility**: $3/day is meaningful income in many economies.
- **Permanent rewards**: Principal is never touched; rewards continue indefinitely.

### 3.3 Yield Sensitivity Analysis

| APR | TVL for $3/day | TVL for 100 players |
|-----|----------------|---------------------|
| 2% | $54,750 | $5,475,000 |
| 3% | $36,500 | $3,650,000 |
| 4% | $27,375 | $2,737,500 |
| 5% | $21,900 | $2,190,000 |

**Risk**: If Jupiter Earn APR drops to 2%, TVL requirements increase 50%.

### 3.4 Comparison to Failed Models

| Model | Revenue Source | Sustainability | GameStake Advantage |
|-------|---------------|----------------|---------------------|
| Token Emissions | Inflation | ❌ Collapses | Real yield, no inflation |
| Play-to-Earn | New players | ❌ Ponzi dynamics | Independent of growth |
| High APR DeFi | Leverage/risk | ❌ Blowup risk | Conservative staking |
| GameStake | Staking yield | ✅ Sustainable | Principal preserved |

## 4. Architecture

### 4.1 Components

```
┌────────────────────────────────────────────────────────┐
│                    GAME TREASURY                        │
│  Each game maintains its own treasury PDA              │
│  Minimum: $36,500 to activate (1 player slot)          │
│  Configures: distribution token (USDC or game token)   │
└────────────────────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────┐
│                   JUPITER EARN                          │
│  Stablecoins staked via CPI                            │
│  ~3% APR returned as yield (USDC)                      │
└────────────────────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────┐
│              DISTRIBUTION ROUTER                        │
│  If USDC mode: direct to rewards vault                 │
│  If token mode: Jupiter Swap → game token → vault      │
└────────────────────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────┐
│                  REWARDS VAULT                          │
│  Weekly harvest of rewards (USDC or game token)        │
│  Proportional distribution to eligible players         │
└────────────────────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────┐
│                 PLAYER REGISTRY                         │
│  Verified players with proof of activity               │
│  Anti-bot measures enforced                            │
└────────────────────────────────────────────────────────┘
```

### 4.2 Distribution Modes

Games choose how to distribute staking yield to players:

| Mode | Flow | Benefits | Considerations |
|------|------|----------|----------------|
| **USDC** | Yield → Players | Universal value, no slippage | No token utility |
| **Game Token** | Yield → Jupiter Swap → Token → Players | Creates buy pressure, token utility | Slippage, token volatility |
| **Hybrid** | Split % USDC / % Token | Balanced approach | More complex |

**Token Mode Mechanics**:
1. Weekly yield harvested in USDC
2. USDC swapped to game token via Jupiter Swap
3. Game tokens distributed to eligible players
4. Creates consistent buy pressure on game token

**Critical Note**: Token mode means player rewards fluctuate with token price. A $3/day USDC yield could be worth more or less depending on token value at claim time.

### 4.3 Account Structure

```rust
pub struct GameTreasury {
    pub game_id: Pubkey,
    pub admin: Pubkey,
    pub usdc_deposited: u64,
    pub jupiter_position: Pubkey,
    pub total_distributed: u64,
    pub player_count: u32,
    pub minimum_tvl: u64,           // $36,500 default
    pub is_active: bool,
    // Distribution configuration
    pub distribution_mode: DistributionMode,
    pub game_token_mint: Option<Pubkey>,  // Required if token mode
    pub token_percentage: u8,              // 0-100, for hybrid mode
}

pub enum DistributionMode {
    Usdc,           // Pay in USDC directly
    GameToken,      // Swap all yield to game token
    Hybrid,         // Split between USDC and game token
}

pub struct Player {
    pub wallet: Pubkey,
    pub game_id: Pubkey,
    pub registered_at: i64,
    pub last_activity: i64,
    pub activity_score: u32,
    pub total_claimed: u64,
    pub psg1_verified: bool,
}
```

### 4.3 Instruction Set

```rust
// Treasury management
initialize_treasury(game_id, admin)
deposit_to_treasury(amount)
activate_treasury()              // Requires minimum TVL met

// Player management  
register_player(wallet, game_nft, activity_proof)
update_activity(wallet, proof)
verify_psg1(wallet, psg1_proof)  // Optional, increases rewards

// Distribution
harvest_rewards()                // Weekly, permissionless
distribute_to_players()          // Proportional to eligible players
claim_rewards(wallet)            // Player claims their share
```

## 5. Anti-Bot Framework

### 5.1 Eligibility Requirements

| Requirement | Purpose | Implementation |
|-------------|---------|----------------|
| Game NFT ownership | Proof of purchase | On-chain verification |
| Minimum activity | Proof of play | Game-reported metrics |
| Wallet age > 30 days | Filter fresh bots | On-chain timestamp |
| Transaction history | Sybil resistance | Activity analysis |
| PSG1 verification | Identity layer | Optional, bonus rewards |

### 5.2 Activity Proof

Games must report player activity. Minimum thresholds:
- Weekly active sessions
- On-chain transactions within game
- Achievement/progression data

Players below threshold forfeit that week's rewards (redistributed to active players).

### 5.3 PSG1 GameHub Integration

Optional but incentivized:
- PSG1-verified players receive 10% reward bonus
- Leverages existing Solana gaming identity infrastructure
- Provides stronger Sybil resistance
- Shared reputation across games (Block Stranding, Bonk Arena, etc.)

## 6. Economic Scenarios

### 6.1 Launch Scenario (Conservative)

**Assumptions**:
- Initial TVL: $365,000
- APR: 3%
- Players: 10

**Results**:
```
Annual yield: $365,000 × 0.03 = $10,950
Weekly yield: $10,950 ÷ 52 = $210.58
Per player weekly: $210.58 ÷ 10 = $21.06
Per player daily: $21.06 ÷ 7 = $3.01 ✓
```

### 6.2 Growth Scenario

**Year 1**: $365,000 TVL → 10 players
**Year 2**: $1,000,000 TVL → 27 players  
**Year 3**: $3,650,000 TVL → 100 players

Growth comes from:
- Additional treasury deposits from game revenue
- Compounding portion of rewards (optional)
- New game integrations

### 6.3 Stress Scenario (APR Drop)

**If APR drops to 2%**:
```
$365,000 × 0.02 ÷ 365 = $20/day total
$20 ÷ 10 players = $2/day per player
```

**Mitigation**: Reduce player slots or accept lower per-player rewards. Principal remains intact.

### 6.4 Failure Scenario

**Complete yield failure (0% APR)**:
- No rewards distributed
- Principal remains intact
- Players receive nothing but lose nothing
- Treasury can withdraw and redeploy

This is the worst case. Compare to token emission models where failure means total value loss.

## 7. Implementation Requirements

### 7.1 Smart Contracts (Solana/Anchor)

- Treasury PDA with Jupiter Earn CPI
- Player registry with activity tracking
- Weekly distribution logic
- Admin controls with timelock

### 7.2 Off-Chain Components

- Activity oracle for game metrics
- Dashboard for transparency
- SDK for game integration

### 7.3 Security Requirements

- Audit before mainnet
- Multisig for treasury admin
- Emergency pause capability
- No upgrade authority (immutable after audit)

## 8. Limitations and Risks

### 8.1 Acknowledged Limitations

1. **Scale requires capital**: 1,000 players needs $36.5M TVL
2. **Yield dependency**: Model fails if Jupiter Earn fails
3. **Not a salary**: $3/day is supplemental, not replacement income
4. **Bot risk**: Anti-bot measures add friction and aren't perfect
5. **Regulatory uncertainty**: Gaming rewards may face scrutiny

### 8.2 Risk Matrix

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| APR drops below 2% | Medium | Medium | Reduce player slots |
| Jupiter Earn failure | Low | High | Emergency withdrawal |
| Smart contract bug | Low | High | Audit, bug bounty |
| Bot attack | Medium | Medium | Multi-layer verification |
| Regulatory action | Low | High | Legal review, compliance |

## 9. Conclusion

GameStake is not a revolutionary new economic model. It's the application of basic yield farming to gaming rewards with honest math and conservative assumptions.

**What we're proposing**:
- Real yield from Jupiter Earn powers player rewards
- $36,500 TVL = 1 player at $3/day, forever
- No tokens, no emissions, no Ponzi dynamics
- Sustainable, verifiable, transparent

**What we're NOT proposing**:
- Get-rich-quick scheme
- Replacement for employment
- Risk-free returns
- Magic economics

The math works. The question is whether games will fund treasuries and players will accept $3/day as meaningful. In much of the world, they will.

---

*GameStake Protocol – Concept Draft v0.1*
*Contact: LinuxMakesMeHappy@proton.me*

