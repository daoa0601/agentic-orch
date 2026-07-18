# Source versioning and releases

This workspace uses independent semantic versions for source compatibility. It does not require an
npm or PyPI publication workflow. A version means “this Git source boundary passed its own checks
at this commit,” not “a package was uploaded to a registry.”

## Version sources

- Node projects read their version from `package.json`.
- Python projects read their version from `pyproject.toml`.
- The catalog version is recorded in `repositories.toml` and tags changes to the coordinated
  checkout, documentation, and pinned repository revisions.
- Projects started from `0.1.0`; versions advance independently rather than in lockstep.

While a project is below `1.0.0`, a patch release is backward-compatible and a minor release may
change an unstable API. A project reaches `1.0.0` only when its supported boundary and migration
policy are explicit.

## Release procedure

1. Run the repository's native preflight from a clean working tree.
2. Merge the reviewed change to the repository's default branch.
3. Confirm the manifest version and changelog or release notes describe the boundary.
4. Create an annotated `vMAJOR.MINOR.PATCH` tag on that verified commit.
5. Push the tag. A GitHub release is optional; registry publication is not part of this workspace.

Tags are immutable source anchors. Do not move or reuse a released tag. A fix after a tag receives a
new patch version. The root catalog pins exact submodule commits, so a catalog tag also reproduces
the sibling layout used by local imports.

## Cross-repository use

- A coordinated checkout uses the submodule commit recorded by the catalog.
- Node consumers continue to use `workspace:*`; Python consumers continue to use explicit uv path
  sources.
- A standalone CI job must explicitly check out each sibling at a tag or full commit. It must not
  silently fall back to a similarly named registry package.
- HTTP and artifact contracts carry their own schema/API version independently of the source tag.

`benchmark-audit-core` is versioned as an unchanged source snapshot. Creating Git custody and a tag
does not authorize restructuring its API or files.

## Visibility and secret safety

Repository visibility is separate from licensing and versioning. New repositories stay private
until a public-readiness audit passes. Local `.env` files, credentials, caches, generated data,
machine identifiers, and internal orchestration logs are never part of a release. A suspected live
credential is rotated before any history cleanup; deleting it from the latest commit is not enough.
