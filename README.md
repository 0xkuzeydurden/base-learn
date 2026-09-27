# Base Learn

[![CI](https://github.com/0xkuzeydurden/base-learn/actions/workflows/ci.yml/badge.svg)](https://github.com/0xkuzeydurden/base-learn/actions/workflows/ci.yml)

A Hardhat and React/Vite toolkit for exploring Base Learn Solidity exercises,
deploying contracts, and submitting supported contracts to the configured registries.

Developed by [Kuzey Durden](https://x.com/kuzeydurdn).

## Attribution

The Solidity exercises are derived from [mztacat/Base](https://github.com/mztacat/Base),
including contributions by Mztacat, smmroms (philip0529), and Hachiman (Hachiman2284).
This project adds Hardhat deployment tooling and a React/Vite interface. The existing
license identifiers and source attribution are retained in the contract files.

## What is included

- Solidity exercises covering storage, arrays, mappings, inheritance, tokens, and NFTs.
- Hardhat scripts for deploying individual groups of contracts and writing deployment reports.
- A browser interface with RainbowKit, wagmi, and viem for wallet-driven deployments.
- Constructor forms, supported registry submissions, and local progress tracking per wallet and chain.

The browser interface is configured for Base Sepolia. The command-line configuration
also includes Base mainnet. Public contract addresses in the source identify the
registries used by the application.

## Local setup

Use a recent Node.js version compatible with the dependencies in the root and `ui/`
packages, and npm. Contract deployment requires a funded wallet on the selected network.

```bash
git clone https://github.com/0xkuzeydurden/base-learn.git
cd base-learn
npm install
npm run compile
```

Compilation is sufficient to generate the artifacts used by the UI and does not
send a transaction.

### Browser interface

Create `ui/.env` with your WalletConnect project ID:

```dotenv
VITE_WALLETCONNECT_PROJECT_ID=your_project_id
```

Variables prefixed with `VITE_` are exposed to the browser. Do not put wallet private
keys or server secrets in them. The all-zero fallback in the source is a placeholder.

```bash
cd ui
npm install
npm run dev
```

The interface imports Hardhat artifacts from the root `artifacts/` directory.
Compile the contracts again after changing Solidity source. Use `npm run build`
inside `ui/` to produce the frontend in `ui/dist/`.

### Command-line deployment

Create a root `.env` locally when using a wallet to deploy to Base or Base Sepolia:

```dotenv
PRIVATE_KEY=your_wallet_private_key
BASE_RPC_URL=https://mainnet.base.org
BASE_SEPOLIA_RPC_URL=https://sepolia.base.org
ETHERSCAN_API_KEY=your_api_key_if_needed
```

The RPC overrides and explorer API key are optional. Keep real credentials out of
Git; the root `.gitignore` excludes `.env` files. No sample `.env.example` is included.

Constructor defaults are supplied by `deploy-config.json`. Use
`deploy-config.example.json` as a reference when editing those values.

| Command | Purpose |
| --- | --- |
| `npm run compile` | Compile the Solidity contracts |
| `npm run clean` | Remove generated Hardhat build files |
| `npm run deploy:local` | Deploy against a separately running localhost node |
| `npm run deploy:base-sepolia` | Deploy to Base Sepolia using the configured wallet |
| `npm run deploy:base` | Deploy to Base mainnet using the configured wallet |

For a local deployment, start `npx hardhat node` in one terminal, then run
`npm run deploy:local` in another. Deployment commands write contract names,
addresses, constructor arguments, and transaction hashes to
`deployments/<network>-deployments.json`. Re-running a deployment creates new
instances and can replace the report, so retain reports you still need.

## Registry submissions

The UI includes a Claim PIN action for tasks that have a configured registry.
Review the selected network, contract address, and wallet transaction before
confirming. The repository does not establish current badge eligibility or guarantee
that a particular registry or external campaign remains available.

## Repository layout

- `contracts/` — Solidity exercise sources.
- `scripts/` — Contract deployment and registry scripts.
- `hardhat.config.js` — Compiler and network configuration.
- `deploy-config*.json` — Example constructor arguments and deployment defaults.
- `ui/` — React/Vite interface, wallet integration, and progress tracking.
- `netlify.toml` — Hosting configuration supplied with the project.

The hosting configuration currently calls a root `npm run build` script, which is
not defined in the root package. Review the build command before deploying; contract
compilation and the frontend build are separate steps.

## Project status

This is an educational project. The repository contains no automated test suite.
Review contract behavior and validate deployment and registry flows on a test
network before using funds on mainnet.
