## Definition of done (DoD)

The "Definition of Done" in a software project is like the finish line for each task or user story. It's a checklist that ensures everything related to a piece of work is completed to an acceptable standard defined by the acceptance criteria before considering it truly finished.

In our case, we want template applications showcased in the gallery to guarantee developers can 

- Discover
- Setup
- Adapt
- Extend
- Deploy and publish

a functional application.

## Acceptance criteria for gallery acceptance

> [WARNING]
> This is a draft

The following checklist must be complete before a template is published

- [] Standards compliant [readme.md](../../README.md) as the one in the example, is in place
- [] .workflow file is in place
- [] GitHub Action runs without errors
- [] .devcontainer configuration is in place
- [] DevContainer has been tested locally and runs
- [] Codespaces run
- [] Infrastructure as code is in place
- [] azure.yml file is in place
- [] `azd up` successfully provisions and deploys a functional app
- [] Minimum coverage tests are in place
- [] All tests pass
- [] Microsoft Managed Service Identity is implemented

In the absense of e2e tests, 

- [] The application has been manually tested to work as per the requirement

### The following items are not strictly enforced but may prevent the template from being added to the gallery

- [] Project code follows standard structure
- [] Code follows styleguide