# KovanicaDAG — CODE_INDEX

Maps documentation topics to authoritative source files in
`/home/antonio/KovanicaDAG/kovanica-protocol` (the merged repo, branch
`main`). Links are `file://` URIs that open the file locally.

*Checked 2026-09-08.*

## Consensus core — `crates/kovanica-dag`

| Topic | File |
|-------|------|
| Crate docs + re-exports, doctest tour | [lib.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/lib.rs) |
| Block / BlockId, work, timestamp, nonce | [block.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/block.rs) |
| Dag store: insert/validate, reachability, mergeset, ghostdag data, preview, chain key | [dag.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/dag.rs) |
| GHOSTDAG colouring: selected parent, mergeset, k-cluster | [ghostdag.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/ghostdag.rs) |
| Linearization / selected chain | [ordering.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/ordering.rs) |
| Pluggable insert-time validation | [validation.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/validation.rs) |
| Snapshot (replay-log) persistence | [snapshot.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/snapshot.rs) |
| Difficulty retargeting (`Retarget::next_work`) | [difficulty.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/difficulty.rs) |
| Proof-of-work (`meets_target`/`mine`) | [pow.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/pow.rs) |
| Reachability oracle (interval-tree + future-covering sets) | [reachability.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/reachability.rs) |
| VRF (ECVRF Ristretto255, leader selection / beacon) | [vrf.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/src/vrf.rs) |
| Consensus/adversarial tests | [tests/consensus.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/tests/consensus.rs) |
| Reachability differential tests | [tests/reachability.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/tests/reachability.rs) |
| Difficulty tests | [tests/difficulty.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/tests/difficulty.rs) |
| PoW tests | [tests/pow.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-dag/tests/pow.rs) |

## State / ledger — `crates/kovanica-state`

| Topic | File |
|-------|------|
| Crate docs, re-exports, end-to-end doctest | [lib.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/lib.rs) |
| Address / KeyPair / verify / `kvnc…dag` | [keys.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/keys.rs) |
| Transactions, sighash, canonical encoding | [tx.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/tx.rs) |
| UtxoSet + per-UTXO `creation_height` (checkpoint v6) | [utxo.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/utxo.rs) |
| Ledger: per-block state, finality, hybrid, vault/CSV rules, checkpoints | [ledger.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/ledger.rs) |
| LedgerStore (append-only replay log) | [store.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/store.rs) |
| Structural validation (`TxStructureValidator`) | [validation.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/validation.rs) |
| Multisig (RFC-001, M-of-N P2SH) | [multisig.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/multisig.rs) |
| HTLC (RFC-004, atomic swaps) | [htlc.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/htlc.rs) |
| Vault / CSV (RFC-005) | [vault.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/vault.rs) |
| Script v2 / stealth (RFC-003, CSV opcode) | [script_v2.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/script_v2.rs) |
| Stake registry (hybrid) | [stake.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/stake.rs) |
| SPV verification helpers | [spv.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/src/spv.rs) |
| Vault test suite (26 tests) | [tests/vault.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/tests/vault.rs) |
| HTLC / multisig / native-token / stealth suites | [tests/htlc.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/tests/htlc.rs) · [tests/multisig_consensus.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/tests/multisig_consensus.rs) · [tests/native_token_consensus.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/tests/native_token_consensus.rs) · [tests/stealth_script_v2_consensus.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/tests/stealth_script_v2_consensus.rs) |
| Hybrid / persistence / finality / undo-log suites | [tests/hybrid.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/tests/hybrid.rs) · [tests/persistence.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/tests/persistence.rs) · [tests/finality.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/tests/finality.rs) · [tests/undo_log.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-state/tests/undo_log.rs) |

## Node / binary — `crates/kovanica-node`

| Topic | File |
|-------|------|
| Crate docs, re-exports, RPC doctest | [lib.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/lib.rs) |
| Node: genesis/send/produce/balance, vault & HTLC helpers, sync | [node.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/node.rs) |
| Mempool | [mempool.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/mempool.rs) · [mempool_v2.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/mempool_v2.rs) |
| Networking: gossip + TCP pull / framed exchange | [net.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/net.rs) |
| P2P mesh + hardening | [p2p.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/p2p.rs) · [p2p_hardening.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/p2p_hardening.rs) |
| DHT (Kademlia) + DNS seeds | [dht.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/dht.rs) · [dns_seed.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/dns_seed.rs) |
| Long-lived relay session + SPV client | [relay.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/relay.rs) · [spv.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/spv.rs) |
| Line RPC (incl. `vault_*`, `htlc_*` commands) | [rpc.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/rpc.rs) |
| Explorer (JSON API + static UI, WS, faucet) | [explorer.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/explorer.rs) · [explorer.html](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/explorer.html) |
| Metrics + structured logging | [metrics.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/metrics.rs) |
| Fuzzing infrastructure | [fuzz.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/fuzz.rs) |
| Binary (`serve`/`demo`) | [main.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/src/main.rs) |
| Vault node/RPC tests (5 tests) | [tests/vault_node.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/tests/vault_node.rs) |
| Other node test suites | [tests/network.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/tests/network.rs) · [tests/p2p.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/tests/p2p.rs) · [tests/rpc.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/tests/rpc.rs) · [tests/hybrid_node.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-node/tests/hybrid_node.rs) |

## FFI / mobile — `crates/kovanica-ffi`

| Topic | File |
|-------|------|
| UniFFI object surface | [light_node.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-ffi/src/light_node.rs) |
| FFI tests (9+ cases) | [tests/ffi.rs](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-ffi/tests/ffi.rs) |
| Android build (cargo-ndk) | [build-android.sh](file:///home/antonio/KovanicaDAG/kovanica-protocol/crates/kovanica-ffi/build-android.sh) |

## Docs / plans (development series)

| Topic | File |
|-------|------|
| AGENTS (conventions + roadmap) | [AGENTS.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/AGENTS.md) |
| KVP standard registry | [docs/KVP.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/KVP.md) |
| RFC-001 Multisig | [docs/RFC-001-Multisig.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/RFC-001-Multisig.md) |
| RFC-002 Native tokens | [docs/RFC-002-NativeTokens.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/RFC-002-NativeTokens.md) |
| RFC-003 Script v2 + stealth | [docs/RFC-003-ScriptV2-and-Stealth.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/RFC-003-ScriptV2-and-Stealth.md) |
| RFC-004 HTLC | [docs/RFC-004-Htlc.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/RFC-004-Htlc.md) |
| RFC-005 Vault / CSV | [docs/RFC-005-Vault.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/RFC-005-Vault.md) |
| Plans | [docs/plans/vault-time-lock.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/plans/vault-time-lock.md) · [docs/plans/mobile-light-node.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/plans/mobile-light-node.md) · [docs/plans/android-light-node-app.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/plans/android-light-node-app.md) · [docs/plans/htlc-atomic-swap.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/plans/htlc-atomic-swap.md) |
| Testnet ops | [TESTNET.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/TESTNET.md) · [OPERATIONS.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/OPERATIONS.md) · [docs/soak-snapshot-2026-09-03.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/soak-snapshot-2026-09-03.md) |
| Explorer API docs | [docs/api/explorer.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/api/explorer.md) |
| Tokenomics / overview | [docs/TOKENOMICS.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/TOKENOMICS.md) · [docs/WHAT-IS-KOVANICA.md](file:///home/antonio/KovanicaDAG/kovanica-protocol/docs/WHAT-IS-KOVANICA.md) |