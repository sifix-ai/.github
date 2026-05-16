<div align="center">

# SIFIX

### Intercept-First Web3 Security on 0G

**SIFIX is a Web3 security layer that intercepts wallet requests before signature, analyzes transaction risk with 0G Compute, stores verifiable records with 0G Storage, and uses 0G Agentic ID plus on-chain contracts for transparent security signals.**

[![0G Galileo](https://img.shields.io/badge/Network-0G%20Galileo%20Testnet-blue)](https://chainscan-galileo.0g.ai)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.4-blue.svg)](https://www.typescriptlang.org/)

[Architecture](#-architecture) · [How It Works](#-how-it-works) · [Repositories](#-repositories) · [Quick Start](#-quick-start) · [Tech Stack](#-tech-stack)

</div>

---

## The Problem

Web3 users lose billions annually to:
- **Phishing sites** mimicking legitimate dApps
- **Malicious smart contracts** with hidden backdoors
- **Approval scams** draining wallets silently
- **Rug pulls** from seemingly legitimate projects
- **Complex DeFi interactions** with hidden risks

Traditional security tools are reactive — they warn after the damage is done. **SIFIX is proactive**: it intercepts transactions *before* they're signed, simulates execution, explains risks in plain language, and gives the user a clear choice to analyze, continue, or block.

---

## The Solution

SIFIX adds a **security decision layer** between the user and the blockchain:

```
1. INTERCEPT  → Catch wallet requests before signature (browser extension)
2. SIMULATE   → Run transaction in safe sandbox (viem on 0G Galileo)
3. ANALYZE    → AI evaluates risks with historical context (0G Compute)
4. STORE      → Save verifiable analysis records (0G Storage)
5. VERIFY     → Use 0G Agentic ID and on-chain SIFIX contracts for transparent identity and security state
6. PROTECT    → User sees clear risk explanation + recommendation
```

---

## Architecture

```mermaid
graph TB
    subgraph BROWSER["USER'S BROWSER"]
        DAPP_UI[Any dApp<br/>Uniswap, Aave...]
        EXT_UI[SIFIX Extension<br/>Chrome MV3]
        MM[MetaMask / Web3 Wallet]
        DAPP_UI -->|TX request| EXT_UI
        EXT_UI -->|intercepted TX| MM
    end

    subgraph SIFIX["SIFIX dApp (Next.js 16)"]
        DASH[Dashboard<br/>Setup, history, reputation]
        API[API Routes<br/>analysis, auth, sync]
        DB[(Prisma Database<br/>app + scan history)]
        AGENT["@sifix/agent SDK"]
        DASH --> API
        API --> DB
        API --> AGENT
    end

    subgraph ZG["0G Galileo Infrastructure"]
        CHAIN[0G Chain<br/>SIFIX Contract]
        COMPUTE[0G Compute<br/>AI Inference]
        STORAGE[0G Storage<br/>Evidence]
        AGID[0G Agentic ID<br/>Identity]
    end

    EXT_UI -->|POST /api/v1/analyze| API
    MM -->|sign TX| CHAIN
    AGENT --> COMPUTE
    AGENT --> STORAGE
    AGENT --> CHAIN
    API --> AGID

    style BROWSER fill:#1a1a2e,color:#fff
    style SIFIX fill:#16213e,color:#fff
    style ZG fill:#0f3460,color:#fff
    style EXT_UI fill:#3b9eff,color:#fff
    style AGENT fill:#a855f7,color:#fff
    style COMPUTE fill:#22c55e,color:#fff
    style STORAGE fill:#f59e0b,color:#000
```

<details>
<summary>📐 ASCII Version</summary>

```
┌─────────────────────────────────────────────────────────────────────┐
│                        USER'S BROWSER                                │
│                                                                      │
│  ┌──────────────┐     ┌──────────────────┐     ┌────────────────┐   │
│  │  Any dApp     │────▶│  SIFIX Extension │────▶│   MetaMask /   │   │
│  │  (Uniswap...) │     │  (Chrome MV3)    │     │   Web3 Wallet  │   │
│  └──────────────┘     │                  │     └───────┬────────┘   │
│                       │ • TX Interceptor │             │             │
│                       │ • Domain Scanner │             │             │
│                       │ • Shield Badge   │             │             │
│                       └────────┬─────────┘             │             │
│                                │                       │             │
└────────────────────────────────┼───────────────────────┼─────────────┘
                                 │                       │
                    ┌────────────▼───────────────────────▼────────┐
                    │            SIFIX dApp (Next.js 16)           │
                    │                                              │
                    │  ┌────────────┐  ┌──────────┐  ┌─────────┐  │
                    │  │ Dashboard  │  │  35 API   │  │ Prisma  │  │
                    │  │ (12 pages) │  │  Routes   │  │ SQLite  │  │
                    │  └────────────┘  └─────┬─────┘  └────┬────┘  │
                    │                        │              │        │
                    │  ┌─────────────────────▼──────────────▼────┐  │
                    │  │         @sifix/agent (SDK)               │  │
                    │  │                                         │  │
                    │  │  ┌──────────┐  ┌──────────┐  ┌──────┐  │  │
                    │  │  │ Simulator│  │ AI Analyz │  │Storage│  │  │
                    │  │  │ (viem)   │  │ (0G/LLM) │  │Client │  │  │
                    │  │  └──────────┘  └──────────┘  └──────┘  │  │
                    │  │                                         │  │
                    │  │  ┌──────────────┐  ┌────────────────┐   │  │
                    │  │  │ ComputeClient│  │ ThreatIntel    │   │  │
                    │  │  │ (0G Compute) │  │ (Prisma)       │   │  │
                    │  │  └──────────────┘  └────────────────┘   │  │
                    │  └─────────────────────────────────────────┘  │
                    └──────────────────────┬────────────────────────┘
                                           │
                    ┌──────────────────────▼────────────────────────┐
                    │           0G Galileo Infrastructure            │
                    │                                               │
                    │  ┌──────────┐  ┌──────────┐  ┌────────────┐  │
                    │  │ 0G Chain │  │ 0G Compute│  │ 0G Storage │  │
                    │  │ (EVM)    │  │ (AI Inf.) │  │ (Evidence) │  │
                    │  └──────────┘  └──────────┘  └────────────┘  │
                    └───────────────────────────────────────────────┘
```
</details>

---

## How It Works — Detailed

### End-to-End Transaction Security

```mermaid
flowchart LR
    subgraph STEP1["1. INTERCEPT"]
        A1[User initiates TX<br/>on any dApp] --> A2[Extension intercepts<br/>before signing]
    end

    subgraph STEP2["2. SIMULATE"]
        B1[TransactionSimulator<br/>viem + 0G Galileo] --> B2[SimulationResult<br/>gas, balances, events]
    end

    subgraph STEP3["3. ANALYZE"]
        C1[Fetch Threat Intel<br/>historical context] --> C2[AI Risk Analysis<br/>0G Compute / LLM] --> C3[RiskScore 0-100<br/>BLOCK / WARN / ALLOW]
    end

    subgraph STEP4["4. STORE"]
        D1[0G Storage<br/>verifiable evidence] --> D2[rootHash<br/>proof reference]
    end

    subgraph STEP5["5. VERIFY + LEARN"]
        E1[0G Agentic ID + SIFIX Contract<br/>identity and security state] --> E2[Save to DB<br/>richer context for next scan]
    end

    STEP1 --> STEP2 --> STEP3 --> STEP4 --> STEP5

    style STEP1 fill:#22c55e,color:#fff
    style STEP2 fill:#3b9eff,color:#fff
    style STEP3 fill:#a855f7,color:#fff
    style STEP4 fill:#f59e0b,color:#000
    style STEP5 fill:#ef4444,color:#fff
```

### AI Learning Loop

```mermaid
flowchart TD
    SCAN([Scan Transaction]) --> ANALYZE[AI Analyze<br/>with historical context]
    ANALYZE --> STORE_0G[Store to 0G Storage<br/>immutable proof]
    STORE_0G --> SAVE_DB[Save to local DB<br/>ScanHistory table]
    SAVE_DB --> NEXT([Next scan of same address<br/>→ richer context → better analysis])

    NEXT -.->|feedback loop| SCAN

    style SCAN fill:#3b9eff,color:#fff
    style ANALYZE fill:#a855f7,color:#fff
    style STORE_0G fill:#f59e0b,color:#000
    style SAVE_DB fill:#22c55e,color:#fff
    style NEXT fill:#ef4444,color:#fff
```

### Step 1: Transaction Interception

When a user initiates any blockchain transaction (swap, transfer, approve, etc.):

1. The **SIFIX Extension** injects a `tx-interceptor.js` script into the page's `MAIN` world
2. This script wraps `window.ethereum.request()` with a proxy
3. When `eth_sendTransaction` or `eth_signTransaction` is detected, it's intercepted before reaching MetaMask
4. The transaction data (`from`, `to`, `value`, `data`) is sent to the background service worker
5. Background calls `POST /api/v1/analyze` on the SIFIX dApp with the transaction parameters

### Step 2: Transaction Simulation

The `@sifix/agent` SDK's `TransactionSimulator` (powered by viem):

1. Constructs a `publicClient.call()` to the 0G Galileo RPC
2. Simulates the transaction against the latest block state
3. Captures: success/failure, gas usage, balance changes, events, revert reasons
4. Returns a `SimulationResult` — no actual state change on-chain

### Step 3: Threat Intelligence Lookup

Before AI analysis, the agent fetches **historical context**:

1. `PrismaThreatIntel.getAddressIntel(address)` queries the local SQLite database
2. Looks up the last 50 scans involving this address
3. Aggregates: average risk score, max risk, known threats, risk distribution, recent scan history
4. This historical data is fed to the AI as context — so it **learns from past scans**

### Step 4: AI Risk Analysis

The `AIAnalyzer` sends a structured prompt to the AI containing:
- Transaction details (from, to, value, calldata)
- Simulation results (gas, balance changes, revert reason)
- Historical threat intel (past scans, risk patterns)

**AI Providers (priority order):**
1. **0G Compute** — Decentralized AI inference on 0G network (recommended)
2. **OpenAI-compatible** — Fallback: OpenAI, Groq, OpenRouter, Ollama, Together AI, etc.

Returns:
- `riskScore` (0-100) — numerical risk assessment
- `confidence` (0-1) — how confident the AI is
- `reasoning` — human-readable explanation of risks
- `threats[]` — specific threats identified
- `recommendation` — BLOCK / WARN / ALLOW

### Step 5: Evidence Storage

Every analysis is **permanently stored** on 0G Storage:

1. Analysis JSON is written to a temp file
2. A Merkle tree is generated from the data
3. The file is uploaded to 0G Storage via the indexer
4. Returns an immutable `rootHash` that can be verified on the explorer
5. Anyone can download and verify the evidence via `/api/v1/storage/[hash]/download`

### Step 6: Learning Loop

After each analysis, the result is saved to the local database:

1. `PrismaThreatIntel.saveScanResult()` stores the scan in `ScanHistory` table
2. Next time the same address is scanned, the AI gets richer historical context
3. Over time, the agent builds a **profile** for each address: risk trends, known threats, behavior patterns
4. This makes each subsequent analysis more informed than the last

### Domain Safety (Background)

Independently from transaction interception, the extension **auto-scans every website** the user visits:

1. User navigates to a URL → `chrome.tabs.onUpdated` fires
2. Background checks: local cache → local scam blacklist → local safe list → SIFIX API → GoPlus API
3. Badge color updates: green (safe), amber (warning), red (danger)
4. Floating shield overlay appears on the page showing the safety status
5. Results are cached per session to avoid redundant API calls

---

## Repositories

| Repository | Description | Version |
|---|---|---|
| [sifix-agent](https://github.com/sifix-ai/sifix-agent) | AI Security Agent SDK — simulation, analysis, 0G Compute, 0G Storage, threat intel | Active |
| [sifix-dapp](https://github.com/sifix-ai/sifix-dapp) | Web dashboard + API backend — auth, analysis, history, reputation, sync | Active |
| [sifix-extension](https://github.com/sifix-ai/sifix-extension) | Chrome extension — wallet interception, domain scanning, shield overlay | Active |
| [sifix-contracts](https://github.com/sifix-ai/sifix-contracts) | Smart contracts — on-chain reporting and transparent security state on 0G | Active |
| [sifix-indexer](https://github.com/sifix-ai/sifix-indexer) | Event indexing and reconciliation for SIFIX on-chain activity | Active |
| [sifix-docs](https://github.com/sifix-ai/sifix-docs) | Public documentation site for architecture, APIs, and guides | Active |

---

## Repositories Deep Dive

### @sifix/agent — AI Security Agent SDK

The brain of SIFIX. A TypeScript SDK that orchestrates the entire security pipeline.

**Key Components:**
- `SecurityAgent` — Main orchestrator class
- `TransactionSimulator` — Simulates TX via viem on 0G Galileo
- `AIAnalyzer` — Routes to 0G Compute or OpenAI-compatible providers
- `StorageClient` — Uploads evidence to 0G Storage
- `ThreatIntelProvider` — Pluggable interface for historical context

```typescript
import { SecurityAgent } from '@sifix/agent';

const agent = new SecurityAgent({
  rpcUrl: 'https://evmrpc-testnet.0g.ai',
  compute: { privateKey, providerAddress },
  storage: { indexerUrl, privateKey },
  threatIntel: new PrismaThreatIntel(),
});

const result = await agent.analyzeTransaction({
  from: '0x...', to: '0x...', data: '0x...', value: 0n,
});
// result.analysis.riskScore → 0-100
// result.analysis.recommendation → BLOCK | WARN | ALLOW
// result.storageRootHash → 0G Storage proof
```

See [sifix-agent/README.md](https://github.com/sifix-ai/sifix-agent/blob/master/README.md) for full API reference.

---

### sifix-dapp — Web Dashboard + API Backend

The control center. Built on Next.js 16 with App Router.

**Core capabilities:**
- Wallet request analysis via `/api/v1/analyze`
- Dashboard flows for setup, history, reputation, tags, watchlist, and extension activation
- Auth and Agentic ID integration
- Threat intelligence persistence and storage proof retrieval
- Sync and reconcile flows for on-chain reporting state

See [sifix-dapp/README.md](https://github.com/sifix-ai/sifix-dapp/blob/master/README.md) for setup guide and full documentation.

---

### sifix-extension — Chrome Extension

The user-facing protector. Minimal and non-intrusive.

**What it does:**
- Auto-scans every website for phishing/scam indicators
- Intercepts wallet transactions before signing
- Shows floating shield badge with safety status
- Connects to dApp via SIWE (Sign-In with Ethereum) for API access

**Architecture:**
- Background Service Worker — domain scanning, TX analysis orchestration
- Content Scripts — TX interceptor (MAIN world), shield badge, warning banners
- Popup (340x440) — connect/disconnect + protection toggle
- Local IndexedDB — transaction history cache

**Multi-Layer Domain Safety:**
1. Session cache → 2. Local scam blacklist → 3. Local safe list → 4. SIFIX API → 5. GoPlus fallback

See [sifix-extension/README.md](https://github.com/sifix-ai/sifix-extension/blob/master/README.md) for build and installation instructions.

---

## Quick Start

### Prerequisites

- Node.js 18+
- pnpm (recommended) or npm
- MetaMask or compatible Web3 wallet
- 0G Galileo Testnet configured in wallet

### 1. Clone All Repos

```bash
git clone https://github.com/sifix-ai/sifix-agent.git
git clone https://github.com/sifix-ai/sifix-dapp.git
git clone https://github.com/sifix-ai/sifix-extension.git
```

### 2. Setup Agent SDK

```bash
cd sifix-agent
pnpm install
pnpm build
```

### 3. Setup dApp

```bash
cd sifix-dapp
pnpm install
cp .env.example .env
# Edit .env with your configuration
pnpm db:push
pnpm dev
```

### 4. Setup Extension

```bash
cd sifix-extension
pnpm install
pnpm build
# Load build/chrome-mv3-prod/ in Chrome as unpacked extension
```

### 5. Connect

1. Open the SIFIX dApp in your browser
2. Connect your wallet (0G Galileo Testnet)
3. The extension auto-activates — badge appears on all sites

---

## Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend | Next.js 16, React 19, TailwindCSS 4 | Dashboard & marketing site |
| Wallet | Wagmi v3, Viem v2 | Blockchain interactions |
| Extension | Plasmo 0.88, Chrome MV3 | Browser extension framework |
| Agent SDK | TypeScript, OpenAI SDK | AI security analysis pipeline |
| AI Inference | 0G Compute, OpenAI, Groq, Ollama | Risk analysis providers |
| Database | SQLite via Prisma | Local threat intelligence |
| Simulation | Viem public client | Transaction simulation |
| Storage | 0G Storage (0g-storage-ts-sdk) | Immutable evidence storage |
| Compute | 0G Compute (0g-compute-ts-sdk) | Decentralized AI inference |
| Blockchain | 0G Galileo Testnet (Chain ID: 16602) | Settlement & reputation |
| Identity | ERC-7857 (Agentic ID) | Agent identity on-chain |
| Local Storage | Dexie (IndexedDB) | Extension TX cache |

---

## Risk Scoring

| Score | Level | Recommendation | Description |
|---|---|---|---|
| 0-19 | SAFE | ALLOW | No significant risks detected |
| 20-39 | LOW | ALLOW | Minor concerns, generally safe |
| 40-59 | MEDIUM | WARN | Moderate risks, review recommended |
| 60-79 | HIGH | WARN | Significant threats, proceed with caution |
| 80-100 | CRITICAL | BLOCK | Severe threats, transaction should be blocked |

---

## AI Learning System

SIFIX's AI gets smarter over time through a **learning loop**:

```
Scan TX → AI Analyze (with historical context)
                ↓
         Store to 0G Storage (immutable proof)
                ↓
         Save to local DB (ScanHistory table)
                ↓
         Next scan of same address → richer context → better analysis
```

**What the AI learns:**
- Risk score trends for each address (improving, stable, or deteriorating)
- Known threat patterns (specific scam types associated with an address)
- Risk distribution (how often SAFE vs HIGH vs CRITICAL)
- Community feedback (tags, votes, reports)

**This means:**
- First scan of a new address → baseline analysis
- 10th scan → AI knows the address's history and patterns
- 100th scan → AI has a detailed risk profile and can detect anomalies

---

## Environment Variables

### dApp (.env)

```env
# Database
DATABASE_URL="postgresql://user:password@127.0.0.1:5432/sifix"

# 0G Galileo Chain
NEXT_PUBLIC_ZG_RPC_URL="https://evmrpc-testnet.0g.ai"
NEXT_PUBLIC_ZG_CHAIN_ID="16602"

# WalletConnect
NEXT_PUBLIC_WALLETCONNECT_PROJECT_ID=""

# AI Provider (fallback when 0G Compute not configured)
AI_API_KEY=""
AI_BASE_URL=""
AI_MODEL=""

# 0G Storage (server-side wallet)
ZG_INDEXER_URL="https://indexer-storage-testnet-turbo.0g.ai"
STORAGE_PRIVATE_KEY=""
STORAGE_MOCK_MODE="true"

# 0G Compute (decentralized AI inference)
COMPUTE_PRIVATE_KEY=""
COMPUTE_PROVIDER_ADDRESS=""

# Agentic ID (ERC-7857)
NEXT_PUBLIC_AGENTIC_ID_CONTRACT_ADDRESS="0x2700F6A3e505402C9daB154C5c6ab9cAEC98EF1F"
NEXT_PUBLIC_AGENTIC_ID_TOKEN_ID=""

# SIFIX Contract (0G Galileo)
NEXT_PUBLIC_SIFIX_CONTRACT_ADDRESS="0xBBa8b030D80113e50271a2bbEeDBE109D9f1C42e"
```

---

## Network Information

| Parameter | Value |
|---|---|
| Network | 0G Galileo Testnet |
| Chain ID | 16602 |
| RPC | https://evmrpc-testnet.0g.ai |
| Explorer | https://chainscan-galileo.0g.ai |
| Storage Explorer | https://storage-testnet.0g.ai |
| Storage Indexer | https://indexer-storage-testnet-turbo.0g.ai |
| SIFIX Contract | 0xBBa8b030D80113e50271a2bbEeDBE109D9f1C42e |
| SIFIX Contract Explorer | https://chainscan-galileo.0g.ai/address/0xbba8b030d80113e50271a2bbeedbe109d9f1c42e |
| Agentic ID Contract | 0x2700F6A3e505402C9daB154C5c6ab9cAEC98EF1F |
| Agentic ID NFT | https://chainscan-galileo.0g.ai/nft/0x2700F6A3e505402C9daB154C5c6ab9cAEC98EF1F/99 |

---

## Project Structure

```
sifix-ai/
├── sifix-agent/          # AI Security Agent SDK
│   ├── src/
│   │   ├── index.ts              # SecurityAgent orchestrator
│   │   ├── ai/analyzer.ts        # AI risk analysis
│   │   ├── compute/client.ts     # 0G Compute client
│   │   ├── core/simulator.ts     # Transaction simulator
│   │   ├── storage/client.ts     # 0G Storage client
│   │   └── threat-intel/         # Threat intel interface
│   └── package.json
│
├── sifix-dapp/           # Web Dashboard + API
│   ├── app/
│   │   ├── api/v1/              # 35 API routes
│   │   ├── dashboard/           # 12 dashboard pages
│   │   └── page.tsx             # Landing page
│   ├── components/              # UI components
│   ├── lib/                     # Libraries & helpers
│   ├── prisma/                  # Database schema
│   └── package.json
│
└── sifix-extension/      # Chrome Extension
    ├── src/
    │   ├── background/          # Service worker
    │   ├── contents/            # Content scripts
    │   ├── components/          # UI components
    │   ├── hooks/               # React hooks
    │   ├── lib/                 # Utilities
    │   └── popup.tsx            # Extension popup
    └── package.json
```

---

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## License

All SIFIX repositories are licensed under the **MIT License**.

---

<div align="center">

**Built on 0G Chain, 0G Compute, 0G Storage, and 0G Agentic ID**

[Website](https://sifix.ai) · [GitHub](https://github.com/sifix-ai) · [0G Chain](https://0g.ai)

</div>
