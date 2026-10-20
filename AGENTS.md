# Agent Instructions for Plus3 IT Repositories

This file provides instructions for AI agents working with Plus3 IT repositories. Every repository created from this template inherits these instructions.

## Overview

Plus3 IT repositories follow a standardized structure using tardigrade-ci tooling for development, testing, and releases. Most repositories are hosted on GitHub and use reusable workflows from plus3it/actions-workflows.

## Before you finish

Before reporting your work as complete:

1. Run `make lint` to catch formatting and style issues
2. Run tests as described in the Repository-specific → Testing section
3. For user-visible changes in repos with `.bumpversion.cfg`:
    - Update `CHANGELOG.md` following [Keep a Changelog](http://keepachangelog.com/) format
    - Commit your changes, including the CHANGELOG.md entry
    - Run the appropriate version bump (`make bumpversion/patch`, `minor`, or `major`), which creates its own commit
    - Both commits go in the same pull request
4. Regenerate any maintained documentation (e.g., `make docs/generate` for Terraform module READMEs)
5. In your final report, list everything a person must still do: pushes, pull requests, applies, account or credential setup, and anything you could not verify

## Tooling

Repositories created from this template include tardigrade-ci, a Docker-based test framework that provides standardized make targets for development tasks.

### Available Commands

Run `make help` to see all available targets. Common targets include:

- `make lint` — Runs all linters (editorconfig, YAML, JSON, shell scripts, Python, Terraform, HCL)
- `make test` — Runs tests (typically Terraform pytest via mockstack)
- `make docker/run target=<TARGET>` — Runs a make target inside the tardigrade-ci Docker container
- `make <lang>/format` — Formats files (e.g., `make python/format`, `make terraform/format`, `make hcl/format`, `make json/format`)

### Docker Image and Makefile

The `Dockerfile` at the repository root pins the tardigrade-ci version with a `FROM plus3it/tardigrade-ci:<version>` line. The repository's dependency bot (Dependabot or Renovate; see Repository Automation) updates this pin. The Makefile bootstrap reads the same pin to clone the matching tardigrade-ci version.

The repository Makefile includes tardigrade-ci's bootstrap:

```makefile
include $(shell test -f .tardigrade-ci || curl -sSL -o .tardigrade-ci "https://raw.githubusercontent.com/plus3it/tardigrade-ci/master/bootstrap/Makefile.bootstrap"; echo .tardigrade-ci)
```

This bootstrap automatically clones or updates the tardigrade-ci tooling into a local subdirectory based on the Dockerfile pin.

## Release Process

Use bump2version to increment the version in `.bumpversion.cfg`:

```bash
make bumpversion/patch   # For bug fixes (1.2.3 → 1.2.4)
make bumpversion/minor   # For new features (1.2.3 → 1.3.0)
make bumpversion/major   # For breaking changes (1.2.3 → 2.0.0)
```

This command updates `current_version` in `.bumpversion.cfg` and commits the change (the config has `commit = True` and `tag = False`). Update `CHANGELOG.md` in the same PR following [Keep a Changelog](http://keepachangelog.com/) format.

On GitHub, when the PR merging the version bump is pushed to the default branch (main or master), the release workflow (`.github/workflows/release.yml`) runs automatically. The workflow creates a GitHub Release and tag named after `current_version` ONLY if that version differs from the latest existing tag. Release notes are generated automatically by GitHub, not extracted from CHANGELOG.md. On other hosts, see Repository-specific → CI/CD for the release process.

## Repository Automation

Dependency updates depend on where the repository is hosted:

- **GitHub**: Dependabot (`.github/dependabot.yml`) opens update PRs, and Mergify (`.mergify.yml`) auto-approves and merges Dependabot PRs. Mergify has no rules for other PRs.
- **Other hosts (for example, GitLab)**: Renovate (`renovate.json`) opens update merge requests, run by the host's CI. Dependabot and Mergify do not work there.

Check which of these files the repository has before changing dependency automation.

## Formatting

All repositories follow `.editorconfig` rules. Validate formatting with `make ec/lint`.

## Repository-specific

<!--
  INSTRUCTIONS FOR REPOSITORY MAINTAINERS:
  Fill in the sections below for your specific repository. Remove this comment and any
  placeholder sections that don't apply. Keep the pointer files (CLAUDE.md, .gemini/settings.json)
  if your team uses those AI tools, or remove them if not.
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
