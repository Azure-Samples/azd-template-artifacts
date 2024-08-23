# Troubleshooting validation errors

The following document is a collection of typical errors detected by the validation pipeline and how to fix them.

> [!IMPORTANT]
> This is a living document. Please check often for updates.

## Functional requirement: deployment and provisioning error on `azd up`

### Error: UnmatchedPrincipalType: The PrincipalId '{id}' has type 'ServicePrincipal' , which is different from specified PrinciaplType 'User'.

#### Steps to fix this error:

A viable solution is to create a flag to avoid assigning the principal type to service, and create it for the current user. It can be seen in this example:

https://github.com/Azure-Samples/azure-openai-assistant-javascript/pull/18/files

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

## Security requirement: missing workflow

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

### Steps to fix this error

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
