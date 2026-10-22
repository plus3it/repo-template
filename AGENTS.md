# Agent Instructions

Before starting any task in this repository:

1. Run `make agents/sync`. It writes the shared Plus3 IT instructions for this repository's tardigrade-ci version to `.agents/plus3it/AGENTS.md`.
2. Read `.agents/plus3it/AGENTS.md` and follow it together with this file. If the sync fails, tell the user, and don't guess the conventions.

These rules always apply, even without the shared instructions:

- Don't push, open pull requests, publish releases, or apply infrastructure changes unless the user asks. List those steps for a person in your final report.
- Don't edit files under `.agents/plus3it/`; they are generated and overwritten by the sync.

@.agents/plus3it/AGENTS.md

## Repository-specific

<!--
  INSTRUCTIONS FOR REPOSITORY MAINTAINERS:
  Fill in the sections below for your specific repository. Remove this comment and any
  placeholder sections that don't apply. Don't add tool-specific instruction files such as
  CLAUDE.md; agent tools read AGENTS.md directly.
  
  If this repository fits one of the profiles (terraform-module, salt-formula, tardigrade-deployment),
  set AGENTS_PROFILE := <profile> in the Makefile before the include line.
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
