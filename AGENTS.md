# Agent Instructions for Plus3 IT Repositories

<!-- BEGIN plus3it-shared: shared across Plus3 IT repositories; change it in plus3it/repo-template, then copy it to the others -->
## Overview

Plus3 IT repositories use tardigrade-ci for linting, testing, and releases. Run `make help` to list every target, and prefer make targets over running tools directly. Repository-specific instructions follow this shared section.

## Before you finish

1. Run `make lint`.
2. Run the tests described in Repository-specific → Testing.
3. For user-visible changes in a repository with `.bumpversion.cfg`: add a `CHANGELOG.md` entry ([Keep a Changelog](http://keepachangelog.com/) format) and commit it with your changes, then run `make bumpversion/patch` (or `minor` or `major`), which creates its own commit. Both commits go in the same pull request.
4. Regenerate any generated docs, for example `make docs/generate` for Terraform module READMEs.
5. In your final report, list everything a person must still do (pushes, pull requests, applies, account or credential setup) and anything you could not verify.

## Tooling

- `make lint`, `make test`, and `make <lang>/format` (for example, `make terraform/format`).
- `make docker/run target=<target>` runs a target inside the pinned tardigrade-ci container.
- The `Dockerfile` line `FROM plus3it/tardigrade-ci:<version>` pins tardigrade-ci, and the Makefile bootstrap clones the same version.

## Releases

On GitHub, merging a version bump to the default branch runs `.github/workflows/release.yml`, which creates a release and tag for the new version; GitHub generates the release notes. On other hosts, see Repository-specific → CI/CD.

## Dependency automation

- **GitHub**: Dependabot (`.github/dependabot.yml`) opens update pull requests, and Mergify (`.mergify.yml`) auto-approves and merges them.
- **Other hosts (for example, GitLab)**: Renovate (`renovate.json`), run from the host's CI.

## Formatting

Follow `.editorconfig`; check with `make ec/lint`.
<!-- END plus3it-shared -->

## Repository-specific

<!--
  INSTRUCTIONS FOR REPOSITORY MAINTAINERS:
  Fill in the sections below for your specific repository. Remove this comment and any
  placeholder sections that don't apply. Don't add tool-specific instruction files such as
  CLAUDE.md; agent tools read AGENTS.md directly.
-->

### Purpose

<!-- Describe what this repository does. For Terraform modules, describe the AWS resources it manages.
For Salt formulas, describe the security controls or configurations it applies. For deployment repos,
describe which accounts and environments it manages. Keep this to 1-2 paragraphs. -->

### Layout

<!-- Describe the directory structure and key files. For Terraform modules, mention modules/, tests/,
and any special directories. For Salt formulas, mention pillar structure. For deployment repos, describe
the stacks/roots/modules organization. -->

### CI/CD

<!-- Describe how changes are tested and deployed. Common patterns:
- "PRs trigger lint and test workflows; releases are created when .bumpversion.cfg changes on the default branch"
- "Uses GitHub Actions with reusable workflows from plus3it/actions-workflows"
- "Tests run against mockstack (moto by default) for AWS resources"
For deployment repos with custom CI/CD (GitLab, Spacelift), describe the actual workflow. -->

### Testing

<!-- Describe how to run tests locally and what they test. For Terraform modules:
- "Run `make mockstack/up`, `make mockstack/pytest`, then `make mockstack/clean`, the same way CI does"
- "Tests are in tests/ and use pytest-terraform"
- "Tests use moto by default; setting `USE_LOCALSTACK` routes them through LocalStack. `ONLY_MOTO` is obsolete and has no effect"
For other repos, describe the test strategy. -->
