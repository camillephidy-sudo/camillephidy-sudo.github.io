---
title: "Lockstep — Decentralized Milestone Escrow on Sepolia"
collection: portfolio
date: 2025-05-01
venue: "Alchemy University EVM Capstone"
excerpt: "Full-stack Web3 demo: Foundry smart contract + React/wagmi frontend for trustless milestone payments between clients and freelancers on Sepolia testnet."
link: https://github.com/camillephidy-sudo/Escrow-for-Freelancers-Clients
tags:
  - web3
  - solidity
  - foundry
  - react
  - defi
  - escrow
---

## Overview

**Lockstep** is a milestone escrow system for freelancers and clients—developed as my **Alchemy University EVM Capstone**. ETH is locked on **Sepolia**; the client releases payment to a freelancer only when each milestone is approved on-chain.

| Attribute | Detail |
|-----------|--------|
| **Role** | Full-stack developer |
| **Period** | May 2025 |
| **Program** | [Alchemy University EVM certification](https://university.alchemy.com/certifications/evm-chain) |
| **Stack** | Solidity, Foundry, React, Vite, wagmi, Sepolia testnet |
| **License** | MIT |
| **Repository** | [camillephidy-sudo/Escrow-for-Freelancers-Clients](https://github.com/camillephidy-sudo/Escrow-for-Freelancers-Clients) |

## Problem & Solution

| | |
|---|---|
| **Problem** | Clients and freelancers struggle with trust: pay upfront vs. deliver first; traditional escrow is opaque and slow. |
| **Solution** | On-chain escrow locks funds in `Escrow.sol`; the client calls `approveMilestone` to release proportional ETH per milestone. |

## Architecture

```text
React (wagmi)  →  Escrow.sol  →  Sepolia testnet
```

```text
├── contracts/          # Foundry: Escrow.sol, tests, deploy script
├── frontend/           # Vite + React + wagmi UI
└── README.md
```

## Smart Contract Design

- **`Escrow.sol`** holds client-deposited ETH until milestones are approved
- **`approveMilestone`** releases proportional payment per completed deliverable
- **Foundry test suite** validates funding, approval, and withdrawal flows before deployment
- Deploy script supports Sepolia broadcast and Etherscan verification

### Run tests

```bash
cd contracts
forge test -vv
```

### Deploy to Sepolia

```bash
cd contracts
forge script script/Deploy.s.sol:DeployScript --rpc-url sepolia --broadcast --verify -vvvv
```

## Frontend

The Vite + React client connects via **wagmi** and **WalletConnect**, letting users:

1. Connect a Sepolia wallet
2. Create and fund a milestone escrow
3. Approve milestones and release payments
4. Inspect transactions on Etherscan

```bash
cd frontend
npm install
npm run dev
```

Environment variables: `VITE_ALCHEMY_API_KEY`, `VITE_ESCROW_ADDRESS`, `VITE_WALLETCONNECT_PROJECT_ID`.

## Demo Flow (Certification Video)

| Time | Section | Talking points |
|------|---------|----------------|
| 0:00–0:30 | Elevator pitch | “Lockstep: milestone escrow on Sepolia for freelancers and clients.” |
| 0:30–1:00 | Problem | Pay-upfront vs. deliver-first trust gap |
| 1:00–1:30 | Solution | ETH locked in contract; milestone approvals release funds |
| 1:30–3:30 | Live demo | Connect → create & fund → approve milestone → Etherscan |
| 3:30–4:00 | Wrap-up | Foundry tests, wagmi frontend, AU learnings |

## Certification Checklist

- [ ] Contract deployed on Sepolia with address in frontend config
- [ ] Wallet funded with Sepolia ETH (client + freelancer accounts)
- [ ] Record ≤5 min demo video (pitch, problem, solution, live demo)
- [ ] Submit to [AU EVM Project Submission](https://university.alchemy.com/certifications/evm-chain)
- [ ] Optional: deploy frontend to Vercel

## Skills Demonstrated

Solidity & Foundry testing · smart contract deployment · React/wagmi wallet integration · testnet DevOps · technical documentation · capstone demo storytelling

## Resources

- [GitHub repository](https://github.com/camillephidy-sudo/Escrow-for-Freelancers-Clients)
- [Alchemy University EVM certification](https://university.alchemy.com/certifications/evm-chain)
- [Alchemy Builders Discord](https://discord.com/invite/alchemy-builders)
