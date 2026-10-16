# Agentic orchestration workspace

A coordinated checkout of independent repositories for building, running, and evaluating agent
systems. Each project is a git submodule with its own history, environment, lockfile, and checks.
Nothing here is published to a package registry; projects reuse each other only as sibling source
checkouts.

```bash
git clone --recurse-submodules https://github.com/daoa0601/agentic-orch.git
# or, in an existing clone:
git submodule update --init --recursive
```

## Layout

Projects are grouped by dependency. Every sibling dependency stays inside its group folder, and no
dependency crosses groups. [repositories.toml](repositories.toml) is the machine-readable list.

| Group | Project | What it is |
| --- | --- | --- |
| `node-stack/` | `ts-quality` | Shared TypeScript presets, secret scan, and git hooks (dev tooling) |
| | `node-guardrails` | Bounded no-shell process, bounded HTTP/JSON, content-addressed files |
| | `agent-blocks` | Agent, runtime, team, and organization building blocks; scoped-worktree template |
| | `drone-client` | Drone v1 wire contract and hardened Node client |
| | `drone` | Policy-gated sandbox service running one no-network VM per job |
| | `nexus` | Authenticated OpenAI-compatible LLM gateway |
| | `templar` | Security-analysis application built on the libraries above, calling Drone over HTTP |
| `eval-stack/` | `benchmark-audit-core` | Audit schemas, canonical hashing, seals, deterministic headline gate |
| | `agent-trace-platform` | AgentTrace trace lake: collector, SDK, storage, pipelines, datasets |
| | `agent-benchmark-lab` | Runs and gates agent benchmarks; writes traces to AgentTrace |
| | `probe` | Evidence-first inference-optimization lab for Apple Silicon |
| `research/` | `alphaevolve` | LLM-driven evolutionary code optimization prototype |
| | `observer` | Provider-neutral model execution, usage, and pricing provenance (mid-migration) |
| `archive/` | `cs-e4645-data-intensive-computing` | Frozen course research artifact; study, don't extend |
| `templates/` | `science-replication` | Starter for a computational paper replication |

Dependency edges:

```text
node-stack   node-guardrails ──> agent-blocks, drone, nexus, templar
             drone-client ─────> drone, templar
             agent-blocks ─────> templar
             ts-quality ───────> every Node repo (dev only)
             drone ············> templar (HTTP only)
eval-stack   benchmark-audit-core ──> agent-trace-platform, agent-benchmark-lab, probe
             agent-trace-platform ──> agent-benchmark-lab
```

`research/`, `archive/`, and `templates/` projects stand alone.

## Working in a repository

Read the target repository's README and AGENTS.md, then run its own checks from inside it. There is
no root package manager, lockfile, or all-projects command.

## Cross-repo boundaries

This section is the single home for policy that spans repositories. Project docs link here instead
of restating it.

**Sibling dependencies.** Node consumers declare `"@agentic-orch/<name>": "workspace:*"` and list
only the siblings they need in their own `pnpm-workspace.yaml` (`../<name>`). Python consumers
declare the dependency normally and point uv at the sibling:

```toml
[tool.uv.sources]
benchmark-audit-core = { path = "../benchmark-audit-core", editable = true }
```

A missing sibling fails the install; there is no registry fallback. Cross-language reuse goes
through HTTP or file-artifact contracts, never a cross-language import.

**CI.** A repository's CI checks out the siblings it needs at their default branch next to itself.
A repository that depends on a private sibling (agent-benchmark-lab) has no hosted CI, because a
workflow token can't read another private repository; its local `make check` is the gate. The
submodule pins in this catalog are the reproducible record of a known-good combination.

**One retry owner per call.** Exactly one layer owns automatic retries and fallback for a logical
model call:

| Need | Use |
| --- | --- |
| Supported hosted models with managed provider credentials and availability routing | A managed gateway (e.g. Vercel AI Gateway) directly |
| Local, private, offline, or explicitly versioned deterministic targets | Nexus |
| Provider-neutral application execution with retries, circuits, usage, and pricing | Observer, over one transport |
| One operator-authenticated local CLI inside a bounded Agent Blocks run | Agent Blocks' Codex/OpenCode adapter |
| Reproducible benchmark traffic | One pinned route and one retry owner, with the resolved route recorded |

If a managed gateway owns fallback, Nexus and Observer must not add another fallback layer for that
call. Callers above a gateway or Nexus don't wrap the same call in their own retry loop either,
unless the operation is explicitly safe to repeat and the extra billable work is accepted.
AgentTrace records what happened. It is not a billing or retry authority.

**Trace correlation.** One logical call carries one externally safe correlation ID across the
caller, any gateway, and AgentTrace. Trace exporters emit derived spans; they don't replace a
service's own attempt ledger.

## Starting something new

- Paper replication: export `templates/science-replication` into a new repository (see its README).
  Then add only what the experiment needs: `benchmark-audit-core` for evidence and gates,
  `agent-benchmark-lab` to run and gate benchmarks, AgentTrace for durable traces, Agent Blocks for
  agent composition, Drone for sandboxed execution, and Nexus for owned routing.
- A project that doesn't depend on anything here belongs outside this folder.
