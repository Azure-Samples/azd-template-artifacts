# Template README

Copy this file into a template repository root and replace the `ALL_CAPS` tokens with template-specific content. Remove any optional sections that do not apply.

````md
# PROJECT_NAME

One short paragraph that explains what the template deploys, who it is for, and which Azure scenario it demonstrates.

> Optional: add badges for GitHub Codespaces, dev containers, CI status, or package health only when those features are configured and working.

![Architecture diagram or screenshot](./assets/architecture.png)

## Important Security Notice

This template is intended to help you get started on Azure. Before using it in production, review authentication, authorization, networking, data protection, secrets management, and monitoring for your environment.

**Disclaimer:** To make sure your deployment is production-grade, you may need to follow additional security guidelines provided by each service.

If the template uses Microsoft Entra ID, managed identities, Key Vault, or Microsoft Foundry resources, describe the exact security model and any operator responsibilities here.

## Features

- Feature or scenario outcome 1
- Feature or scenario outcome 2
- Key Azure services, frameworks, or developer workflows included in the template

## Getting Started

### Prerequisites

- An Azure subscription
- [Azure Developer CLI (`azd`)](https://aka.ms/install-azd)
- Any language, runtime, SDK, or tool prerequisites required by this template
- Any access, quota, or service enablement prerequisites required by this template

### Quickstart

1. Initialize a new project from the template.
2. Sign in:

   ```shell
   azd auth login
   ```

3. Provision and deploy:

   ```shell
   azd up
   ```

4. Verify the deployed experience using the steps or URLs this template produces.
5. Clean up resources when you are done:

   ```shell
   azd down
   ```

### Local Development

Explain how to run, test, and debug the application locally.

### Optional Developer Environment

Include dev container, GitHub Codespaces, or CI/CD setup instructions only if this repository supports them.

## Guidance

### Architecture

Summarize the deployed components, data flow, and integration points.

### Costs

Call out the primary Azure services that affect cost and link to current pricing guidance when helpful.

### Security

Explain the authentication model, secret handling, and any production hardening steps a customer should evaluate.

### Customization

Explain the main places a developer is expected to modify when adapting the template.

## Resources

- Link to architecture or product documentation
- Link to related samples
- Link to troubleshooting or advanced setup docs
````
