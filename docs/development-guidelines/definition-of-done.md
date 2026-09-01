# Definition of done (DoD)

A template is done when a new contributor can discover it, understand the scenario, run the documented setup, and successfully deploy the intended experience with Azure Developer CLI (`azd`).

Use this checklist before requesting publication or declaring a major template update ready.

## How to use this checklist

- Record evidence for each **MUST** item in the PR, issue, or workflow run where you completed it.
- Treat **SHOULD** items as the default expectation unless you have a documented reason not to apply them.
- Treat **MAY** items as optional polish.

For the shared meaning of **MUST**, **SHOULD**, and **MAY**, see [docs/readme.md](../readme.md).

## MUST

### Core `azd` functionality

- [ ] `azure.yaml` exists in the template repository root.
- [ ] Infrastructure assets appropriate to the template are committed (for example `infra/`, or a framework-specific equivalent).
- [ ] The documented `azd up` path has been executed successfully for the supported happy path.
- [ ] If the template provisions Azure resources, the documented teardown path has been verified (`azd down` or an explicitly documented equivalent).

### Awesome AZD repository baseline

These items are **MUST** requirements when preparing a template for the public Awesome AZD collection. They are not universal requirements for every private or standalone `azd` project.

- [ ] `README.md` explains the scenario, prerequisites, deployment steps, verification steps, and cleanup guidance.
- [ ] `README.md` contains the `## Important Security Notice` policy heading and the literal H2 headings `## Features`, `## Getting Started`, `## Guidance`, and `## Resources` required by the collection validator.
- [ ] `LICENSE.md`, `SECURITY.md`, `CONTRIBUTING.md`, `.github/CODE_OF_CONDUCT.md`, and an issue template are present.
- [ ] The repository description is set, and the repository topics include `azd-templates` and `ai-azd-templates` along with relevant language, model, and technology topics.

### Review and publication readiness

- [ ] The repository can point reviewers to evidence for the latest successful validation run, test run, or manual verification.
- [ ] The Awesome AZD security validation using [PSRule for Azure](https://azure.github.io/PSRule.Rules.Azure/features/#learn-by-example) passes without warnings. `microsoft/template-validation-action` uses PSRule by default.
- [ ] If the template is being submitted to Awesome AZD, the required gallery metadata and assets are prepared as described in [publishing-guidelines.md](../../publishing-guidelines.md).

## SHOULD

- [ ] The repository uses the copyable structure in [docs/template-readme.md](../template-readme.md) or an equivalent README structure tailored to the template.
- [ ] Pull requests exercise the same documented setup path that end users follow.
- [ ] Tests cover the deployable application or infrastructure logic that changed.
- [ ] Security guidance explains how the template handles authentication, authorization, secrets, and operator setup.
- [ ] When Azure services support Microsoft Entra ID or managed identities for the scenario, the template prefers them or documents why an alternative is required.
- [ ] If you use [microsoft/template-validation-action](https://github.com/microsoft/template-validation-action/blob/main/README.md), its latest relevant run is green before submission. This action is Awesome AZD collection automation, not a generic `azd` requirement.

The validation action checks for an `azure-dev` workflow and `.devcontainer` by default, and `useDevContainer` defaults to `true`. If those optional assets are intentionally omitted, configure the action step to validate only the paths the repository supports and disable dev-container execution:

```yaml
with:
  validatePaths: "README.md,LICENSE.md,SECURITY.md,CONTRIBUTING.md,CODE_OF_CONDUCT.md,ISSUE_TEMPLATE.md,azure.yaml,infra"
  useDevContainer: false
```

Adjust `validatePaths` when the template uses a framework-specific infrastructure path or a different supported repository layout.

## MAY

- [ ] Include a dev container or GitHub Codespaces configuration when it materially reduces setup friction.
- [ ] Include CI/CD automation such as `azd pipeline config` output when it matches the repository's delivery model.
- [ ] Include screenshots, demo videos, or architecture diagrams that make the scenario easier to evaluate.
