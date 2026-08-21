# Operational guidelines

These guidelines help maintainers run a template repository with clear ownership and durable governance.

## Ownership

Each template repository **SHOULD** have:

- at least one named maintainer responsible for readiness and publication,
- at least one additional maintainer or administrator who can handle reviews, settings, and urgent fixes,
- clear ownership for major technical areas when the template spans multiple languages, frameworks, or Azure services.

## Planning and backlog management

Maintainers **SHOULD** keep planning artifacts in GitHub so work stays reviewable and searchable:

- issues for bugs, features, and follow-up work,
- milestones or GitHub Projects for larger efforts,
- pull requests linked to the issues they resolve.

Labels are optional, but they help when they reflect real work dimensions such as infrastructure, frontend, backend, documentation, security, or blocked work.

## Repository governance

- Changes to the default branch **SHOULD** flow through pull requests.
- The default branch **SHOULD** be protected with branch protection or rulesets.
- Important decisions **SHOULD** be recorded in issues, pull requests, or docs so new maintainers can reconstruct the reasoning later.

## Published template maintenance

After a template is published, maintainers **SHOULD**:

- keep docs aligned with the current deployable scenario,
- keep validation and automated checks healthy,
- triage incoming issues,
- make breaking changes intentionally and document migration or replacement guidance when needed.

If the repository uses collection automation such as [microsoft/template-validation-action](https://github.com/microsoft/template-validation-action/blob/main/README.md), make sure maintainers know where its results appear and how to act on them.
