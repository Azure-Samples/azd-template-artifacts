# Troubleshooting guidance

This page provides a durable diagnostic workflow for common `azd`, infrastructure, identity, model deployment, and security-validation problems.

> [!IMPORTANT]
> Start by identifying **which stage failed**: local setup, provisioning, deployment, post-provision initialization, runtime application behavior, or CI/CD validation. That distinction usually shortens the investigation more than any individual error-specific tip.

## 1. Capture the failing command and the exact stage

Record:

- the command that failed, such as `azd provision`, `azd deploy`, or `azd up`
- whether the failure occurred locally or in CI/CD
- the current `azd` environment name
- the Azure subscription and target region or data zone
- the service or hook that failed, if applicable

When a failure happens in CI/CD, start with the workflow logs. When it happens locally, start with the terminal output from the failing `azd` command and any hook output that ran immediately before the failure.

## 2. Separate infrastructure failures from application failures

Use this quick triage:

- **Provisioning failed**: investigate IaC, parameters, RBAC, region support, quota, or provider-specific constraints.
- **Deployment failed after provisioning succeeded**: investigate service packaging, build output, containerization, hook behavior, or deployment target configuration.
- **Runtime failed after deployment succeeded**: investigate application configuration, identity, networking, observability, or data initialization.

Do not rebuild the entire environment until you know which category the failure belongs to.

## 3. Validate `azure.yaml`, hooks, and portability assumptions

Many cross-platform failures come from configuration drift rather than Azure itself.

Check that:

- `azure.yaml` still matches the current repository structure
- hook paths are correct relative to the expected working directory
- root hooks assume the repository root as `cwd`
- service hooks assume the service `project` directory as `cwd`
- inline hooks declare `shell` explicitly when shell syntax matters
- Windows and POSIX hook variants exist when the script syntax differs

If a hook only works on one operating system, the failure may appear as a provisioning or deployment problem even though the underlying issue is shell portability.

## 4. Validate model, version, and deployment type choices

For Microsoft Foundry and Azure OpenAI-related scenarios, confirm all of the following before assuming a service outage:

- the **model name** is still valid
- the **model version** is still supported
- the selected **deployment type** matches the scenario
- the chosen **region** or **data zone** supports that combination
- the subscription has the required **quota**, **capacity**, and **scope**

Use the current Microsoft Learn pages rather than older repository notes:

- [Deployment overview for Microsoft Foundry Models](https://learn.microsoft.com/en-us/azure/foundry/concepts/deployments-overview)
- [Understanding deployment types in Microsoft Foundry Models](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types)
- [Foundry Models sold by Azure](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure)
- [Microsoft Foundry Models quotas and limits](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/quotas-limits)
- [Data, privacy, and security for Foundry Models sold by Azure](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)

### Common model-deployment diagnostic pattern

If a deployment works in one environment but not another, compare:

- subscription
- region or data zone
- deployment type
- model version
- available quota scope

That comparison often reveals a real difference in platform support or quota allocation.

## 5. Check identity and RBAC before changing infrastructure

Many deployment failures are actually authorization failures.

Confirm:

- the signed-in user or federated workload identity is targeting the intended subscription and tenant
- required resource providers are registered when the service requires them
- managed identities were created successfully
- role assignments exist at the correct scope
- newly created principals have had time to propagate if the failure occurred immediately after creation

### Common RBAC symptoms

#### Principal does not exist or replication-delay style errors

This often indicates one of the following:

- the wrong principal ID was used
- the principal was created moments earlier and has not propagated yet
- the role assignment omitted the needed `principalType`
- the assignment scope is wrong

#### Principal type mismatch

If the platform reports a principal type mismatch, verify that the assignment matches the actual principal being granted access, such as `User`, `ServicePrincipal`, or `Group`.

## 6. Check configuration and secret handling

If the deployment succeeds but the application fails at runtime, confirm that:

- the application is reading the expected environment variables
- secret names and configuration keys match between app code and Azure configuration
- a managed-identity path is being used where the template expects it
- any remaining secrets were actually populated in the secure store used by the template

If the app uses `DefaultAzureCredential`, verify that the expected credential source is available in the current environment. A successful local login does not guarantee that the cloud-hosted workload identity has equivalent access.

## 7. Review data initialization separately from infrastructure creation

If the app depends on seeded data, migrations, or indexing:

- check whether the initialization step ran at all
- confirm whether it ran as a root hook, service hook, app startup task, or manual step
- verify that the step is idempotent
- rerun only the initialization logic when possible instead of reprovisioning everything

This is especially important for templates that use `postprovision` hooks to prepare databases, indexes, or sample content.

## 8. Troubleshoot CI/CD securely

For CI/CD failures, review both pipeline configuration and security posture.

Check:

- whether the repository is using the intended provider configuration for `azd pipeline config`
- whether GitHub Actions or Azure Pipelines authentication uses the intended federated or client-credential flow
- whether the workflow has the minimum required permissions and no more
- whether required secrets or environment variables were configured
- whether branch protections or environment approvals are blocking deployment

### GitHub Actions security troubleshooting

- Prefer OIDC/federated credentials over long-lived client secrets when supported.
- Avoid introducing `pull_request_target` for untrusted pull request execution unless the repository has a reviewed and narrowly justified need.
- If security tooling flags excessive workflow permissions, reduce them instead of broadening them further to "make the build pass."

## 9. Troubleshoot security findings conceptually

Security findings are often symptoms of a missing control rather than a request to copy a specific snippet.

### Identity-related findings

If a scan reports insecure service authentication:

- first verify whether the target Azure service supports managed identity
- then confirm whether the application and SDK path can use `TokenCredential`
- if a secret is still required, document the exception and store it securely

### Workflow and code scanning findings

If a repository or collection requires layered security scanning, confirm that the expected controls are present, such as:

- Microsoft Security DevOps or equivalent IaC/security analysis
- CodeQL or equivalent code scanning
- dependency review
- secret scanning

Address the underlying concern the finding represents. Do not add broad permissions or unrelated actions without understanding why the check failed.

## 10. Use observability once deployment succeeds

When Azure resources are up and deployment has finished, use application and platform telemetry to continue diagnosis:

- application logs
- deployment logs
- health endpoints
- Azure Monitor and Application Insights data
- resource-specific diagnostic settings where enabled

Provisioning success only proves the control plane worked. It does not prove the application is healthy.

## Escalation checklist

Before escalating an issue, collect:

- the failing command
- the exact error text
- the stage of failure
- the operating system
- the `azd` environment name
- the subscription and region or data zone
- whether the issue reproduces locally, in CI/CD, or both
- whether the problem started after changing model, version, deployment type, RBAC, or hooks

That evidence makes it much easier to distinguish product issues from template regressions.
