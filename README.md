<div align="center">

<img src="https://img.shields.io/badge/SOLANA-100000?style=for-the-badge&logo=solana&logoColor=white&labelColor=9945FF&color=14F195" alt="Solana" />
<img src="https://img.shields.io/badge/METEORA-100000?style=for-the-badge&labelColor=0B4650&color=0B4650&logoColor=E6FF2B" alt="Meteora" />

# ⚡ Clod

**Token launchpad on Solana. Powered by Meteora Dynamic Bonding Curve.**

[![Website](https://img.shields.io/badge/clod.fun-F9F7F2?style=flat-square&logo=google-chrome&logoColor=0B4650)](https://clod.fun)
[![X](https://img.shields.io/badge/@getclod-000?style=flat-square&logo=x&logoColor=white)](https://x.com/getclod)
[![Docs](https://img.shields.io/badge/Docs-0B4650?style=flat-square&logo=gitbook&logoColor=white)](https://clod.fun/docs)
[![npm](https://img.shields.io/badge/npm-clod--sdk-CB3837?style=flat-square&logo=npm&logoColor=white)](https://www.npmjs.com/package/clod-sdk)

</div>

---

### Quick Start

```bash
npm install clod-sdk @solana/web3.js
```

```typescript
import { Clod } from 'clod-sdk';
import { Connection, Keypair } from '@solana/web3.js';

const clod = new Clod({
  connection: new Connection('https://api.mainnet-beta.solana.com'),
  wallet: yourWalletAdapter,
});

// Deploy a token in one call
const { mint, pool, signature } = await clod.deploy({
  name: 'MyToken',
  symbol: 'MTK',
  image: './token-logo.png',
});

console.log(`Token deployed: ${mint.toBase58()}`);
console.log(`Pool live on Meteora: ${pool.toBase58()}`);
```

---

### SDK Features

```typescript
// Deploy token with locked liquidity on Meteora DBC
const token = await clod.deploy({ name, symbol, image });

// Get real-time market data
const data = await clod.getMarketData(mintAddress);
// → { mcap, price, volume24h, liquidity, bondingProgress }

// Execute trades through the bonding curve
const tx = await clod.buy(mintAddress, solAmount);
const tx = await clod.sell(mintAddress, tokenAmount);

// Claim creator fees
const fees = await clod.getClaimableFees(walletAddress);
const tx = await clod.claimFees(mintAddress);

// Listen to real-time trades
clod.onTrade(mintAddress, (trade) => {
  console.log(`${trade.type} ${trade.tokenAmount} for ${trade.solAmount} SOL`);
});
```

---

### Architecture

```
┌──────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   clod-sdk   │────▶│   Clod Backend   │────▶│  Solana + SPL   │
│              │     │                  │     │  Token 2022     │
│  TypeScript  │     │  REST API        │     └────────┬────────┘
│  npm package │     │  WebSocket       │              │
└──────────────┘     └──────────────────┘     ┌────────▼────────┐
                                              │ Meteora Dynamic │
                                              │ Bonding Curve   │
                                              │                 │
                                              │ Locked LP       │
                                              │ 2% Creator Fee  │
                                              └─────────────────┘
```

---

### API Reference

| Method | Description |
|--------|-------------|
| `clod.deploy(opts)` | Deploy token with auto-created Meteora DBC pool |
| `clod.buy(mint, sol)` | Buy tokens through the bonding curve |
| `clod.sell(mint, tokens)` | Sell tokens back to the pool |
| `clod.getMarketData(mint)` | Fetch price, mcap, volume, liquidity |
| `clod.getClaimableFees(wallet)` | Check unclaimed creator fees |
| `clod.claimFees(mint)` | Claim accumulated trading fees |
| `clod.listTokens(opts)` | List all deployed tokens with filters |
| `clod.onTrade(mint, cb)` | Real-time trade event listener |

---

### How fees work

```
Every trade → 2% fee → 100% to creator wallet

No staking. No lockups. No claiming delays.
Fees arrive in your wallet automatically.
```

---

### Repos

| Package | Description | Status |
|---------|-------------|--------|
| [`clod-sdk`](https://github.com/getclod/clod-sdk) | TypeScript SDK for Clod protocol | 🔨 In development |
| [`clod-examples`](https://github.com/getclod/clod-examples) | Example bots, scripts & integrations | 📋 Planned |

---

<div align="center">

**[clod.fun](https://clod.fun)** · [Docs](https://clod.fun/docs) · [X](https://x.com/getclod)

<sub>Built on Solana · Powered by Meteora</sub>

</div>
