# KovanicaDAG — Roadmap

Tracking document mirrored from `kovanica-protocol/AGENTS.md` (the normative
source). Stages 0–3 are shipped history; Post-Stage 3 tracks hardening.

## Stage 0 — Shipped: single-chain → BlockDAG testnet

Deployed on `kovanica-testnet` (seed: `seed.kovanica.online:9000`, explorer:
`explorer.kovanica.online`). CI gates every push.

- [x] Transactions + UTXO state layer; apply state in linearized order (`kovanica-state`)
- [x] ed25519 signatures for spend authorisation
- [x] Block-level validation at insert (structural `BlockValidator` + stateful via `Ledger` + `Dag::preview`)
- [x] Recursive GHOSTDAG linearization; per-block UTXO state composes along
- [x] Per-block UTXO state built incrementally from selected parent (`Ledger`, matches `apply_dag`)
- [x] Finality-depth pruning + re-orgs (`Ledger::with_finality`)
- [x] Persistence: replay-log snapshots (`write_snapshot`/`read_snapshot`, state recomputed)
- [x] Reachability oracle (`reachability::Reachability`) as the `Dag`'s backing
- [x] Incremental reachability maintenance (Kaspa interval **reindexing**)
- [x] Incremental / streaming on-disk store (`LedgerStore`)
- [x] Runnable node binary + line RPC (`serve`/`demo`)
- [x] Mempool + block production; multi-node block dissemination (in-process + TCP pull)
- [x] Continuous in-process p2p gossip (`p2p::Mesh`), tx dissemination, mempool eviction
- [x] Long-lived TCP relay (`relay::RelaySession`) + WebSocket (`/ws`)
- [x] Difficulty adjustment + **consensus enforcement** (`Dag::set_difficulty`, `Block.timestamp_ms`)
- [x] Wall-clock future-time bound on timestamps (node policy, injectable clock)
- [x] **Real PoW** (`kovanica-dag::pow`): `H * work < 2^256`, opt-in, composable with difficulty
- [x] Halving schedule (`HalvingSchedule`); TX size limits; WebSocket explorer + live WS client
- [x] Human addresses (`kvnc…dag` base58), framed bidirectional TCP sync, multi-input transfers
- [x] CI gate + dual-stack P2P listeners

## Stage 1 — Operations hardening (shipped)

No consensus changes; make the testnet trustworthy to operate.

- [x] Arm auto-deploy (`VPS_HOST` / `VPS_USERNAME` / `VPS_PRIVATE_KEY` / `DEPLOY_ENABLED=true`)
- [x] Seed ops runbook: `OPERATIONS.md` (backup/restore, restart, post-deploy checks)
- [x] Web proxy question resolved: kovanica-web reaches explorer **server-side** (cors-proxy dropped)
- [x] `kvnc…dag` addresses in web wallet UI (shipped upstream)

## Stage 2 — Scale & persistence (shipped)

- [x] Headers-first sync (tips/headers → bodies by hash)
- [x] DAG-level payload pruning behind the reachability oracle (`payload: Option<Vec<u8>>`)
- [x] Finality checkpointing (UTXO set at finality depth + tip-segment replay; format v2/v3)
- [x] Reachability interval-reindex amortisation tuning (`CHILD_RESERVE`)

## Stage 3 — Protocol evolution (shipped, workspace 0.2.0)

- [x] VRF for leader selection / randomness beacon (ECVRF Ristretto255, IRTF CFRG draft)
- [x] P2P hardening (rate limits, duplicate suppression, peer scoring/banning)
- [x] Mempool v2 (orphan pool, fee-based eviction, capacity limits)
- [x] Stake registry for hybrid PoW + VRF-staked validation (slice 1)
- [x] Hybrid PoW + VRF-staked block admission (slice 2)
- [x] FFI slices 1–8 (LightNode bindings, custody/unbond, SPV/filters, packaging, wallet UX, release)
- [x] Android LightNode app slices 9a–9d (genesis gate, wallet UX, light sync, staking uplink)

## Post-Stage 3 — Production hardening

1. ✅ **Light clients / SPV proofs** — header chain, Merkle proofs, Golomb-Rice filters, `SpvClient`
2. ✅ **Multi-seed discovery** — DNS seeds + DHT Kademlia (deployment wiring left in `TODO.md`)
3. ✅ **Observability & reliability** — Prometheus metrics, structured logging, alert rules, fuzzing
4. 🔜 **Testnet soak & parameter tuning — ACTIVE NEXT**
   - 24/7 testnet, multiple independent seed operators (seed = Hostinger VPS; seed3 = AWS eu-north-1)
   - Measure: orphan rate, propagation latency, fork rate, disk growth (both seeds expose `/metrics`)
   - Tune: `k`, finality depth, payload pruning depth, difficulty window
5. ✅ **Wallet & explorer polish** — hardware wallet (Ledger/Trezor), BIP39/BIP44, fee estimation, DAG viz

## Kovi — engineering agent (standalone repo, `KovanicaDAG/kovanica-agent`)

Shipped 2026-09-09 on the VPS (CPU path). Snapshot: `kovanica-agent/`, overview [[KOVI]].

- [x] FastAPI `/chat` + `/confirm`, LangGraph + SQLite checkpoints
- [x] RAG: Qdrant `kovanica_codebase` (251 chunks of the protocol repo),
      fastembed `BAAI/bge-small-en-v1.5`, Rust-aware chunking
- [x] Sandboxed `cargo check/test/clippy/build` via socket-owning
      `sandbox-runner` sidecar (network-disabled ephemeral containers)
- [x] CPU compose path (Ollama `qwen2.5-coder:3b`) + GPU path (vLLM Qwen-32B-Coder)
- [x] End-to-end `/chat` verified on the VPS
- [ ] gVisor (`runsc`) on the host; harden sandbox-runner (socket proxy, sidecar auth)
- [ ] Real auth (`AUTH_JWKS_URL`) before non-localhost exposure
- [ ] Arm apply→PR path (`AGENT_GIT_APPLY_ENABLED=1` + repo auth); auto-index webhook

## Upgrade phases (cross-repo plan)

| Phase | Status |
|---|---|
| 1 — Foundation & consensus infra | ✅ (D2 pending) |
| 2 — Consensus evolution | 🔄 B1 epoch randomness beacon · B2 DAG-level past-set pruning · B3 UTXO undo log (all in progress; B3 undo log tested) |
| 3 — Performance & scalability | ✅ |
| 4 — Mobile light-node | ✅ |
| 5 — Wallet & security (multisig) | ✅ |
| 6 — Operations & reliability | ✅ |
| 7 — P2 polish | ✅ (Android unit tests for Format.kt/KVLS parsing still pending — no SDK locally) |

---

*Last checked against `kovanica-protocol/AGENTS.md`: 2026-09-08. Kovi agent snapshot added 2026-09-09.*