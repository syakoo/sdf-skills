# sdf-skills

Agent skills for the Structured Development Flow. The skills live in `sdf-requirements/`, `sdf-design/`, and `sdf-implement/`; the README describes them.

## Releases

Every change to the skills ships as a release, so installed copies can be pinned to a tag.

- Every PR to `main` carries exactly one `semver:major` / `semver:minor` / `semver:patch` label. Never leave a PR unlabeled: it would merge without a release.
- Choose the level by what an installed skill's user sees: `patch` for wording, fixes, and docs/CI; `minor` for a new or changed step, section, or artifact in a skill; `major` for a change that breaks documents or workflows made with the earlier version.
- Merging publishes the next `vX.Y.Z` release and tag (`.github/workflows/release-on-merge.yml`). There is no version file; do not add one, and do not create tags by hand.
