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

#### Steps to fix this error:

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

### Global deployment endpoints related errors

If you are implementing [Global Deployment Type](https://learn.microsoft.com/azure/ai-services/openai/how-to/deployment-types#global-versus-regional-deployment-types) and you come across `azd down` errors, or other issues, follow [this link](https://github.com/Azure-Samples/azd-template-artifacts/blob/main/docs/development-guidelines/global-deployment.md)

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
