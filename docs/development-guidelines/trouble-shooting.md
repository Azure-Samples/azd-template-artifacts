# Troubleshooting validation errors

## Provisioning and deployment errors

The following section contains a collection of typical errors detected by the validation pipeline when running `azd up` and/or `azd down` and respective recommendations to fix them.

> [!IMPORTANT]
> This is a living document. Please check often for updates.

## Getting log information

When coming across a validation error for those commands, in the issue created by the validation pipeline bot, follow this steps:

- click on the `Details` panel header, to expland it and see the log, as you see in the gif file below.
- find the error details, with more information about the error

### Region availability

Models are available in certain regions, only. If you come across an error referring to Region Availability, [go here](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#region-availability)

Make sure to update your template readme.md file, to reflect the available regions for used models.

### Quotas and limits

Azure AI Services feature quotas and limits. For more information [refer to this document](https://learn.microsoft.com/en-us/azure/ai-services/openai/quotas-limits)

Make sure that your template readme.md file, offers information or links to documentation on pricing, quotas and limits.

## Other errors

### Error: UnmatchedPrincipalType: The PrincipalId '{id}' has type 'ServicePrincipal' , which is different from specified PrinciaplType 'User'.

#### Solution:

A viable solution is to create a flag to avoid assigning the principal type to service, and create it for the current user. It can be seen in this example:

You can see the full example, here: https://github.com/Azure-Samples/azure-openai-assistant-javascript/pull/18/files

In main.bicep

```yaml
@description('Flag to decide where to create OpenAI role for current user')
param createRoleForUser bool = true

// User roles
module openAiRoleUser 'core/security/role.bicep' = if (createRoleForUser) {
  scope: resourceGroup
  name: 'openai-role-user'
  params: {
    // more code

```

### Error BCP332: The provided value (whose length will always be greater than or equal to 15) is too long to assign to a target for which the maximum allowable length is 10

The `maxLength` property defined for a parameter in `main.bicep` is too small for the actual value.

#### Solution:

Increase the `maxLength` value to accommodate longer inputs.

### Principal XXX does not exist in the directory XXX. Check that you have the correct principal ID. If you are creating this principal and then immediately assigning a role, this error might be related to a replication delay. In this case, set the role assignment principalType property to a value, such as ServicePrincipal, User, or Group.  See https://aka.ms/docs-principaltype

The RBAC assignment in `main.bicep` is missing the required `principalType` property. This can result in a replication delay or incorrect assignment.

#### Solution:

Add the `principalType` field, e.g. 'ServicePrincipal', 'User', or 'Group'.

### ERROR: error executing step command 'deploy --all': failed deploying service 'indexer': archive/tar: write too long

This is a known issue affecting both the Azure Developer CLI (azd) and the devcontainers/ci GitHub action.

#### Solution:

Pending on issues:
  - https://github.com/Azure/azure-dev/issues/4803
  - https://github.com/devcontainers/ci/issues/366

## Security requirements

When the security scan is enabled, you may come across the following warnings

### Missing workflow

If either of the following actions are not present in the `.github/workflows` folder

- microsoft/security-devops-action
- github/codeql-action/upload-sarif

the validation pipeline will throw a warning like this

```
Error: microsoft/security-devops-action is missing in .github/workflows/azure-dev.yml.
Error: github/codeql-action/upload-sarif is missing in .github/workflows/azure-dev.yml.
Error: microsoft/security-devops-action is missing in .github/workflows/bicep-audit.yml.
```

indicating the workflow file and the action missing.

#### Steps to fix this error

To fix the error, add the action to the corresponding workflow file, like for example:

```yml
jobs:
  deploy:
    // more properties...
    steps:
      // Other actions

      - name: Run Microsoft Security DevOps Analysis
        uses: microsoft/security-devops-action@v1
        id: msdo
        with:
          tools: templateanalyzer

      - name: Upload results to Security tab
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: ${{ steps.msdo.outputs.sarifFile }}

    // More steps
```

> [!WARNING]
> The recommended actions may change or increase over time

You can see the full example file [here](https://github.com/Azure-Samples/azd-ai-starter/blob/main/.github/workflows/azure-dev.yml)

### Security requirements: Managed Identity

A template's security scan may return several results with a SARIF format, as a result of running the @microsoft/security-devops-action. Solving a concrete error or warning may require the configuration or deployment of additional, with the consequent cost and deployment time increase.

One common error, is using plain-text keys to authenticate requests to a service, typically represented by 

> error: TA-000019 - For enhanced authentication security, use a managed identity. On Azure, managed identities eliminate the need for developers to have to manage credentials by providing an identity for the Azure resource in Azure AD and using it to obtain Azure Active Directory (Azure AD) tokens. 

#### Steps to fix this error

Make sure Entra ID and Default Credentials are enabled for all services in the template.

### Solving additional errors in the scan log

Please identify the error code and head to [PS Rule Reference](https://azure.github.io/PSRule.Rules.Azure/en/rules/) to find a potential solution

Disclaimer: To make sure your deployment is production-grade, you may need to follow additional security guidelines by each service.
