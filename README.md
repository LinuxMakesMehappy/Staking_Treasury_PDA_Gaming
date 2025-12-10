# GameStake Protocol

Sustainable player rewards through Jupiter Earn staking yields.

## The Problem

Web3 gaming economics are broken. Token emissions inflate to zero. Play-to-earn collapses. Players get burned. The math never worked.

## The Solution

Lock stablecoins. Stake for real yield. Pay players forever.

```
$36,500 TVL × 3% APR ÷ 365 = $3/day per player
```

That's it. No tokens. No emissions. No complexity.

## Why $3/Day

$3/day is the minimum wage in India, Philippines, Indonesia, and much of the developing world. This isn't about making players rich—it's about making Web3 gaming accessible to billions.

**One funded slot = $36,500 TVL = $3/day forever.**

## How It Works

1. Games deposit stablecoins to their GameStake treasury
2. Treasury stakes in Jupiter Earn (~3% APR)
3. Weekly yield harvested
4. **Games choose distribution token**: USDC or their native game token
5. Players prove activity, earn proportional share

## Distribution Options

| Mode | How It Works | Use Case |
|------|--------------|----------|
| **USDC** | Yield paid directly in USDC | Universal value, simple |
| **Game Token** | Yield swapped via Jupiter to game token | Token utility, ecosystem alignment |
| **Hybrid** | Split between USDC and game token | Balanced approach |

Games configure their distribution preference. Jupiter Swap handles conversion automatically.

## The Math

| TVL | Funded Players | Daily Each | Monthly Each |
|-----|----------------|------------|--------------|
| $36,500 | 1 | $3 | $90 |
| $365,000 | 10 | $3 | $90 |
| $1,000,000 | 27 | $3 | $90 |
| $3,650,000 | 100 | $3 | $90 |

Linear. Predictable. Verifiable.

## Anti-Bot Measures

- PSG1 GameHub identity verification (optional, incentivized)
- Game NFT ownership required
- Minimum on-chain activity threshold
- Wallet history validation
- Weekly claim cadence only

## For Games

Minimum $36,500 TVL to activate. One integration call:

```typescript
await gamestake.registerPlayer(wallet, gameId, activityProof);
```

See [INTEGRATION.md](INTEGRATION.md) for details.

## For Jupiter

This model depends entirely on Jupiter Earn. We're proposing a gaming vertical that:
- Brings consistent TVL from gaming treasuries
- Showcases Jupiter Earn utility beyond DeFi
- Creates sustainable player rewards without token emissions
- Provides clean, auditable economics

See [WHITEPAPER.md](WHITEPAPER.md) for full specification.

## Status

**Concept stage. Seeking funding and developers.**

- [WHITEPAPER.md](WHITEPAPER.md) – Protocol specification
- [INTEGRATION.md](INTEGRATION.md) – Game developer guide  
- [FUNDING.md](FUNDING.md) – Investment proposal

## Contact

LinuxMakesMeHappy@proton.me

## License

MIT
