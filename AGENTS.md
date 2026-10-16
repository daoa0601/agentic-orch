# Workspace context

This folder is a catalog of independent repositories, not a monorepo. Projects are grouped by
dependency (`node-stack/`, `eval-stack/`, `research/`, `archive/`, `templates/`); see README.md for
the layout and the cross-repo boundaries, which are the one place for policy spanning repositories.
Read the target repository's own README/AGENTS.md before editing it and run its own checks. Do not
add a root lockfile or bulk-upgrade every project to one toolchain.

Project directories are git submodules with independent histories; a catalog commit may advance a
submodule pointer or move a project between groups but must not collapse project source into the
root repository. Never commit `.codex-subagents/` or local `.env` state.

Siblings are reused only as source checkouts (`workspace:*` with `pnpm-workspace.yaml` entries, or
uv path sources). Nothing is published to a registry: don't add pack checks, release procedures,
or registry metadata. A sibling dependency must stay inside its group folder. A project that
depends on nothing here, and isn't agent infrastructure, belongs outside this folder.

`benchmark-audit-core` is a stable reference API: don't reorganize it. CS-E4645 is a finalized
research artifact. Observer is mid-migration. Avoid broad mechanical changes in them unless the
task explicitly targets them.

Delegate only to Opus 5.5 (`opus`) for Claude subagents and workflows, and Sol 6.1 (`gpt-6.1-sol`,
via `codex exec`) for an independent second opinion. Do not use Sonnet, Haiku, Fable, or any other
model.
