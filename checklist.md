# This is the requirement checklist for an `azd` (Azure Developer CLI) template to be considered complete

# Repository Management
 
- [ ] **README** - Standards compliant [README.md](../../README.md) as the one in the example, is in place
- [ ] **Licensing** - [License](../../LICENSE.md) is in place. Make sure you choose the [correct license](https://www.microsoft.com/en-us/legal/intellectualproperty/open-source).
- [ ] **Security** - [Security guidelines](../../SECURITY.md)are in place
- [ ] **Contribution guidelines** - [Contribution guidelines](../../CONTRIBUTING.md) are in place
- [ ] **Code of conduct** - [Code of conduct](.github/CODE_OF_CONDUCT.md) is in place
- [ ] **Issue Template** - [Issue template](.github/ISSUE_TEMPLATE.md) is in place
- [ ] **Labels** - Language, model and relevant technology topic labels are added, including `azd-templates` and `ai-azd-templates` (The latter is being created)
- [ ] **Description** - Repo description is in place, describing the use case and technologies used in the solution
 
## Source code structure and conventions
- [ ] **azure.yaml** - `azure.yaml` file is in place and services are configured for azd
    - [ ] **AZD Telemetry** - `azure.yaml` should include metadata for telemetry including version number. See [azure.yaml](https://github.com/Azure-Samples/todo-python-mongo-aca/blob/05a7aa59c0253628e293ca0fcc98f35a942df1cc/azure.yaml#L5)
    - [ ] **Service Source** - In `azure.yaml`, the `project` property for each service should point at a subfolder, not at root (`.`). Typically the subfolder is labeled `src` but that may vary. See [azure.yaml](https://github.com/Azure-Samples/todo-python-mongo-aca/blob/05a7aa59c0253628e293ca0fcc98f35a942df1cc/azure.yaml#L8)
- [ ] **Infrastructure as code** is in place (`/infra` folder where applicable, manifest files or code generators in the case of `Aspire` and similar )
    - [ ] **Bicep Format** - Bicep files should be formatted with `bicep format`. Can possibly be automated with [pre-commit](https://github.com/Azure4DevOps/check-azure-bicep) or [GitHub actions](https://github.com/pamelafox/django-quiz-app/pull/15/files#diff-8af3e80c405f1ab691b04ee13deecae46a34d6e87cc81b9a5f21490bc17e2609R29).
    - [ ] **Core Modules** - `main.bicep` should reference modules from `core`, copied from [azure-dev](https://github.com/Azure/azure-dev/tree/main/templates/common/infra/bicep).
     ```cp -r ../azure-dev/templates/common/infra/bicep/core/* infra/core/.```
- [ ] **Workflow** - `.workflow` file is in place (This refers to GitHub Actions `.github/workflows/azure-dev.yml` or custom workflow to run on a GitHub runner)
- [ ] **Devcontainer** - `.devcontainer` should contain a `devcontainer.json` and `Dockerfile`/`docker-compose.yaml` in order to create a full local dev environment. Start with [azure-dev versions](https://github.com/Azure/azure-dev/tree/cb28058af1e7139be4381532f6b1167d9cd948fb/templates/common/.devcontainer) and modify as needed. See [docker-compose.yaml](https://github.com/Azure-Samples/azure-django-postgres-flexible-appservice/blob/main/.devcontainer/docker-compose_dev.yml) for example that includes a local database service.
- [ ] **Hook Scripts** - all hook scripts (pre-/post- provisioning and deployment scripts) shall include both sh and pwsh versions.
 
## Functional requirements
 
- [ ] **azd up** - successfully provisions and deploys a functional app with `azd up`
- [ ] **azd Pipeline Config** - `.github/workflows` should include [`azure-dev.yaml`](https://github.com/Azure-Samples/todo-python-mongo-aca/blob/main/.github/workflows/azure-dev.yml) to support `azd pipeline config`
- [ ] **GitHub Actions** run tasks without errors
- [ ] **DevContainer** has been tested locally and runs
- [ ] **Codespaces** run [locally and in browser]
- [ ] **Dashboard** - Resources should include a dashboard so that `azd monitor` works, either by referencing the `monitoring.bicep` module or creating a dashboard separately. See [main.bicep](https://github.com/Azure-Samples/todo-python-mongo-aca/blob/05a7aa59c0253628e293ca0fcc98f35a942df1cc/infra/main.bicep#L129)
    - [ ] **Monitoring** - Application code should include either OpenCensus or OpenTelemetry so that the monitor is populated. See [todo/app.py](https://github.com/Azure-Samples/todo-python-mongo-aca/blob/05a7aa59c0253628e293ca0fcc98f35a942df1cc/src/api/todo/app.py#L58)
- [ ] All tests pass
    
    In the absense of e2e tests,
    - [ ] The application has been manually tested to work as per the requirement
 
## Security requirements
 
- [ ] **Managed Identity** - Microsoft Managed Service Identity is implemented
- [ ] **security-devops-action** - The application must run microsoft/security-devops-action. [Example](https://github.com/Azure-Samples/azure-search-openai-demo/blob/ab1e4806a176b084b1980a9ee7e1c55bdac1a6d5/.github/workflows/azure-dev-validation.yaml#L25)
    - If unfamiliar with setting up GH Actions, then you can follow the instructions here: [defender-for-cloud/quickstart-onboard-github](https://learn.microsoft.com/en-us/azure/defender-for-cloud/quickstart-onboard-github)
 
When a service selected doesn't support Managed Identity, the corresponding issue must have been reported and the security considerations section in the readme, should clearly explain the alternatives.
 
- Azure Key Vault is a preferred alternative
 
### The following items are not strictly enforced but may prevent the template from being added to the gallery
 
- [ ] Project code follows standard structure, [per language](../structure-samples/structure-samples.md)
- [ ] Code follows recommended styleguide
