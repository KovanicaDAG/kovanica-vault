# KovanicaDAG — Project Overview for Obsidian

> **Vault note.** This vault (`kovanica-vault`) hosts the documentation for the
> **kovanica-protocol** project. The `kovanica-protocol/` folder is a snapshot of
> the merged repo's docs (README, AGENTS, TESTNET, OPERATIONS, TODO, and the full
> `docs/` tree). This overview describes the **current merged repo layout** and
> supersedes the old per-repo snapshot content from the pre-merge era.

## What is this?

**kovanica-protocol** is a **DAG-based distributed ledger** — a high-throughput,
parallel-block cryptocurrency/ledger protocol built on a **Directed Acyclic
Graph** (BlockDAG) rather than a single linear chain. The name *kovanica* is
Serbo-Croatian for "coin / mint."

Blocks reference **multiple parents**, enabling parallel block production and
high block rates (BPS). Consensus follows **GHOSTDAG** (Sompolinsky, Wyborski &
Zohar — the protocol behind Kaspa). Hybrid admission combines Nakamoto
proof-of-work with VRF-staked block production (Algorand/Praos-style sortition),
and phones sync as light nodes through UniFFI bindings.

## Repository Structure (merged `kovanica-protocol`)

```
kovanica-protocol/
├── crates/
│   ├── kovanica-dag/        # DAG + GHOSTDAG consensus core (reachability, colouring, linearization, PoW/difficulty/VRF)
│   ├── kovanica-state/      # UTXO ledger applied in GHOSTDAG order (ed25519, stake, hybrid, snapshots, SPV)
│   ├── kovanica-node/       # Runnable node: RPC, mempool, P2P mesh + DHT/DNS, metrics, self-hosted explorer
│   ├── kovanica-ffi/        # LightNode — UniFFI bindings for Kotlin/Swift mobile light nodes
│   └── kovanica-cli/        # CLI wallet
├── web/                     # TanStack Start web UI
├── android-light-node/      # Jetpack Compose light-node wallet app
├── docs/                    # RFCs, KVP standards, plans, ops docs (snapshot in kovanica-protocol/docs/)
└── AGENTS.md                # Source of truth for conventions + roadmap (snapshot in kovanica-protocol/)
```

## Kovi — the engineering agent

