# Solana Private Transaction Tool

> A privacy-focused transaction platform built on Solana that breaks on-chain links between sender and recipient.

---

## Overview

This tool enables private token transfers on Solana by routing transactions through intermediary wallets, token swaps, and cross-chain bridges — making it significantly harder to trace the origin and destination of funds through on-chain analysis.

## How It Works

Transactions are routed through **shadow wallets** — temporary keypairs generated client-side that act as intermediaries. Funds flow from the sender to a shadow wallet, are optionally swapped or bridged, and then forwarded to the final recipient. The on-chain trail between sender and recipient is broken at each hop.

### Privacy Levels

| Level | Method | Privacy Rating |
|-------|--------|---------------|
| **Enhanced** | Shadow wallet + DEX swap (different token out) | ⭐⭐⭐ |
| **Maximum** | Shadow wallet + cross-chain bridge (exits and re-enters Solana) | ⭐⭐⭐⭐⭐ |
| **Maximum+** | Double bridge — two sequential cross-chain hops with separate shadow wallets | ⭐⭐⭐⭐⭐+ |

### Transaction Flow

```
Enhanced:
  Sender → Shadow Wallet → DEX Swap → Recipient

Maximum:
  Sender → Shadow Wallet → Bridge Out → Bridge In → Recipient

Maximum+ (Double Bridge):
  Sender → Shadow₁ → Bridge Out → Bridge In → Shadow₂ → Bridge Out → Bridge In → Recipient
```

## Features

- **Multi-token support** — SOL, USDC, USDT, and other SPL tokens
- **Batch mode** — Send to multiple recipients in one session with a single wallet approval
- **Cross-chain bridge** — Exits Solana and re-enters to break on-chain links
- **Same-token routes** — Stablecoins bridge out and return as the same token
- **Auto-splitting** — Large amounts are split across multiple shadow wallets
- **RPC resilience** — Automatic failover across multiple RPC endpoints
- **MEV protection** — Transaction bundling to prevent front-running
- **Recovery system** — Stuck funds in shadow wallets can be recovered
- **Single approval** — All transactions consolidated into one wallet popup

## Supported Tokens

| Token | Enhanced | Maximum | Maximum+ |
|-------|----------|---------|----------|
| SOL | ✅ | ✅ | ✅ |
| USDC | ✅ | ✅ | ✅ |
| USDT | ✅ | ✅ | ✅ |

## Architecture

```
Frontend (Vanilla JS)
├── Main app logic & UI
├── Privacy engine (Enhanced mode — DEX routing)
├── Maximum engine (Bridge routing + double bridge)
├── Bridge module (Cross-chain API integration)
├── DEX swap module (token aggregator)
├── RPC resilience layer (multi-endpoint failover)
└── Visual effects (particles, glassmorphism, 3D tilt)

Backend (Node.js)
└── Lightweight proxy server for API calls
```

### Key Design Decisions

- **Client-side key generation** — Shadow wallet keypairs are generated in the browser and never leave the client
- **No backend state** — The server is a stateless proxy; all transaction logic runs client-side
- **Automatic cleanup** — Shadow wallets are emptied and discarded after each transaction
- **Fee system** — 1% fee on all transactions sent to a designated fee wallet

## UI

Exchange-style interface with:
- Glassmorphism cards with backdrop blur
- Particle background with floating connections
- 3D tilt effect on interactive cards
- Three main tabs: Send Privately, Bridge, Batch
- Consistent table-based layouts across all tabs
- Real-time balance display and USD estimates

## Privacy Considerations

- Shadow wallets are **ephemeral** — generated per transaction, not reused
- Cross-chain bridges break the direct Solana→Solana link entirely
- Double bridge (Maximum+) adds a second hop through an additional shadow wallet
- Amount splitting distributes funds across multiple intermediaries
- No transaction history is stored on the server or client

## Requirements

- Node.js 18+
- Phantom or Backpack wallet (browser extension)
- SOL for transaction fees and rent

## Status

Active development — see [CHANGELOG.md](./CHANGELOG.md) for full version history.
