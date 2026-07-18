# Workspace boundaries and local reuse

The organizing constraint is correct ownership and imports, not publication. These projects remain
independent repositories in one coordinated local folder.

## Principles

1. Extract mechanics only when at least two consumers genuinely share them.
2. Keep domain policy in the application that can explain and test it.
3. Make local dependencies explicit and fail closed when a sibling is absent.
4. Keep one lockfile and one quality gate per repository; do not create a root dependency universe.
5. Copy starters, import libraries, call services, and study final research artifacts—do not blur
   those four reuse modes.
6. Do not force a mature or frozen research repository into the newest template.

## Current Node dependency graph

```text
ts-quality ---------------------> Agent Blocks, Drone, Nexus, Templar (development)
node-guardrails ----------------> Agent Blocks, Drone, Nexus, Templar (runtime primitives)
drone-client -------------------> Templar (Drone HTTP contract)
agent-blocks/root --------------> Templar (member/team/organization composition)
agent-blocks/scoped-worktree ---> Templar (orchestration template)
```

The reusable modules use private `@agentic-orch/*` names. A consumer declares:

```json
{
  "dependencies": {
    "@agentic-orch/agent-blocks": "workspace:*",
    "@agentic-orch/drone-client": "workspace:*",
    "@agentic-orch/node-guardrails": "workspace:*"
  },
  "devDependencies": {
    "@agentic-orch/ts-quality": "workspace:*"
  }
}
```

and lists only the required siblings:

```yaml
packages:
  - "."
  - "../agent-blocks"
  - "../drone-client"
  - "../node-guardrails"
  - "../ts-quality"

linkWorkspacePackages: true
sharedWorkspaceLockfile: false
```

The consumer lock keeps `link:../...` resolutions. `workspace:*` prevents a similarly named npm
package from satisfying the dependency and documents that the coordinated folder is required.

Agent Blocks has two layers:

- the root export is generic agents, runtimes, templates, members, teams, and organizations;
- `templates/scoped-worktree` owns the existing Codex, Git worktree, evaluator, reviewer, and
  candidate-selection policy.

Consumers must import the narrow subpath that matches what they use. Templar uses the generic root
for organization composition and the scoped-worktree subpath for harness contracts. Its security
team names, phases, member assignments, and fail-closed runtime guard remain application policy.

## Python sibling dependencies

Python consumers keep a conventional project requirement and make the local source choice explicit
to uv:

```toml
[project]
dependencies = ["benchmark-audit-core>=0.1,<0.2"]

[tool.uv.sources]
benchmark-audit-core = { path = "../benchmark-audit-core", editable = true }
```

AgentTrace, Probe, and Agent Benchmark Lab follow this pattern. `benchmark-audit-core` remains
unchanged. The CS-E4645 course repository is a frozen reference and no longer receives the new
integration work. Cross-language reuse uses HTTP or artifact contracts; a Python project never
imports a TypeScript workspace module.

## Automation boundary

The private catalog checkout records every sibling as an exact Git submodule commit, so it can
materialize the local `workspace:*` and uv path layout without a package registry. A standalone
repository checkout still cannot invent its siblings: its CI must explicitly check out each needed
repository at an immutable tag or full commit. Run each repository's native gate from the
coordinated folder until its standalone workflow performs those checkouts. An npm or PyPI release is
not required.

Source releases use independent semantic versions and annotated `vMAJOR.MINOR.PATCH` tags. See
`VERSIONING.md`; the catalog itself versions only topology, documentation, and pinned revisions.

## Ownership map

| Boundary | Owns | Does not own |
| --- | --- | --- |
| Agent Blocks | Agent composition and named orchestration templates. | Provider routing, sandbox service policy, application prompts. |
| Node Guardrails | Neutral bounded process, HTTP, and CAS primitives. | Application schemas, authorization, domain failures. |
| Drone Client | Drone's Node wire contract and transport hardening. | Drone server implementation or operation policy. |
| TypeScript Quality | Repeated repository mechanics and presets. | Consumer tests, builds, coverage, native checks. |
| Benchmark Audit Core | Portable evidence and deterministic gate primitives. | Running benchmarks, retrieving evidence, persistence, domain promotion. |
| Agent Benchmark Lab | Safe trace capture, outcome semantics, evidence commands, and audit-gate workflow. | Shared schemas, experiment runners, course artifacts, datasets, or scientific claims. |
| Observer | Provider-neutral dispatch, attempts, usage, pricing provenance. | Trace-lake storage or managed-gateway internals. |
| AgentTrace | Immutable traces and derived datasets/evaluations. | Retry ownership, billing authority, operational dispatch. |
| Nexus | Inbound gateway policy, aliases, target equivalence, attempt ledger. | Agent orchestration or experiment semantics. |
| Drone | Trusted operation admission and sandbox execution. | Caller-selected images, commands, mounts, or host paths. |
| Templar | Security team plans, assignment enforcement, workflow policy, human gates, and results. | Generic organization primitives or sandbox implementation. |

## Managed gateway boundary

Vercel AI Gateway can be a direct managed transport or one Nexus target. Nexus remains valuable for
owned authentication, stable aliases, local/private targets, and deterministic policy. If a managed
gateway owns provider fallback, Nexus and Observer must not add another automatic fallback layer for
that same logical call. Retain requested and resolved model/provider/route evidence for benchmarks.

See `nexus/MANAGED_GATEWAYS.md` and `observer/docs/MANAGED_GATEWAYS.md`; both describe integration
boundaries, not a currently enabled Vercel deployment.

## Repository posture

- `agent-benchmark-lab`, `probe`, `agent-trace-platform`, and the science-replication starter reflect the
  current reproducibility direction.
- `cs-e4645-data-intensive-computing` is a frozen final artifact and keeps its report/execution split.
- `observer` is in a large active migration and should be changed through its own contracts.
- `alphaevolve` was the clear structural exception; its implementation now lives under one
  `src/alphaevolve` namespace.
- Agent Blocks and Nexus use their generic GitHub slugs; their package and service identities match
  the coordinated checkout.
