# Workspace context

This folder contains independent repositories, not a monorepo. Read the target repository's own
`AGENTS.md` before editing it and run that repository's native checks. Do not add a root lockfile or
bulk-upgrade every project to one toolchain.

The root Git repository is a catalog and coordinated checkout. Project directories are Git
submodules with independent histories; a catalog commit may advance a submodule pointer but must not
collapse project source into the root repository. Never commit `.codex-subagents/` or local `.env`
state.

Local Node modules use unpublished `@agentic-orch/*` names with `workspace:*` and sibling workspace
entries. Repository visibility does not imply registry publication. Python sibling reuse uses
explicit uv path sources. Cross-language reuse goes through a versioned HTTP or artifact boundary.

`benchmark-audit-core` is a reusable reference that should not be reorganized with the rest of the
folder. The CS-E4645 repository is a finalized research artifact. Observer is mid-migration. Avoid
broad mechanical changes in either unless the task explicitly targets them.

For a new paper replication, start from `templates/science-replication`, then consult
`REUSE_CATALOG.md` and add only the components the experiment actually needs.
