# 🚨 MEVTrap: Real-Time Sandwich Attack Detection on Hoodi using Drosera Traps

**Author**: Mr.Warx

**Date**: July 2025

**Project**: [MEVTrap GitHub Repository](https://github.com/Warx04/Mevtrap-POC)

**Network**: Hoodi Testnet

**Stack**: Solidity, Foundry, Drosera Protocol, Uniswap V2

---

## 🌍 Introduction

**MEVTrap** is a smart-contract-based system designed to detect **sandwich attacks** in real time on Ethereum testnets using the [Drosera](https://drosera.network/) infrastructure. It leverages the on-chain power of Drosera’s trap architecture to monitor decentralized exchanges (DEXs) like **Uniswap V2**, without relying on off-chain bots or centralized servers.

In the landscape of MEV (Maximal Extractable Value), detecting malicious actors early is crucial. MEVTrap aims to provide a **public, transparent, and cost-effective** way to identify sandwiching behavior on-chain, where anyone can validate what happened and when.

![Mevtrap](https://github.com/user-attachments/assets/bd7b5b9e-ab4e-4f55-9fbc-d3f70b25c8cc)


---

## 🧠 What is a Sandwich Attack?

A **sandwich attack** is a common MEV exploit that targets user trades in decentralized exchanges. The attacker:

1. **Front-runs** a user transaction to affect price
2. Lets the user execute their transaction at a worse rate
3. **Back-runs** to profit from the new price

**Example on Uniswap:**

```
Tx A: User swaps 10 ETH → DAI
Tx B: MEV bot buys DAI (pushes price up)
Tx C: MEV bot sells DAI (after user, captures profit)

```

---

## 💡 What Does MEVTrap Do?

MEVTrap runs as a **trap contract** deployed on Hoodi and watched by Drosera operators. It:

- Samples the last few blocks (configurable)
- Looks for patterns matching the A → B → C structure
- Confirms whether a sandwich occurred based on token flows, amounts, and timing
- If confirmed, triggers a **ResponseProtocol** to log the data

All of this happens *on-chain*.

---

## 🛠️ How It Works

**Architecture Flow:**

```
User Tx (A) ──►
               │
MEV Bot Tx (B) ── sandwich pattern ──► MEVTrap ──► DroseraProtocol
               │                          │
User Tx (C) ──►                         ResponseContract logs attacker, victim, tokens, amounts

```

It analyzes transaction patterns via `collect()` and emits logs using `handleSandwich(...)` when `shouldRespond()` confirms a pattern.

![Flow Trap](https://github.com/user-attachments/assets/54d0d3c4-e8c9-4412-b1a7-665c6a1bf885)


---

## ⚙️ Technologies Used

| Layer | Tool/Library |
| --- | --- |
| Trap engine | [Drosera Trap](https://github.com/drosera-network/contracts) |
| Blockchain | Hoodi testnet |
| Contracts | Solidity (0.8.12) |
| Development Tool | [Foundry](https://book.getfoundry.sh/) |
| Optional Frontend | Next.js + Tailwind (optional dashboard) |

---

## 🚀 Why Is It Innovative?

- 💰 **Cost-efficient detection**: No off-chain infrastructure required
- 📡 **Live monitoring**: Responds as soon as the MEV pattern is validated
- 🔍 **Fully transparent**: Log entries can be verified by anyone on Sepolia
- 🧠 **Extendable**: Trap can evolve to detect other MEV behaviors or L2s

---

## 🧪 Sample Logs and Output

Each MEV incident logs an event with:

```solidity
event SandwichDetected(
    uint256 blockNumber,
    address victim,
    address attacker,
    address tokenIn,
    address tokenOut,
    uint256 amountIn,
    uint256 amountOut,
    bytes32 txHashA,
    bytes32 txHashB,
    bytes32 txHashC
);

```

This event can be indexed on a frontend or monitored via [cast call](https://book.getfoundry.sh/reference/cast/call.html).

---

## 🛡️ Public or Private Trap?

In the Drosera configuration (`drosera.toml`):

- `private = true`: Trap and results are **public**, enabling researchers to monitor MEV activity transparently
- `private_trap = true`: Makes it accessible only to whitelisted relayers (for R&D or private networks)

---

## 🔧 Setup & Deploy

Clone the repo:

```bash
git clone <https://github.com/Warx04/MEVTrap-POC.git>
cd MEVTrap-POC
bun install
forge build

```

Deploy on Hoodi:

```bash
forge script scripts/DeployResponseProtocol.s.sol --rpc-url <https://ethereum-hoodi-rpc.publicnode.com> --broadcast

```

Apply trap to Drosera:

```bash
drosera apply

```

---

## 🛰️ Future Plans

- 📊 Frontend dashboard to visualize MEV incidents in real-time
- 🔄 Support for other testnets (Holesky Testnet, and Hoodi Testnet)
- 💡 Integration with Uniswap V3 Pool callbacks
- 🔒 Private detection with optional on-chain sanctions

---

## 📜 License

MIT License – Open and free to use with attribution.

Built with ❤️ for on-chain transparency.
