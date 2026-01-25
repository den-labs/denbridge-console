# DenBridge Console

DenBridge Console is a testnet-first, cross-chain payroll stack that moves USDC from EVM to Stacks and executes batch payouts with real transaction receipts. It’s built for teams that need transparent treasury funding, fast payroll runs, and verifiable on-chain reporting.

## What’s inside (3 projects)

- **Web Console (Next.js)**: the operator dashboard and 3‑stage wizard for wallet setup, funding, and payroll execution.
- **Relayer (Node)**: a lightweight status service that validates EVM confirmations and detects Stacks USDCx mints via Hiro API, powering real-time progress and receipts.
- **Contracts (Clarinet)**: batch payout smart contract(s) for Stacks, emitting structured events for receipt parsing.

## Why it matters

- **Cross-chain clarity**: track funding from EVM to Stacks with explicit, verifiable status.
- **No custody**: transfers are user‑signed; the system doesn’t hold funds.
- **Operational speed**: upload CSVs, validate, execute, and export receipts in minutes.

---

# USDCx Payout Engine

Batch payouts for grants & payroll on Stacks. Non-custodial, wallet-signed transfers using USDCx (SIP-010).

## Quickstart

```bash
# Install dependencies
pnpm install

# Run everything (devnet + web)
pnpm dev

# Or run individually:
pnpm web      # Next.js app at http://localhost:3000
pnpm devnet   # Clarinet devnet
```

## Architecture

```
stacks-hackathon/
├── apps/
│   └── web/                    # Next.js 15 + TypeScript (App Router)
├── contracts/
│   └── usdcx-payout-engine/    # Clarinet project + devnet
├── design/                     # UI source of truth (do not modify)
├── pnpm-workspace.yaml
└── package.json                # Root scripts
```

## Routes

| Route | Description |
|-------|-------------|
| `/` | Marketing landing page |
| `/app` | Create Payout Batch (CSV upload, preview, validation) |
| `/app/sign` | Execution & Signing (transaction queue, live event log) |
| `/app/complete` | Receipt (success summary, export CSV/JSON, print) |

## Features

### Create Payout Batch (`/app`)
- CSV upload with drag & drop
- Automatic validation of Stacks addresses
- Safety limits (configurable):
  - Max 50 recipients per batch
  - Max 100,000 USDCx total
  - Max 10,000 USDCx per recipient
- Live preview table with row management

### Execution & Signing (`/app/sign`)
- Transaction queue with status indicators
- Live event log (terminal-style)
- Network fee estimation
- Verification checkbox before signing

### Receipt (`/app/complete`)
- Success rate and statistics
- Transaction history with search
- Export to CSV or JSON
- Print receipt functionality

## CSV Template

The expected CSV format:

```csv
recipient,amount,memo
SP2J6ZY48GV1EZ5V2V5RB9MP66SW86PYKKNRV9EJ7,500.00,Dev Grant - October
SP1HTBVD3JG9C05J7HBJTHGR0GGW7KXW28M5JS8QE,1250.00,Design Bounty
SP3FGQ8Z7JY9BWYZ5WM53E0M9NK7WHJF0691NZ159,750.00,Community Lead
```

Download a template from the app or use the format above.

## Smart Contract

See [`contracts/usdcx-payout-engine/README.md`](./contracts/usdcx-payout-engine/README.md) for contract details.

**MVP Approach**: Non-custodial wallet-signed transfers. Each transfer is signed individually by the user's wallet (Leather). The contract provides optional batch tracking and event logging.

## Wallet Integration

Uses `@stacks/connect` for Leather wallet integration:
- Connect/disconnect wallet
- Display truncated address
- Network status indicator
- Sign and broadcast SIP-010 transfers

## Development

### Prerequisites
- Node.js 18+
- pnpm 8+
- Clarinet (for contract development)
- Leather wallet (browser extension)

### Environment Variables

Create `apps/web/.env.local` (optional):

```env
NEXT_PUBLIC_STACKS_NETWORK=mainnet
```

### Running Tests

```bash
# Contract tests
cd contracts/usdcx-payout-engine
npm install
npm test

# Web tests (if added)
cd apps/web
pnpm test
```

## Known Limitations (Mainnet Beta)

- Maximum batch size: 50 transactions
- Maximum total amount: 100,000 USDCx
- Maximum per recipient: 10,000 USDCx
- Transactions are simulated in MVP (real broadcast coming soon)

## Sources of Truth

1. **UI Design**: `/design/**` (HTML + screenshots) - Do not modify
2. **Protocol**: Stacks Connect + SIP-010 transfers

## References

- [Stacks Connect - Wallet Connection](https://docs.stacks.co/stacks-connect/connect-wallet)
- [Sending Transactions](https://docs.stacks.co/get-started/build-a-frontend/sending-transactions)
- [SIP-010 Fungible Token Standard](https://github.com/stacksgov/sips/blob/main/sips/sip-010/sip-010-fungible-token-standard.md)

## License

MIT
