# IRSB

> On-chain guardrails for AI agents — EIP-7702 spend limits, cryptographic execution receipts, automated dispute resolution. No agent should hold unguarded keys.

**Live work → [jeremylongshore/irsb](https://github.com/jeremylongshore/irsb)**

---

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/U5S225PTME)

## The Problem

AI agents are executing on-chain transactions at increasing scale. Every major framework — AgentKit, ElizaOS, Olas, Virtuals, Brian AI — gives agents wallet access. None of them answer a fundamental question: **what happens when the agent overspends, calls the wrong contract, or acts outside its mandate?**

Today, the answer is: nothing. The agent holds the keys, and the owner trusts that it behaves correctly. Agent wallets face the same attack surfaces as any EOA — prompt injection, key compromise, logic bugs — but agents operate autonomously. A compromised agent can drain funds before a human notices.

## Three Layers

**1. Policy Enforcement** — An agent's EOA delegates to a `WalletDelegate` smart contract via EIP-7702. Every transaction passes through five on-chain caveat enforcers before execution. They cannot be bypassed by the agent, its framework, or a compromised prompt.

**2. Execution Receipts** — Every successful agent action produces a cryptographic receipt: what it intended, what happened, supporting evidence, and a signature proof. V2 adds dual attestation with agent + client EIP-712 co-signatures.

**3. Automated Monitoring** — The watchtower scans receipts against a configurable rule engine. Violations auto-file disputes on-chain. Deterministic cases (timeout, wrong amount) resolve automatically. Complex cases escalate with counter-bonds.

## On-Chain Enforcers

| Enforcer | What It Does | Example |
|---|---|---|
| `SpendLimitEnforcer` | Daily and per-transaction spending caps | 0.1 ETH/day, 0.01 ETH/tx |
| `TimeWindowEnforcer` | Restrict actions to defined time windows | 09:00–17:00 UTC only |
| `AllowedTargetsEnforcer` | Whitelist of approved contract addresses | Uniswap V3 Router only |
| `AllowedMethodsEnforcer` | Whitelist of approved function selectors | `swap()` yes, `approve()` no |
| `NonceEnforcer` | Replay prevention for each delegated action | One nonce per action |

## Live on Sepolia

| Contract | Address |
|---|---|
| `WalletDelegate` | [`0x6e7262bA8eE3e722aD5f83Ad793f3c071A3769cB`](https://sepolia.etherscan.io/address/0x6e7262bA8eE3e722aD5f83Ad793f3c071A3769cB) |
| `IntentReceiptHub` | [`0xD66A1e880AA3939CA066a9EA1dD37ad3d01D977c`](https://sepolia.etherscan.io/address/0xD66A1e880AA3939CA066a9EA1dD37ad3d01D977c) |
| `DisputeModule` | [`0x144DfEcB57B08471e2A75E78fc0d2A74A89DB79D`](https://sepolia.etherscan.io/address/0x144DfEcB57B08471e2A75E78fc0d2A74A89DB79D) |
| `SolverRegistry` | [`0xB6ab964832808E49635fF82D1996D6a888ecB745`](https://sepolia.etherscan.io/address/0xB6ab964832808E49635fF82D1996D6a888ecB745) |

Full addresses, ABIs, and deployment scripts in the [live repo](https://github.com/jeremylongshore/irsb#live-on-sepolia).

## Documentation

Protocol spec, audit reports, EIP submissions, and integration guides all live in the main repository's `docs/` tree.

[→ Open jeremylongshore/irsb](https://github.com/jeremylongshore/irsb)

---

Intent Solutions — [intentsolutions.io](https://intentsolutions.io)
