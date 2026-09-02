# Template configuration guidelines

This page describes the current cross-cutting configuration guidance for `azd` templates, with emphasis on `azure.yaml`, hooks, portability, and optional contributor assets.

> [!IMPORTANT]
> `azure.yaml` is the contract between your repository and Azure Developer CLI. Keep it current with the latest [`azd` schema](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/azd-schema) and update it whenever services, infrastructure layout, or lifecycle hooks change.

## Minimum `azd` template configuration

For an `azd`-compatible template:

- `azure.yaml` **MUST** exist at the repository root.
- `azure.yaml` **MUST** remain valid against the current schema.
- The template **SHOULD** define only the services, infrastructure paths, hooks, and version requirements that are needed for the scenario.
- The template **SHOULD** keep environment-specific values in `azd` environment variables, Bicep/Terraform parameters, or platform configuration rather than hardcoding them in scripts.

At a minimum, most templates define:

- `name`
- `metadata.template`
- `infra`
- `services`

Many templates also benefit from:

- `resourceGroup`
- `hooks`
- `requiredVersions`
- `pipeline`
- `workflows`
- `cloud`

## `azure.yaml` responsibilities

The [`azd` schema reference](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/azd-schema) is the source of truth. In practice, `azure.yaml` usually serves four purposes:

1. Describes the deployable application services.
2. Points `azd` at the infrastructure entry point.
3. Registers lifecycle hooks for repository-wide or service-specific automation.
4. Optionally describes CI/CD integration and version requirements.

### Example structure

```yaml
name: my-ai-template
metadata:
  template: my-ai-template@0.1.0
resourceGroup: rg-${AZURE_ENV_NAME}
infra:
  provider: bicep
  path: infra
hooks:
  preprovision:
    posix:
      shell: sh
      run: ./hooks/preprovision.sh
    windows:
      shell: pwsh
      run: ./hooks/preprovision.ps1
  postprovision:
    posix:
      shell: sh
      run: ./hooks/postprovision.sh
    windows:
      shell: pwsh
      run: ./hooks/postprovision.ps1
services:
  api:
    project: ./src/api
    language: js
    host: appservice
    hooks:
      prepackage:
        posix:
          shell: sh
          run: ./hooks/prepackage.sh
        windows:
          shell: pwsh
          run: ./hooks/prepackage.ps1
```

The example above is illustrative. Adjust the `host`, `language`, and hook set to match the actual template.

## Root hooks vs. service hooks

Current `azd` extensibility guidance supports hooks at the root of `azure.yaml` and within individual service definitions.

### Root hooks

Root hooks are best for repository-level or end-to-end actions, such as:

- validating prerequisites before provisioning
- generating shared configuration
- running post-provision data initialization
- performing cross-service smoke tests after deployment

The working directory for a root hook is the project root, where `azure.yaml` lives.

### Service hooks

Service hooks are best for actions that belong to one deployable unit, such as:

- building a frontend bundle
- packaging a service
- verifying service-local dependencies
- running a service-local code generation step

The working directory for a service hook is the service `project` directory.

### Hook design recommendations

- Hooks **SHOULD** be small, deterministic, and easy to rerun.
- Hooks **SHOULD** fail fast when prerequisites are missing.
- Hooks **SHOULD** avoid interactive prompts in CI/CD.
- Repository-level tasks **SHOULD NOT** be duplicated in every service hook.

## Explicit shell selection for portability

The [`azd` extensibility documentation](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/azd-extensibility) notes that hook shell defaults vary by operating system:

- `pwsh` on Windows
- `sh` on Linux and macOS

Because of that behavior:

- Inline hooks **SHOULD** set `shell` explicitly.
- Cross-platform templates **SHOULD** provide separate `posix` and `windows` variants when syntax differs.
- External script files **SHOULD** use the appropriate extension for the intended shell, such as `.sh` or `.ps1`.

If a hook works only in one shell, make that explicit in configuration instead of relying on contributors to infer it.

## Infrastructure configuration

- The `infra` section **SHOULD** point to the authoritative provisioning entry point for the template.
- Bicep templates **SHOULD** keep parameters, modules, and environment-variable mappings organized so that common changes such as region, SKU, and model deployment type do not require editing source files.
- If the template uses layered infrastructure, the `infra.layers` configuration **SHOULD** make dependencies clear, especially when a hook produces values consumed by a later layer.

## Service configuration

- Each entry under `services` **SHOULD** represent a deployable application component.
- Service definitions **SHOULD** use paths relative to the repository root.
- Only services that `azd` needs to build, package, or deploy **SHOULD** be declared.
- If a service is containerized, its configuration **SHOULD** reflect the build and deployment path actually exercised by the template.

## Required versions

Templates **SHOULD** use `requiredVersions` when a minimum `azd` version or extension version is important for correctness. This is especially helpful when the template relies on:

- a newer `azure.yaml` schema capability
- hook behavior that changed in recent releases
- an extension-based provider

Avoid arbitrary version pinning when a minimum supported version communicates the requirement more clearly.

## CI/CD configuration

`azd` supports checked-in pipeline definitions that work with [`azd pipeline config`](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/configure-devops-pipeline).

- GitHub Actions templates typically place `azure-dev.yml` under `.github/workflows/`.
- Azure Pipelines templates typically place `azure-dev.yml` under `.azdo/pipelines/` or `.azuredevops/pipelines/`.

For generic templates:

- Checked-in pipeline definitions are **optional**.
- OIDC or federated authentication **SHOULD** be preferred over client secrets when the platform supports it.
- Workflow permissions **SHOULD** be minimal.

If a target collection requires a pipeline definition for validation or publishing, that collection-specific requirement **MUST** be followed.

## Dev container configuration

A `.devcontainer/` setup can improve onboarding, but for generic `azd` templates it is **optional**.

Use a dev container when it materially improves the contributor experience, for example by preinstalling `azd`, language runtimes, emulators, or platform tooling that is otherwise tedious to configure. If you include one:

- it **SHOULD** reflect the supported toolchain versions
- it **SHOULD** be tested periodically
- it **SHOULD NOT** become the only documented way to use the template unless that is an intentional collection requirement

## Practical review checklist

Before publishing or handing off a template, verify that:

- `azure.yaml` matches the current repository layout
- root and service hooks run from the expected working directories
- hook shells are explicit where portability matters
- service definitions map to the right deployment targets
- optional pipeline and dev container assets are either present intentionally or omitted intentionally
- environment-specific values are parameterized rather than hardcoded
