# Next steps for an AI starter template

> [!IMPORTANT]
> This document is intended as a practical follow-up checklist after scaffolding or cloning an AI-focused `azd` starter template. Adapt the steps to your architecture, language, and target collection.

## 1. Confirm prerequisites

Before you begin, make sure you have:

- the [Azure Developer CLI (`azd`)](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/overview)
- the language and package tooling your app needs
- access to the Azure subscription and tenant you plan to use
- any service-specific prerequisites called out in the repository `README.md`

If you are starting from an existing template, review the repository `README.md` and `azure.yaml` before provisioning anything.

## 2. Scaffold or clone the template

To start from a published `azd` template in an empty directory:

```shell
azd init --template <template-name>
```

To convert or continue work in an existing repository, use the current `azd` guidance:

- [Azure Developer CLI templates](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/azd-templates)
- [Create Azure Developer CLI templates overview](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/make-azd-compatible)

## 3. Review the template contract

Before provisioning, verify the repository structure and configuration:

- `azure.yaml` maps the right services and infrastructure
- `infra/` contains the authoritative IaC entry point
- hooks are present only where they are needed
- environment-specific settings are parameterized

Use these local guidance pages:

- [Template configuration](../development-guidelines/template-configuration.md)
- [Recommended practices per development domain](../development-guidelines/recommended-practices-per-domain.md)

## 4. Choose the right AI deployment path

If the template deploys models through Microsoft Foundry or Azure OpenAI, decide early which deployment type you intend to support by default:

- **Global Standard** for the common default
- **Data Zone Standard** for US/EU/APAC processing-boundary requirements
- **Standard** for single-region requirements
- **Provisioned** for predictable high-volume traffic
- **Batch** for asynchronous bulk processing

See [Model deployment type guidance for Microsoft Foundry](../development-guidelines/global-deployment.md).

## 5. Provision the baseline infrastructure

Sign in and provision the baseline environment:

```shell
azd auth login
azd provision
```

At this point you should confirm:

- the intended subscription and tenant were used
- the target region or data zone is correct
- the required model, version, and deployment type are supported
- quota and capacity are sufficient for the scenario

If the template provisions both infrastructure and app code together, use `azd up` instead.

## 6. Build the application and complete initialization

After the core Azure resources exist:

- implement or integrate the application code
- confirm any `postprovision` data initialization is idempotent
- configure Microsoft Entra ID and managed identity where supported
- ensure the app uses secure configuration and avoids checked-in secrets

Prefer Azure SDK authentication paths that work with `TokenCredential`. `DefaultAzureCredential` is a common local-to-cloud default when the SDK path supports it.

## 7. Test locally before full deployment

Before publishing changes, validate the template end to end:

- local unit and integration tests
- hook execution on the supported operating systems
- `azd provision`
- `azd deploy` or `azd up`
- application smoke test
- `azd down` if teardown is part of the expected lifecycle

If something fails, use [Troubleshooting guidance](../development-guidelines/trouble-shooting.md).

## 8. Add optional contributor assets intentionally

Generic `azd` templates do not always need every optional asset.

Add a checked-in pipeline definition when:

- your collection requires CI/CD assets, or
- the template experience is materially better with a ready-to-run pipeline

Add a `.devcontainer/` only when it meaningfully improves onboarding or repeatability for contributors.

If you include CI/CD, align it with current [`azd pipeline config`](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/configure-devops-pipeline) behavior and prefer OIDC or federated authentication where supported.

## 9. Harden security and observability

Before sharing or publishing the template, review:

- least-privilege RBAC assignments
- managed identity usage
- remaining secrets and whether they belong in Key Vault
- layered security controls such as IaC scanning, code scanning, dependency review, and secret scanning
- application logs, metrics, and traces

If a target collection has additional requirements, apply those collection-specific requirements at this stage.

## 10. Validate for publication or handoff

Before you publish, hand off, or propose the template for a collection:

- review the repository `README.md`
- confirm the template still works from a clean environment
- verify documentation links and command examples
- check cost-sensitive defaults
- confirm teardown guidance is clear

If your repository or target collection includes a validation workflow, review its results and address any failures before submission.

## Related guidance

- [Definition of done](../development-guidelines/definition-of-done.md)
- [Operational guidelines](../development-guidelines/operational-guidelines.md)
- [Azure Developer CLI templates](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/azd-templates)
- [Create Azure Developer CLI templates overview](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/make-azd-compatible)