**[Kovi, Product of Kovanica](https://github.com/KovanicaDAG/kovanica-agent)** is a
RAG-powered engineering assistant for this codebase (snapshot in
`kovanica-agent/`, vault overview [[KOVI]]):

- `/chat` + `/confirm` FastAPI, LangGraph + SQLite, Qdrant/fastembed RAG
  (Rust-aware chunking, `BAAI/bge-small-en-v1.5`)
- Sandboxed `cargo check/test/clippy/build` in an ephemeral network-disabled
  container via a socket-owning `sandbox-runner` sidecar
- Human-gated **apply → draft PR** flow (throwaway worktree off `origin/main`,
  `gh`; fail-closed `dry_run` by default)
- GPU (vLLM Qwen-32B-Coder) and CPU (Ollama `qwen2.5-coder:3b`) compose paths

Deployed on the VPS (CPU path) — live `/chat` verified; RAG index of the
protocol repo built (251 chunks); model `qwen2.5-coder:3b` serving.

## Core Domain Concepts

| Term | Meaning |
|------|---------|
| **DAG / BlockDAG** | Ledger is a DAG; blocks reference multiple parents/tips |
| **Tip** | Block with no children yet |
| **Past / ancestors** | All blocks reachable by following parent edges |
| **Selected parent** | Parent with heaviest blue work; forms chain backbone |
| **Mergeset** | `past(B) \ (past(sp) ∪ {sp})` — blocks merged by B |
| **Blue set / red set** | Well-connected honest cluster (blue) vs. side blocks (red) |
| **k parameter** | Max tolerated blue anticone size (every blue block ≤ k blue anticone) |
| **Blue score / work** | Size / total work of a block's blue set; drives selection |
| **Linearization** | Deterministic total order: `order(B) = order(sp) ++ mergeset ++ [B]` |

## KVP / RFC Standards (all Shipped)

| KVP | RFC | Title |
|-----|-----|-------|
| **KVP-101** | [RFC-001](kovanica-protocol/docs/RFC-001-Multisig.md) | Multisig (M-of-N P2SH) |
| **KVP-102** | [RFC-002](kovanica-protocol/docs/RFC-002-NativeTokens.md) | Native multi-asset tokens |
| **KVP-103** | [RFC-003](kovanica-protocol/docs/RFC-003-ScriptV2-and-Stealth.md) | Stealth addresses + script v2 |
| **KVP-104** | [RFC-004](kovanica-protocol/docs/RFC-004-Htlc.md) | HTLC atomic swaps |
| **KVP-105** | [RFC-005](kovanica-protocol/docs/RFC-005-Vault.md) | Time-lock vault / escrow (real CSV) |

## Current Status (as of 2026-09-08)

### ✅ Stage 0 — BlockDAG testnet (shipped)
Deployed on `kovanica-testnet` (seed: `seed.kovanica.online:9000`, explorer:
`explorer.kovanica.online`).

- Transactions + UTXO state, ed25519 signatures; block validation (structural + stateful)
- Recursive GHOSTDAG linearization; per-block UTXO state from selected parent
- Finality-depth pruning + implicit re-orgs; replay-log snapshots
- Incremental reachability oracle (Kaspa reindexing)
- Node binary + line RPC (`serve`/`demo`); mempool + block production + multi-node gossip
- Continuous P2P (`p2p::Mesh`), long-lived TCP relay + WebSocket, dual-stack listeners
- Difficulty retargeting + **consensus enforcement**; wall-clock future-time bound (node policy)
- **Real PoW** (opt-in, Nakamoto `H * work < 2^256`)
- Halving schedule, TX size limits, human addresses (`kvnc…dag`)
- Framed bidirectional TCP sync, multi-input transfers
- CI gate + deploy auto-arming

### ✅ Stage 1 — Operations hardening (shipped)
- Auto-deploy armed (`VPS_HOST`/`VPS_USERNAME`/`VPS_PRIVATE_KEY`, `DEPLOY_ENABLED`)
- Seed ops runbook: `OPERATIONS.md`
- Web proxy resolved (server-side); wallet shows `kvnc…dag` addresses

### ✅ Stage 2 — Scale & persistence (shipped)
- Headers-first sync
- DAG-level payload pruning behind the reachability oracle
- Finality checkpointing (UTXO-set at finality depth, tip-segment replay)
- Reachability interval-reindex amortisation tuning (`CHILD_RESERVE`)

### ✅ Stage 3 — Protocol evolution (shipped, workspace **0.2.0**)
- VRF for leader selection / randomness beacon (ECVRF, IRTF CFRG draft)
- P2P hardening: per-peer rate limits, duplicate suppression, peer scoring/banning
- Mempool upgrades: orphan pool, fee-based eviction, capacity limits
- Stake registry (slice 1) + hybrid PoW + VRF-staked admission (slice 2)
- FFI slices 1–8: LightNode bindings, custody/unbond, SPV/filters, mobile packaging,
  wallet UX layer, docs & release
- Android LightNode app slices 9a–9d (genesis gate, wallet UX, light sync, staking uplink)

### Post-Stage 3 — Production hardening
- ✅ Light clients / SPV proofs (header chain, Merkle proofs, Golomb-Rice filters, `SpvClient`)
- ✅ Multi-seed discovery (DNS seeds + DHT Kademlia)
- ✅ Observability & reliability (Prometheus metrics, structured logging, alerting rules, fuzzing)
- ✅ Wallet & explorer polish (hardware wallet, BIP39/BIP44, fee estimation, DAG viz)
- **🔜 Testnet soak & parameter tuning — ACTIVE NEXT** (seed = Hostinger VPS, seed3 = AWS eu-north-1)

### Upgrade phases (cross-repo execution plan)
Phases 1–7 completed: Foundation & consensus infra · Consensus evolution (in
progress items B1/B2/B3 tracked in the roadmap) · Performance & scalability ·
Mobile light-node · Wallet & security (multisig) · Operations & reliability ·
P2 polish.

## Key Implementation Files (for reference)

| Feature | Files |
|---------|-------|
| Block / DAG / pruning | `crates/kovanica-dag/src/block.rs`, `dag.rs` |
| Reachability oracle | `crates/kovanica-dag/src/reachability.rs` |
| GHOSTDAG colouring / linearization | `crates/kovanica-dag/src/ghostdag.rs`, `ordering.rs`, `vrf.rs` |
| PoW / difficulty | `crates/kovanica-dag/src/pow.rs`, `difficulty.rs` |
| Ledger, finality, checkpoint, hybrid, stake | `crates/kovanica-state/src/ledger.rs`, `stake.rs` |
| Multisig / HTLC / Vault / ScriptV2 | `crates/kovanica-state/src/multisig.rs`, `htlc.rs`, `vault.rs`, `script_v2.rs` |
| Node / RPC / P2P / metrics | `crates/kovanica-node/src/node.rs`, `rpc.rs`, `net.rs`, `p2p.rs`, `metrics.rs` |
| FFI / mobile bindings | `crates/kovanica-ffi/src/lib.rs`, `light_node.rs` |
| Conventions / roadmap | `kovanica-protocol/AGENTS.md` |

Full index with `file://` links: [[CODE_INDEX]].

## Build & Test Commands

```bash
# From kovanica-protocol/
cargo build           # Build all crates
cargo test            # Run all tests (unit + integration + doctest)
cargo clippy --all-targets  # Lint (must be warning-clean)
cargo fmt --check     # Format gate

# Run node demo
cargo run -p kovanica-node -- demo

# Run node REPL
cargo run -p kovanica-node   # then type: help
```

## Links

- **Testnet seed**: `seed.kovanica.online:9000` (seed2/seed3 also resolve via DNS seeds)
- **Explorer**: `explorer.kovanica.online`
- **GitHub**: https://github.com/KovanicaDAG/kovanica-protocol · [Kovi agent](https://github.com/KovanicaDAG/kovanica-agent)
- **Vault navigation**: [[NAVIGATION]] · [[ROADMAP]] · [[CODE_INDEX]]

---

*Updated: 2026-09-09 — kovanica-protocol merged repo, RFC-005 shipped, vault relocated to kovanica-vault, Kovi agent snapshot added.*