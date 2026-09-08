# AGENTS.md — Vault Sync Guide

Guidance for AI assistants (and humans) maintaining **this Obsidian vault**
(`/home/antonio/protokol/kovanica-vault`). It mirrors the `kovanica-protocol`
repo's AGENTS.md doctrine — keep this file in sync with the code repo.

## What this vault is

A documentation vault for the **kovanica-protocol** project. The
`kovanica-protocol/` folder is a **snapshot** of the authoritative repo's docs
(README, AGENTS, TESTNET, OPERATIONS, TODO, and the full `docs/` tree).
`myObsidianVaultDAG.md`, `ROADMAP.md`, `CODE_INDEX.md` and `NAVIGATION.md` are
vault-authored notes; everything else is copied from the repo.

## Authority Map

| Topic | Authoritative Source |
|-------|---------------------|
| Protocol/code design, build, test | `/home/antonio/KovanicaDAG/kovanica-protocol` — read its `AGENTS.md` first |
| Deployed testnet ops | `/home/antonio/KovanicaDAG/kovanica-protocol/TESTNET.md`, `OPERATIONS.md` (verify against the repo) |
| Project overview as presented in Obsidian | `myObsidianVaultDAG.md` |
| Roadmap / status tracking | `ROADMAP.md` (keep in sync with repo `AGENTS.md` roadmap) |

## Responsibilities

1. **Sync documentation** from `kovanica-protocol` → `kovanica-protocol/` snapshot
   (README, AGENTS, TESTNET, OPERATIONS, TODO, docs tree)
2. **Update the project overview** (`myObsidianVaultDAG.md`) when the merged repo
   structure or status changes
3. **Maintain `CODE_INDEX.md`** with current `file://` links to source files
4. **Update `ROADMAP.md`** and other tracking documents when stages complete
5. **Push changes** to the remote repository following the sync recipe

## Remote

The vault is a git repo on branch `main`. Remote is GitHub
`KovanicaDAG/kovanica-vault` (create on first sync if needed).

## Never Commit

- `.obsidian/`, `.trash/`, `.claudian/` — ignored (app state, session data)
- Embedded git clones / nested `.git` directories
- Never create submodules for `kovanica-*` doc folders

## Working Notes

- **Do not invent** APIs, paths, commands, or roadmap items. If a fact isn't
  verifiable in the repo or these docs, say so.
- Verify before claiming: run commands, don't assume output.
- The snapshot under `kovanica-protocol/` describes the **merged repo** layout;
  old per-repo snapshots (`kovanica-cli/ledger/node/web`) from the pre-merge era
  were **not** carried into this vault (stale, contradicted by the merged repo).

## Sync Recipe

1. Make requested edits in this vault
2. `git add -A && git commit -m "docs: <what changed>"`
3. If push rejected: `git pull --rebase origin main`, resolve if needed
4. `git push origin main`

Commit subjects: imperative, prefixed `docs:` (e.g.,
`docs: sync vault snapshot (RFC-005 shipped)`).

## Code Edits

Code changes happen in `/home/antonio/KovanicaDAG/kovanica-protocol` under that
repo's `AGENTS.md` (feature branch + draft PR, `cargo fmt --check && cargo clippy
--all-targets && cargo test` before pushing). Never commit code from this vault.