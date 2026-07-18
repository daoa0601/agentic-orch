# Reuse catalog

Use this catalog to decide whether a new project should import a local module, call a service, copy a
template, or only study an existing research artifact.

## Start a paper replication

1. Copy [`templates/science-replication`](templates/science-replication/README.md) into a new Git
   repository and rename its placeholder package.
2. Record the paper URL, exact upstream revisions, datasets, hypotheses, and acceptance criteria
   before running experiments.
3. Add only the components needed by the experiment:
   - import `benchmark-audit-core` for portable evidence, digests, seals, and headline gates;
   - import Agent Benchmark Lab when the project needs a ready-made evidence, trace, outcome, and
     audit-gating workflow around those core contracts;
   - emit to AgentTrace when durable cross-run traces or datasets are actually required;
   - use Agent Blocks for generic agents or explicit block/member/team/organization ownership,
     opting into `scoped-worktree` only for coding candidates and reviews;
   - call Drone when candidate execution needs a separately enforced sandbox;
   - route through Nexus when owned aliases, local/private targets, or deterministic routing matter;
   - use a managed gateway directly when its supported hosted catalog is the desired boundary.
4. Keep raw observations immutable, write derived outputs to a new run directory, and make the
   README's smoke command exact.

The starter is informed by
[`taclab-research/science-repo-practices`](https://github.com/taclab-research/science-repo-practices),
with local choices documented in its `UPSTREAM.md`.

## Importable local modules

| Module | Import when you need | Do not put here | Current consumers |
| --- | --- | --- | --- |
| [`@agentic-orch/agent-blocks`](agent-blocks/README.md) | Generic agents, runtimes, templates, explicit member/team/organization ownership, run persistence, the opt-in scoped Git-worktree workflow, or its local Codex/OpenCode CLI adapters. | Team semantics, application prompts, provider routing, sandbox service policy, or research-domain claims. | Templar; new orchestrators can use the generic root. |
| [`@agentic-orch/node-guardrails`](node-guardrails/README.md) | No-shell bounded processes, bounded HTTP/JSON helpers, or verified content-addressed files. | Route schemas, authorization, application errors, or domain storage models. | Agent Blocks, Drone, Nexus, Templar. |
| [`@agentic-orch/drone-client`](drone-client/README.md) | Typed Drone v1 HTTP contracts and the hardened Node client. | Drone providers, operation policy, stores, or native workers. | Templar. |
| [`@agentic-orch/ts-quality`](ts-quality/README.md) | Shared TypeScript presets, secret scanning, package-boundary checks, and managed hooks. | A repository's coverage floor, tests, build, or native jobs. | Agent Blocks, Drone, Nexus, Templar. |
| [`benchmark-audit-core`](benchmark-audit-core/README.md) | Portable Python audit schemas, canonical hashing, evidence references, seals, report rendering, and deterministic headline gates. | Evidence retrieval, telemetry, benchmark execution, persistence, or a domain's promotion policy. | AgentTrace, Probe, Agent Benchmark Lab. Intentionally unchanged in this reorganization. |
| [`agent-benchmark-lab`](agent-benchmark-lab/README.md) | A fresh-start evidence CLI, bounded local traces, judge-aware outcome summaries, and the workflow that applies the core gate. | Course artifacts, experiment runners, datasets, prompts, provider policy, or shared audit schemas. | New agent benchmark and paper-replication repositories. |

These modules resolve from sibling source through `workspace:*` or explicit uv paths. Some source
repositories are public after a public-readiness audit, but none requires or expects npm or PyPI
publication.

## Reusable services and systems

| Project | Reuse mode | Best fit | Source of truth it retains |
| --- | --- | --- | --- |
| [AgentTrace](agent-trace-platform/README.md) | Run or import its Python SDK. | Immutable traces, canonical episodes, evaluations, and frozen datasets. | Raw trace objects and versioned derived snapshots. |
| [Drone](drone/README.md) | Call its versioned HTTP API. | Operator-controlled execution of untrusted artifacts in bounded disposable environments. | Job, artifact, operation, and provider state. |
| [Nexus](nexus/README.md) | Run as a gateway or use its local core. | Owned authentication, aliases, deterministic target equivalence, local/private targets. | Its routing policy and attempt ledger. |
| [Observer](observer/README.md) | Extend its provider-neutral Python runtime. | Operational model execution, retry/circuit policy, usage, and pricing provenance. | Each dispatch Observer actually starts and its durable usage fact. |
| [Templar](templar/README.md) | Treat as an application/example consumer. | Policy-gated network, whole-course evidence evaluation, exercise, and source-security workflows with concrete purple, red, blue, reverse-engineering, and assurance rosters. | Templar team plans, fail-closed assignment guards, workflow inputs, artifacts, sealed grading, human gates, and results. |

Services are not libraries merely because their source is nearby. Prefer their versioned protocol
unless a documented SDK boundary exists.

## Research references and active prototypes

| Project | How to reuse it | Reorganization posture |
| --- | --- | --- |
| [Agent Benchmark Lab](agent-benchmark-lab/README.md) | Import its small Python workbench or copy its synthetic audit flow into a new experiment. | Fresh history containing only generic benchmark-integrity code; use this instead of adding new infrastructure to the course artifact. |
| [Probe](probe/README.md) | Copy its evidence-first experiment discipline, immutable run artifacts, hardware provenance, and independent audit pattern. | Mature focused research repo; keep its layout. |
| [CS-E4645](cs-e4645-data-intensive-computing/README.md) | Study the frozen benchmark, reporting, and LUMI evidence paths. | Final research artifact; do not retroactively rewrite it into the new template. |
| [AlphaEvolve](alphaevolve/README.md) | Extend the `LLMProvider` and `Evaluator` protocols for evolutionary-code experiments. | Active prototype; now uses one `src/alphaevolve` package, but algorithm/provider hardening remains ongoing. |
| [Observer](observer/README.md) | Reuse provider and execution contracts after its current migration stabilizes. | Large in-progress migration; avoid broad mechanical restructuring. |

## Choosing a model-traffic boundary

| Need | Prefer |
| --- | --- |
| Supported hosted models, managed/BYOK provider credentials, and hosted availability routing | A managed gateway such as Vercel AI Gateway directly. |
| Local, private, offline, custom compatible, or explicitly versioned deterministic targets | Nexus. |
| One operator-authenticated local CLI used inside a bounded Agent Blocks run | The explicit Codex or OpenCode adapter; keep the credential in that CLI and journal the resolved adapter/model, not the secret. |
| Provider-neutral application execution with retries, circuits, usage, and pricing provenance | Observer, with one selected transport beneath it. |
| Reproducible benchmark traffic | One pinned provider/route, one retry owner, and retained resolved-route evidence. |

Exactly one layer should own automatic retries and fallback for a logical call. AgentTrace records
what happened; it does not become the billing or retry authority.
