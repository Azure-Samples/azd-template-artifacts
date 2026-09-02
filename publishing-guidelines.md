# Publishing guidelines

Use this guide when moving a template from active development to public submission.

## Scope

- **Core `azd` readiness** is about whether the repository is a working Azure Developer CLI (`azd`) template.
- **Awesome AZD publication readiness** is about whether the repository and its metadata are ready for inclusion in the public collection.
- **`microsoft/template-validation-action`** is collection automation that can help validate submission readiness; it is **not** a generic `azd` requirement.

For the shared meaning of **MUST**, **SHOULD**, and **MAY**, start with [docs/readme.md](docs/readme.md).

## Lifecycle

### 1. Build an `azd`-compatible repository

Before you think about collection submission, make sure the repository satisfies the core template requirements:

- `azure.yaml` exists in the repository root or documented working directory.
- Infrastructure assets appropriate to the template are committed.
- The documented `azd up` path works for the intended scenario.

Use these companion guides while building:

- [Definition of done](docs/development-guidelines/definition-of-done.md)
- [Development process](docs/development-guidelines/development-process.md)
- [Operational guidelines](docs/development-guidelines/operational-guidelines.md)
- [Structure samples](docs/structure-samples/structure-samples.md)
- [Template README](docs/template-readme.md)

### 2. Reach repository readiness

Before submission, complete the measurable checks in the [definition of done](docs/development-guidelines/definition-of-done.md). In practice, that means:

- the repository explains how to use the template,
- required OSS and governance files are present,
- `azd` deployment has been validated,
- maintainers can point reviewers to evidence such as PRs, workflow runs, screenshots, or logs.

### 3. Validate for collection submission

For Awesome AZD submissions, you have two common paths:

1. **Validate in your own repository** using [microsoft/template-validation-action](https://github.com/microsoft/template-validation-action/blob/main/README.md).
2. **Use the Awesome AZD automated submission flow**, which validates the repository as part of the contribution process.

When you use `microsoft/template-validation-action`, describe it as **Awesome AZD collection automation** in your repository docs and workflows. It checks collection-oriented requirements such as repository files and gallery-facing conventions in addition to deployability.

### 4. Submit to Awesome AZD

The current public contribution flow is documented in the Awesome AZD contributor guide:

- [Contributor guide](https://azure.github.io/awesome-azd/docs/contribute/)
- [Gallery repository overview](https://github.com/Azure/awesome-azd/blob/main/GALLERY.md)

You can submit in either of these ways:

#### Option A: Open a pull request directly

Follow the contributor guide and add the required metadata to the Awesome AZD repository, including:

- template title and short description,
- source repository link,
- author attribution,
- image asset for the gallery card,
- tags for technologies, language, framework, Azure services, and IaC choice,
- a unique template UUID.

#### Option B: Use the automated template submission issue

The contributor guide also documents an automated submission path:

1. Open the Awesome AZD template submission issue.
2. Let the automation validate the target repository.
3. Review the generated pull request if one is created.
4. Work with collection maintainers until the PR is ready to merge.

## Repository expectations for public submission

For a smooth review, template repositories **SHOULD** be easy for external reviewers to access and assess:

- host the template in a public GitHub repository,
- keep the repository description and topics current,
- keep screenshots, architecture diagrams, and README guidance aligned with the latest deployable scenario.

## After publication

Published templates remain the responsibility of their maintainers.

Maintain the repository by:

- merging changes through pull requests into a protected default branch,
- keeping validation and tests green,
- updating docs when the scenario, prerequisites, or Azure services change,
- responding to collection feedback when a submission or follow-up review surfaces issues.

Awesome AZD review and maintenance practices can evolve over time. Avoid hard-coding turnaround promises or unpublishing timelines in template documentation unless the target collection publishes them explicitly.
