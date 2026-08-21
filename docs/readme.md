# Template guidance

Use this directory as the entry point for authoring and maintaining Azure Developer CLI (`azd`) templates.

## Normative language

The documents in this repository use these terms deliberately:

- **MUST** — an active `azd` requirement, an Awesome AZD validator or collection rule, or a platform constraint
- **SHOULD** — an evidence-backed recommendation that improves reviewability, maintainability, or template usability
- **MAY** — an optional enhancement

## Guidance map

| If you need to... | Start here |
| --- | --- |
| Understand the guidance set and requirement layers | [README](../README.md) |
| Copy a repository landing page for your own template | [template-readme.md](template-readme.md) |
| Check whether a template is ready to ship | [development-guidelines/definition-of-done.md](development-guidelines/definition-of-done.md) |
| Plan day-to-day implementation and review flow | [development-guidelines/development-process.md](development-guidelines/development-process.md) |
| Clarify ownership and maintenance expectations | [development-guidelines/operational-guidelines.md](development-guidelines/operational-guidelines.md) |
| Apply cross-cutting security and engineering practices | [development-guidelines/recommended-practices-per-domain.md](development-guidelines/recommended-practices-per-domain.md) |
| Configure `azure.yaml`, hooks, services, and optional contributor assets | [development-guidelines/template-configuration.md](development-guidelines/template-configuration.md) |
| Choose a Microsoft Foundry model deployment type | [development-guidelines/global-deployment.md](development-guidelines/global-deployment.md) |
| Diagnose provisioning, deployment, identity, model, or CI/CD failures | [development-guidelines/trouble-shooting.md](development-guidelines/trouble-shooting.md) |
| Continue from an AI-focused starter template | [next-steps/next-steps-ai-starter.md](next-steps/next-steps-ai-starter.md) |
| Prepare for public submission | [../publishing-guidelines.md](../publishing-guidelines.md) |
| Compare repository layouts by language | [structure-samples/structure-samples.md](structure-samples/structure-samples.md) |

## Requirement layers

### Core `azd` template requirements

These are the baseline requirements for a working template:

- **MUST** include `azure.yaml`.
- **MUST** include infrastructure assets appropriate to the template scenario.
- **MUST** include the code, configuration, and documentation needed for the supported `azd up` path.

### Awesome AZD collection requirements

These apply when you want to list a template in the public Awesome AZD collection:

- **MUST** satisfy the repository and documentation expectations captured in the [definition of done](development-guidelines/definition-of-done.md).
- **MUST** follow the current public submission process in the [publishing guidelines](../publishing-guidelines.md).
- **MAY** use [microsoft/template-validation-action](https://github.com/microsoft/template-validation-action/blob/main/README.md) to automate collection-oriented checks before submission.

### Optional repository enhancements

These are often useful, but they are not universal `azd` requirements:

- **MAY** include a dev container or GitHub Codespaces configuration.
- **MAY** include CI/CD automation such as `azd pipeline config` output.
- **MAY** add extra governance, testing, or contributor-experience assets beyond the baseline OSS files.
