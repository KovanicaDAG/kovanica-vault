# Kovi — Kovanica Engineering Agent

> **Kovi, Product of Kovanica.** AI engineering assistant for the
> kovanica-protocol codebase. Standalone repo:
> https://github.com/KovanicaDAG/kovanica-agent ·
> this vault folder mirrors the repo (checked 2026-09-09, `main`).

## What it is

A RAG-powered agent that helps the DevTeam work on the Rust protocol codebase
and answers questions about the protocol, testnet, and tooling. It combines:

- **Chat / confirm API** (`agent/main.py`): FastAPI `/chat`, `/confirm`, `/healthz`.
- **LangGraph orchestration** (`agent/graph.py`) with SQLite-checkpointed
  persistent sessions (`agent/checkpoint.py`).
- **RAG codebase search** (`agent/rag.py` + `agent/indexer.py` + `agent/embed.py`):
  Rust-aware chunking (fn/impl/struct/enum/trait/mod) + markdown section
  chunking, embedded via fastembed `BAAI/bge-small-en-v1.5` (384-dim) into a
  Qdrant `kovanica_codebase` collection.
- **Sandboxed cargo runs** (`agent/sandbox_client.py` + `sandbox/runner/server.py`):
  `run_cargo_command` executes inside an ephemeral, **network-disabled** docker
  container. Only the `sandbox-runner` sidecar mounts `/var/run/docker.sock`
  (single least-privileged owner); agent-api POSTs a whitelisted cargo command
  to it. Commands allowed: `check|test|clippy|build`.
- **Human-gated apply → draft PR** (`agent/patchstore.py` + `agent/apply.py`):
  proposes patches into a SQLite patchstore; on `/confirm` (dev role only)
  validates, applies to a throwaway git worktree off `origin/main`, pushes, and
  opens a **draft PR** via `gh`. Never mutates the main checkout. Fail-closed:
  unless `AGENT_GIT_APPLY_ENABLED=1` it only validates/reports (`dry_run`).
- **Auth** (`agent/auth.py`): JWKS mode (`AUTH_JWKS_URL`) or dev shared-secret
  mode (`AUTH_DEV_TOKEN`). Unset = everyone is `user` (fail-closed for dev). A
  Bearer `<AUTH_DEV_TOKEN>` request maps to the `dev` role needed for `/confirm`.

## Stack (docker-compose)

| Service | Role | Image |
|---------|------|-------|
| `vllm` (GPU `docker-compose.yml`) | OpenAI-compatible LLM serving | vLLM, `Qwen/Qwen2.5-Coder-32B-Instruct-AWQ` |
| `vllm` (CPU override `docker-compose.cpu.yml`) | Ollama on port 11434, `qwen2.5-coder:3b` | `ollama/ollama` |
| `qdrant` | Vector store (`kovanica_codebase`) | `qdrant/qdrant` |
| `agent-api` | FastAPI `/chat` `/confirm` | local build (`agent/Dockerfile`) |
| `sandbox-runner` | Owns `/var/run/docker.sock`, spawns sandboxes | local build |
| `open-webui` | Chat web UI | `ghcr.io/open-webui/open-webui` |
| `sandbox-image` | Build-only ephemeral cargo env | local build (profile `build-only`) |

CPU-only host ports differ from the base file (see the compose
`!override`s): open-webui → host **13000**, agent-api → host **13080**,
Ollama → host **11434**. On a GPU host, open-webui → :3000, agent-api →
:8080, vLLM → :8000.

## Running it (summary)

```bash
# in-tree (kovanica-agent/ inside kovanica-protocol):
docker compose --profile build-only build sandbox-image      # pre-vendor deps
docker compose up -d vllm qdrant sandbox-runner agent-api open-webui
# CPU-only:
docker compose -f docker-compose.yml -f docker-compose.cpu.yml up -d

# standalone checkout:
KOVANICA_PROTOCOL_ROOT=/path/to/kovanica-protocol ./sandbox/build-image.sh

# model + index (once):
docker exec kovanica-agent-agent-api-1 python /app/indexer.py --repo /repos/kovanica-protocol
```

Set `AUTH_DEV_TOKEN` in `.env`. Chat: `POST /chat` with
`{"session_id","message"}` and `Authorization: Bearer <AUTH_DEV_TOKEN>`.

## Status / open items (from README)

- ✅ Pre-vendored crate deps into the sandbox image (`cargo fetch`) so
  `--offline` cargo calls succeed.
- 🔲 gVisor (`runsc`) on the host, uncomment `runtime="runsc"` in
  `sandbox/runner/server.py`.
- 🔲 Harden sandbox-runner before opening beyond a trusted DevTeam (socket
  proxy, token on `POST /run`).
- 🔲 Real auth (`AUTH_JWKS_URL` + Keycloak or strong `AUTH_DEV_TOKEN`) before
  any non-localhost exposure.
- 🔲 Arm the apply→PR path: git identity + `gh` auth + `AGENT_GIT_REPO` +
  `AGENT_GIT_APPLY_ENABLED=1`.
- 🔲 Auto-index on merge (GitHub webhook — no receiver exists yet).

## Files

- `README.md` — build/run, architecture
- `SYSTEM_PROMPT.md` — agent system prompt (vocab, citation rule, safety)
- `docker-compose.yml` / `docker-compose.cpu.yml` — stack wiring (GPU / CPU)
- `agent/` — FastAPI app: `main.py` (routes), `graph.py` (LangGraph), `rag.py`
  + `indexer.py` + `embed.py` (RAG), `auth.py` (JWT), `apply.py` +
  `patchstore.py` (draft-PR path), `checkpoint.py` (SQLite sessions),
  `sandbox_client.py` (sandbox delegation)
- `sandbox/` — `Dockerfile` (cargo exec env), `entrypoint.sh` (in-box
  whitelist), `runner/` (socket-owning sidecar), `build-image.sh` (standalone
  build helper)

---
*Updated: 2026-09-09.*