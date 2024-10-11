## Definition of done (DoD)

The "Definition of Done" in a software project is like the finish line for each task or user story. It's a checklist that ensures everything related to a piece of work is completed to an acceptable standard defined by the acceptance criteria before considering it truly finished.

In our case, we want template applications showcased in the gallery to guarantee developers can 

- Discover
- Setup
- Adapt
- Extend
- Deploy and publish

a functional application.

## Acceptance criteria for gallery acceptance

The following checklist must be complete before a template is published

# Repository Management

- [ ] Standards compliant [README.md](../../README.md) as the one in the example, is in place. The validator will check that at a minimum, the following sections are in place: `Important Security Notice`, `Features`, `Getting Started`, `Resources`, `Guidance`, as `h2` subtitles for sections in your [README.md](../../README.md).
- [ ] [License](../../LICENSE.md) is in place. Make sure you choose the [correct license](https://www.microsoft.com/en-us/legal/intellectualproperty/open-source).
- [ ] [Security guidelines](../../SECURITY.md) are in place.
- [ ] [Contribution guidelines](../../CONTRIBUTING.md) are in place.
- [ ] [Code of conduct](.github/CODE_OF_CONDUCT.md) is in place.
- [ ] [Issue template](.github/ISSUE_TEMPLATE.md) or, when multiple templates are provides, they're located in a folder called `ISSUE_TEMPLATE`.
- [ ] Language, model, and relevant technology topic labels are added, including `azd-templates` and `ai-azd-templates`.
- [ ] Repo description is in place, describing the use case and technologies used in the solution.

## Source code structure and conventions

- [ ] GitHub Actions (This refers to .github/workflows/azure-dev.yml or custom workflow to run on a GitHub runner) is in place
- [ ] DevContainer (/.devcontainer folder where applicable) configuration is in place
- [ ] Infrastructure as code is in place (`/infra` folder where applicable, manifest files or code generators in the case of `Aspire` and similar )
- [ ] Azure services configuration (/azure.yml file) is in place
- [ ] end-to-end testing with Playwright is in place, as specified in [these guidelines](./e2e-testing.md)

## Functional requirements

- [ ] `azd up` successfully provisions and deploys a functional app
- [ ] GitHub Actions run tasks without errors
- [ ] DevContainer has been tested locally and runs
- [ ] Codespaces run [locally and in browser]
- [ ] All tests pass

In the absense of e2e tests, we kindly ask you to make sure that

- [ ] The application has been manually tested to work as per the requirement

## Security requirements

- [ ] Microsoft Managed Identity is used for the following services

- Azure AI
- Azure APIM
- Azure App Service
- Azure Container Apps
- Azure Event Grid
- Azure Kubernetes Service
- Azure Search
- Azure SQL Server

considered our Hero Services. 

When a service selected doesn't support Managed Identity, the corresponding issue must have been reported and the security considerations section in the readme, should clearly explain the alternatives.

- Azure Key Vault is a preferred alternative

> [!IMPORTANT]
> Please note that this list may be regularly updated

#### Code quality and integrity

- [ ] Project code follows standard structure, [per language](../structure-samples/structure-samples.md)
- [ ] Code follows recommended styleguide
