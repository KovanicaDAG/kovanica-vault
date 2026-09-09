# KovanicaDAG — Vault Navigation

> Home of the **kovanica-protocol** documentation. Open from Obsidian: this is
> the entry point.

## Core notes

| Note | What |
|------|------|
| [[myObsidianVaultDAG]] | Project overview — merged repo layout, status, standards table |
| [[ROADMAP]] | Stage-by-stage tracking (shipped history, active items) |
| [[CODE_INDEX]] | Docs topics → source files (`file://` links) |
| [[AGENTS]] | Vault sync agent guide (how this vault is maintained) |

## kovanica-protocol snapshot

The `kovanica-protocol/` folder mirrors the authoritative repo docs at
`/home/antonio/KovanicaDAG/kovanica-protocol` (checked 2026-09-08, branch
`main` equivalents of the shipped RFC-005 work).

- `kovanica-protocol/README.md` — repo readme (build/test, light-node quickstarts)
- `kovanica-protocol/AGENTS.md` — conventions, layout, roadmap (**source of truth**)
- `kovanica-protocol/TESTNET.md` — deployed testnet status
- `kovanica-protocol/OPERATIONS.md` — seed/ops runbook
- `kovanica-protocol/TODO.md` — known open items
- `kovanica-protocol/docs/` — KVP/RFC standards, plans, ops docs:
  - Standards: [[RFC-001-Multisig]] · [[RFC-002-NativeTokens]] · [[RFC-003-ScriptV2-and-Stealth]] · [[RFC-004-Htlc]] · [[RFC-005-Vault]] · [[KVP]]
  - Tokenomics / overview: [[TOKENOMICS]] · [[WHAT-IS-KOVANICA]] · [[KVP-102-NativeTokens]]
  - Plans: [[vault-time-lock]] · [[htlc-atomic-swap]] · [[stealth-script-v2-rfc-003]] · [[mobile-light-node]] · [[android-light-node-app]]
  - Release / ops: [[RELEASE]] · [[LEGIT-BOARD]] · [[soak-snapshot-2026-09-03]] · [[explorer]] (api docs)

## kovanica-agent snapshot

The `kovanica-agent/` folder mirrors the authoritative **Kovi** agent repo at
`/home/antonio/protokol/kovanica-agent` (GitHub `KovanicaDAG/kovanica-agent`,
checked 2026-09-09, `main`).

- [[KOVI]] — vault-authored overview (architecture, stack, status)
- `kovanica-agent/README.md` — build/run, architecture
- `kovanica-agent/SYSTEM_PROMPT.md` — agent system prompt
- `kovanica-agent/docker-compose.yml` / `docker-compose.cpu.yml` — stack wiring
- `kovanica-agent/agent/` — FastAPI + LangGraph + RAG + apply-to-PR source
- `kovanica-agent/sandbox/` — cargo sandbox, socket-owning runner, build helper

## Old vault archive

The previous vault snapshot (pre-merge four-repo layout: `kovanica-cli/`, `kovanica-ledger/`,
`kovanica-node/`, `kovanica-web/`) is *not* carried into this vault — those embedded
git clones are stale and contradicted by the merged repo. The checkout remains at
`/home/antonio/KovanicaDAG/KovanicaDAG/KovanicaDAG/` if historical context is needed.

---

*Updated: 2026-09-09 (added kovanica-agent / Kovi snapshot).*