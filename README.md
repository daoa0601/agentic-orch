# Agentic orchestration workspace

This folder is a collection of independent projects and reusable local building blocks. It is not a
single release unit and deliberately has no root package manager, lockfile, or registry publication
plan. Each repository keeps its own history, environment, tests, and deployment boundary.

Start with [REUSE_CATALOG.md](REUSE_CATALOG.md) when choosing components for a new project, and use
[`templates/science-replication`](templates/science-replication/README.md) as the default starting
point for a paper replication.

Clone the coordinated checkout when you want every pinned sibling available:

```bash
git clone --recurse-submodules https://github.com/daoa0601/agentic-orch.git
```

An existing clone can materialize the same layout with `git submodule update --init --recursive`.
The catalog records repository custody and current visibility in
[repositories.toml](repositories.toml); [VERSIONING.md](VERSIONING.md) defines the independent
source-tag policy. This remains a collection of repositories, not a lockstep monorepo.

## Main paths

| Area | Projects |
| --- | --- |
| Local code modules | `agent-blocks`, `node-guardrails`, `drone-client`, `ts-quality`, `benchmark-audit-core` |
| Services and applications | `agent-trace-platform`, `drone`, `nexus`, `observer`, `templar` |
| Research workbenches and projects | `agent-benchmark-lab`, `alphaevolve`, `probe`, `cs-e4645-data-intensive-computing` |
| Copyable starters | `templates/science-replication` |

The detailed dependency and ownership rules live in [STANDARDIZATION.md](STANDARDIZATION.md).

## Local dependency policy

- Node consumers use the unpublished `@agentic-orch/*` names with `workspace:*` and explicit
  sibling entries in their own `pnpm-workspace.yaml`. Repository visibility does not turn those
  source modules into registry packages.
- Python consumers keep a normal version constraint and use `[tool.uv.sources]` for an explicit
  editable sibling path.
- Cross-language reuse goes through a versioned HTTP or artifact contract, never by importing a
  TypeScript package from Python.
- `benchmark-audit-core` is intentionally left unchanged; consume its current API rather than
  reorganizing it with the rest of the folder.
- Start new benchmark-integrity work in `agent-benchmark-lab`; keep the course repository frozen as
  a historical research artifact.

Run checks from the repository you changed. There is intentionally no root command that pretends
all projects share one environment.
