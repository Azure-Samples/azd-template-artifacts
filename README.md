This repo contains all of the artifacts required to build an azd template. An azd template allows a developer to have a fully working application or a application building block that is configured with infrastructure as code (IaC) to provision Azure resources, package the app code, and deploy to Azure with one command, `azd up`.

In this repo you will find:
- A sample README.md file (this file, see below)
- [Recommended practices per domain](/workspaces/azd-template-artifacts/docs/development-guidelines/best-practices-per-domain.md)
- [The definition of done for a template](docs/development-guidelines/definition-of-done.md)
- [Development process](/workspaces/azd-template-artifacts/docs/development-guidelines/development-process.md)
- Guidance on updating to use [Azure OpenAI global deployments](docs/development-guidelines/global-deployment.md)
- [Operational guidelines](docs/development-guidelines/operational-guidelines.md)
- A guide for [troubleshooting validation errors](docs/development-guidelines/operational-guidelines.md)
- A guide for [using the AI-starter template](/workspaces/azd-template-artifacts/docs/next-steps/next-steps-ai-starter.md)
- [Samples by language of expected repo structure](docs/structure-samples)
- [Contributing guidelines](CONTRIBUTING.md)
- [Publishing guidelines](publishing-guidelines.md)
- [Security template and guidelines](SECURITY.md)

Working with these artifacts gives you a starting point for building an azd template or an azd-compatable repo. When using with these artifacts, delete the instructional text before publishing your template or using for your own code base.

# Before you start

> [!IMPORTANT]
> Please make sure to read the [publishing guidelines](./publishing-guidelines.md), to learn more about additional setup steps, standardization, conventions and validation process, to successfully publish a template to one of our collections.

# This is the template README for what is expected in submitted templates

> [!IMPORTANT]
> This is a standard readme file defining the required structure for template validation. Update as required, including replacing all instances of [Project Name] with your project's name, and remove this notice, and the one above.

--- 


# [Project Name]

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](placeholder)
[![Open in Dev Containers](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](placeholder)

(Longer Description, as compared to the GitHub "about" section of the project)
(make sure to highlight the use case!)
Sample application code is included in this project. You can use or modify this app code or you can rip it out and include your own.

[Features](#features) • [Gettting Started](#getting-started) • [Guidance](#guidance)

(include a screenshot of your template's endpoint here-- so users know what it should look like when they're done)

## Important Security Notice (Template Owners, do not remove!)

This template, the application code and configuration it contains, has been built to showcase Microsoft Azure specific services and tools. We strongly advise our customers not to make this code part of their production environments without implementing or enabling additional security features.  


<!-- Documentation page is a WIP, this link does not exist yet -->
For a more comprehensive list of best practices and security recommendations for Intelligent Applications, [visit our official documentation](#link)” 

## Features

This project framework provides the following features:

* Feature 1
* Feature 2
* ...

### Architecture Diagram

Include a diagram describing the application. You can take [this image](https://raw.githubusercontent.com/Azure-Samples/serverless-chat-langchainjs/main/docs/images/architecture.drawio.png) as a reference.

### Demo Video (optional)

(Embed demo video here)

## Getting Started

You have a few options for getting started with this template. The quickest way to get started is [GitHub Codespaces](#github-codespaces), since it will setup all the tools for you, but you can also [set it up locally](#local-environment). You can also use a [VS Code dev container](#vs-code-dev-containers)

This template uses [MODEL 1] and [MODEL 2] which may not be available in all Azure regions. Check for [up-to-date region availability](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability) and select a region during deployment accordingly

  * We recommend using [SUGGESTED REGION]

### GitHub Codespaces

You can run this template virtually by using GitHub Codespaces. The button will open a web-based VS Code instance in your browser:

1. Open the template (this may take several minutes)
    [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](placeholder)
2. Open a terminal window
3. Sign into your Azure account:

    ```shell
     azd auth login --use-device-code
    ```

4. [any other steps needed for your template]
5. Provision the Azure resources and deploy your code:

    ```shell
    azd up
    ```

6. (Add steps to start up the sample app)

### VS Code Dev Containers

A related option is VS Code Dev Containers, which will open the project in your local VS Code using the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers):

1. Start Docker Desktop (install it if not already installed)
2. Open the project:
    [![Open in Dev Containers](https://img.shields.io/static/v1?style=for-the-badge&label=Dev%20Containers&message=Open&color=blue&logo=visualstudiocode)](placeholder)
3. In the VS Code window that opens, once the project files show up (this may take several minutes), open a terminal window.
4. Sign into your Azure account:

    ```shell
     azd auth login
    ```

5. [any other steps needed for your template]
6. Provision the Azure resources and deploy your code:

    ```shell
    azd up
    ```

7. (Add steps to start up the sample app)

8. Configure a CI/CD pipeline:

    ```shell
    azd pipeline config
    ```

### Local Environment

#### Prerequisites

(ideally very short, if any)

* Install [azd](https://aka.ms/install-azd)
  * Windows: `winget install microsoft.azd`
  * Linux: `curl -fsSL https://aka.ms/install-azd.sh | bash`
  * MacOS: `brew tap azure/azd && brew install azd`
* OS
* Library version
* This template uses [MODEL 1] and [MODEL 2] which may not be available in all Azure regions. Check for [up-to-date region availability](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability) and select a region during deployment accordingly
  * We recommend using [SUGGESTED REGION]
* ...

#### Installation

(ideally very short)

* list of any prerequisites
* ...

#### Quickstart

(Add steps to get up and running quickly)

1. Bring down the template code:

    ```shell
    azd init --template [name-of-repo]
    ```

    This will perform a git clone

2. Sign into your Azure account:

    ```shell
     azd auth login
    ```

3. [Packages or anything else that needs to be installed]

    ```shell
    npm install ...
    ```

4. ...
5. Provision and deploy the project to Azure:

    ```shell
    azd up
    ```

6. (Add steps to start up the sample app)

7. Configure a CI/CD pipeline:

    ```shell
    azd pipeline config
    ```

#### Local Development

Describe how to run and develop the app locally

## Guidance

### Region Availability

This template uses [MODEL 1] and [MODEL 2] which may not be available in all Azure regions. Check for [up-to-date region availability](https://learn.microsoft.com/azure/ai-services/openai/concepts/models#standard-deployment-model-availability) and select a region during deployment accordingly
  * We recommend using [SUGGESTED REGION]

### Costs

You can estimate the cost of this project's architecture with [Azure's pricing calculator](https://azure.microsoft.com/pricing/calculator/)

* [Azure Product] - [plan type] [link to pricing for product](https://azure.microsoft.com/pricing/)

### Security

> [!NOTE]
> When implementing this template please specify whether the template uses Managed Identity or Key Vault

This template has either [Managed Identity](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview) or Key Vault built in to eliminate the need for developers to manage these credentials. Applications can use managed identities to obtain Microsoft Entra tokens without having to manage any credentials. Additionally, we have added a [GitHub Action tool](https://github.com/microsoft/security-devops-action) that scans the infrastructure-as-code files and generates a report containing any detected issues. To ensure best practices in your repo we recommend anyone creating solutions based on our templates ensure that the [Github secret scanning](https://docs.github.com/code-security/secret-scanning/about-secret-scanning) setting is enabled in your repos.

## Resources

(Any additional resources or related projects)

* Link to supporting information
* Link to similar sample
* [Develop Python apps that use Azure AI services](https://learn.microsoft.com/azure/developer/python/azure-ai-for-python-developers)
* ...
