# Creating a standards compliant template and publishing it to one of our collections

This document explains the steps necessary to create, validate and publish a standards compliant template, as well as the template lifecycle and it's multiple stages. Each stage maps to a template status.

## Who can publish to a collection?

Some collections are open to all contributors, while curated collections only accept contributions from internal to Microsoft employees and specific partners. If you want to have a template considered, make sure to read and follow the steps described below, and when you're ready, [fill out this form](https://forms.office.com/r/cy1ACkEGK5). We will review it and suggest a collection to include it in.

## What is a collection?

A collection is an entry point to discover, browse and initialize an [Azure Developer CLI](https://github.com/Azure/azure-dev) template. Examples of collection are

- [Awesome AZD](https://azure.github.io/awesome-azd/)
- [AI Apps Gallery](#)

and any other published list, meeting the criteria above

## Step 1: Creating a new template from scratch

If you are creating a new template from scratch, we recommend you use one of the starter templates to build your application template.

To create a new template with all the necessary content, configuration, and structure to successfully pass validation and qualify for curated collections, an author must use the scaffolding mechanism provided by running this command  

`azd init –t [starter-template-name]` 

Starter templates available today are 

Starter template name | URL | Use-case
-- | -- | --
ai-starter | [Azure-Samples/azd-ai-starter: Creates an Azure AI Service and deploys the specified models. (github.com)](https://github.com/azure-samples/azd-ai-starter) | AI without AI-Studio
ai-studio-starter | [Azure-Samples/azd-aistudio-starter: Creates an Azure AI Studio hub, project and required dependent resources including Azure Open AI Service, Cognitive Search and more. (github.com)](https://github.com/Azure-Samples/azd-aistudio-starter)  | AI with AI-Studio

### Initializing the template on your existing sample

You can also initialize the template for an existing code base. To do so, follow [this link](https://learn.microsoft.com/azure/developer/azure-developer-cli/start-with-app-code)

## Step 2: Build your application

This stage maps to template status: `Template in progress`: the template has been generated, published to a repo, and the application is being developed. 

While in progress, the template team can adopt the following development guidelines:

Guideline type | Link
-- | --
Development Guidelines | [azd-template-artifacts/docs/development-guidelines/development-process.md at main · Azure-Samples/azd-template-artifacts (github.com)](https://github.com/Azure-Samples/azd-template-artifacts/blob/main/docs/development-guidelines/development-process.md) 
Operational Guidelines | [azd-template-artifacts/docs/development-guidelines/operational-guidelines.md at main · Azure-Samples/azd-template-artifacts (github.com)](https://github.com/Azure-Samples/azd-template-artifacts/blob/main/docs/development-guidelines/operational-guidelines.md) 
Recommended practices per domain | [azd-template-artifacts/docs/development-guidelines/recommended-practices-per-domain.md at main · Azure-Samples/azd-template-artifacts (github.com)](https://github.com/Azure-Samples/azd-template-artifacts/blob/main/docs/development-guidelines/best-practices-per-domain.md) 
Structure recommendation Samples | [azd-template-artifacts/docs/structure-samples/structure-samples.md at main · Azure-Samples/azd-template-artifacts (github.com)](https://github.com/Azure-Samples/azd-template-artifacts/blob/main/docs/structure-samples/structure-samples.md)


## Step 3: Validate the template

This stage maps to template status: `Template ready`: The template has reached feature complete and has met the acceptance criteria defined here: [Definition of Done](https://github.com/Azure-Samples/azd-template-artifacts/blob/main/docs/development-guidelines/definition-of-done.md)
This template is labeled ready for publication to the collection. 

Please follow the links in the [Definition of Done](https://github.com/Azure-Samples/azd-template-artifacts/blob/main/docs/development-guidelines/definition-of-done.md) to complete it. 

Teams can manually run the validation process by adding the validation action [microsoft/template-validation-action](https://github.com/microsoft/template-validation-action) to their repository and following the guidelines in the reference, to make sure they pass validation before requesting inclusion in the collection. 

If you meet the success criteria, you're ready to publish. Let us know by [filling out this form](https://forms.office.com/r/cy1ACkEGK5). We will review it and suggest a collection to include it in.

## Step4: Get the results of the validation action and act accordingly

Whether you're running the validation workflow manually in your repository, or it has been run automatically by requesting the collection authors to open a pull-request against the collection list to include it (please read more about this process in the [publishing the template](#publishing-the-template) section, below), you can find the [Auto] issue, with the results of the validation scan.

This action will result in two different template statuses:

### Status: template is valid

Check your repository issues for validation confirmation. You will find an issue with the title `[Auto] AI Gallery Standard Validation.`

If the template passes the validation criteria it will be marked as `PASS` or `CONFORMING` and all the items in the checklist will be checked. If the status is `Template valid`, your template will be manually added to the collection, after review from our collection curators.

If you don’t see your template in the gallery in a reasonable time, reach out to the AI Standardization and AI Gallery team. (@Natalia Venditto , @Kristen Womack, @Grace Kulin ) 

If your template is still under active development, read the [Templates failing after publication](#templates-failing-after-publication) section below. 


### Status: template is invalid

If the checks are unsuccessful, the template is invalid and cannot be published. It will be marked as `INVALID` or `NON-CONFORMING`.

There are different levels of validity, and some checks are still under development. 

Please check the [table below](#levels-of-non-conformity-and-expected-action) to understand the levels of non-conformity. 

All items with errors and warnings will display error/warning information. Please follow the “How to fix” links for actionable steps to fix the issues and validate the template for publication. 

### Templates failing after publication

The validation pipeline will be run regularly on all templates. 

Templates failing to pass the validation workflow after published, are subject to be immediately unpublished, depending on the level of the error.  

Please see the recommendation to guarantee template health and integrity, in the [Active development best practices](#active-development-best-practices) section.

### Active development best practices

We suggest that teams actively working on templates that are already published use a `develop` branch for new integration of features, and have a requirement to have a mandatory validation workflow triggered on push to `main` or default branch, using [microsoft/template-validation-action](https://github.com/microsoft/template-validation-action)

### Levels of non-conformity and expected action

Erroring Item | Level | Action
-- | -- | --
Readme.md errors | Moderate | Author receives a notification and unpublishing occurs within 7 days
Missing standard OSS files | Moderate | Author receives a notification and unpublishing occurs within 7 days
`azd up` fails | High | Author receives a notification and unpublishing occurs within 24 hours
`azd down` fails | Moderate | Author receives a notification and unpublishing occurs within 7 days
Missing .devcontainer config | Moderate | Author receives a notification and unpublishing occurs within 7 days
Missing pipeling configuration (azure-dev.yml) | Low | Author receives a notification. No unpublishing occurs
MI is enabled for all services | Moderate | Author receives a notification and unpublishing occurs within 7 days


## Give us feedback!

If you would like to give us feedback on the scaffolding, publishing or validation processes, or have documentation improvement suggestions, reach out! 

🚀 Happy coding!
