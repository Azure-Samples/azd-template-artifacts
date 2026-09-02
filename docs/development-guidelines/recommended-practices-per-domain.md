# Recommended practices per development domain

These cross-cutting guidelines help keep `azd` templates portable, secure, maintainable, and easier to validate over time.

> [!IMPORTANT]
> In this document, **MUST** is reserved for platform or schema requirements and for clearly scoped collection requirements. **SHOULD** indicates recommended practices for most templates.

## General template design

- Application configuration **SHOULD** come from environment variables, `azd` environment values, or platform configuration rather than checked-in machine-specific files.
- A template **SHOULD** keep application code, infrastructure code, and operational scripts easy to locate and reason about. Common layouts include:
  - `src/` for deployable code
  - `infra/` for Bicep or Terraform
  - `hooks/` or service-local hook folders for lifecycle scripts
- Generic `azd` templates **SHOULD** remain as portable and optional-dependency-light as possible. Pipelines, dev containers, and other contributor conveniences can be added when they provide value, but they are not universal requirements for every template.

## Template configuration and portability

- `azure.yaml` **MUST** be present at the repository root for an `azd` template and **MUST** conform to the current [`azd` schema](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/azd-schema).
- Template authors **SHOULD** keep `azure.yaml` focused on deployable services, infrastructure configuration, hooks, and version requirements rather than embedding large amounts of environment-specific logic.
- Root hooks in `azure.yaml` **SHOULD** be used for repository-level actions, such as preflight validation, provisioning-time data initialization, or post-deployment checks.
- Service hooks **SHOULD** be used only for work that is specific to one service, such as building assets, packaging, or running service-local validation.
- Hooks **SHOULD** declare `shell` explicitly when using inline commands. The current `azd` extensibility guidance notes that the default shell varies by operating system; setting `shell: sh` or `shell: pwsh` explicitly improves portability.
- Hook scripts **SHOULD** be idempotent and safe to rerun in local development and CI.
- Containerized services **SHOULD** be tested from Windows and Linux contributor paths when the template claims cross-platform support. If local Docker behavior differs by OS or architecture, document the supported path and the reason.

## Identity, authentication, and secrets

- Templates **SHOULD** use **Microsoft Entra ID** and **managed identity** for Azure-to-Azure authentication whenever the target service supports it.
- Application code **SHOULD** prefer Azure SDKs and libraries that accept `TokenCredential` so the same code path can work locally and in Azure.
- `DefaultAzureCredential` **SHOULD** be treated as a common local-to-cloud default, not a universal requirement. It is a strong default when the SDK and runtime support it, but some tools, languages, or service integrations may require a different credential strategy.
- RBAC assignments **SHOULD** follow least-privilege scope and role selection. Assign only the roles required for the app, deployment identity, and operational tooling.
- Key-based authentication **SHOULD** be minimized. When secrets still remain, they **SHOULD** be stored in Azure Key Vault or an equivalent secure secret store rather than in code, checked-in files, or long-lived CI secrets.
- GitHub Actions-based deployments **SHOULD** use OIDC/federated credentials instead of client secrets whenever the platform flow supports it.

## Infrastructure as code (IaC)

- Infrastructure **SHOULD** be declared in Bicep or Terraform under a clear `infra/` structure.
- Bicep-based templates **SHOULD** prefer [Azure Verified Modules (AVM)](https://aka.ms/avm) when they fit the scenario and improve maintainability, while still keeping the composition understandable for template consumers.
- Documentation and examples **SHOULD** use current conceptual patterns instead of pinning brittle module versions unless a collection or scenario specifically requires a known fixed version.
- Parameters **SHOULD** expose the environment-specific choices that developers commonly need to change, such as regions, SKUs, model deployment types, or feature flags.
- Provisioning flows **SHOULD** be repeatable. Resource names, tags, and parameter defaults should make it clear which environment owns which deployment.

## CI/CD and supply chain security

- Templates that include CI/CD **SHOULD** align with current [`azd pipeline config`](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/configure-devops-pipeline) behavior instead of relying on older bespoke setup instructions.
- Generic templates **SHOULD NOT** require a checked-in pipeline definition unless the target collection or publishing flow requires one.
- Generic templates **SHOULD NOT** require a checked-in `.devcontainer` unless the template intentionally optimizes for that contributor experience or a collection requires it.
- CI/CD workflows **SHOULD** use minimal permissions and grant write scopes only where they are required.
- GitHub workflows **SHOULD** layer security controls appropriate to the repository, such as:
  - Microsoft Security DevOps and/or IaC scanning
  - CodeQL or equivalent code scanning
  - dependency review
  - secret scanning
- Workflow examples and checked-in pipeline definitions **SHOULD** use currently maintained action versions rather than outdated major versions when a version reference is needed.
- Workflows **SHOULD** avoid risky `pull_request_target` patterns for untrusted pull requests unless the repository has a narrowly reviewed reason and compensating controls.

## Testing and validation

- Template authors **SHOULD** validate the full happy path regularly:
  - `azd init` or template acquisition
  - `azd provision`
  - `azd deploy` or `azd up`
  - app smoke test
  - `azd down` when teardown is part of the support story
- Hooks, scripts, and packaging logic **SHOULD** be tested on the operating systems the template claims to support.
- Applications **SHOULD** include the level of automated testing appropriate for the sample: unit tests where logic exists, integration tests where services interact, and smoke tests for deployment validation.
- Validation steps **SHOULD** be non-interactive in CI wherever possible.

## Observability and operational readiness

- Deployed applications **SHOULD** emit structured logs that are useful in both local development and Azure-hosted environments.
- Templates **SHOULD** integrate with Azure Monitor and Application Insights when those services meaningfully improve diagnosability for the scenario.
- Health probes, startup validation, and actionable error messages **SHOULD** be included for long-running services.
- Operational guidance **SHOULD** explain where to look first when provisioning, deployment, identity, or runtime issues occur.

## Data initialization and schema migration

- Data initialization **SHOULD** be explicit, repeatable, and safe to rerun.
- When initial data or schema setup depends on provisioned Azure resources, a `postprovision` hook is often the right place to trigger the operation.
- Initialization logic **SHOULD** be idempotent. A second run should confirm the desired state rather than duplicating data or corrupting the environment.
- For applications that require database migrations, the migration path **SHOULD** be automated and documented rather than left as a manual tribal step.

## Security and compliance

- Security guidance **SHOULD** be layered rather than relying on a single control.
- Templates **SHOULD** document any unavoidable exceptions, such as a service that does not support managed identity or a tool that still requires a secret.
- Network exposure, data access, and privileged operations **SHOULD** be reduced to the minimum required for the scenario.
- Contributors **SHOULD** review both infrastructure security findings and application-layer findings before publishing or promoting a template.

## Cost management

- Default SKUs, dataset sizes, and deployment choices **SHOULD** keep first-run cost reasonable for a developer or evaluator.
- Documentation **SHOULD** call out the main cost drivers, such as provisioned capacity, premium plans, large databases, persistent AI endpoints, or background compute.
- If a less expensive default is available for local experimentation, the template **SHOULD** use it and document the tradeoffs.
- Teardown guidance **SHOULD** be easy to find, including when `azd down` is sufficient and when additional cleanup may still be needed.

## Collection-specific requirements

Some collections add stricter publication requirements than generic `azd` compatibility. When a collection requires items such as CI/CD definitions, dev container assets, additional policy scanning, or specific documentation sections, those collection-specific items **MUST** be followed in addition to the general guidance above.
