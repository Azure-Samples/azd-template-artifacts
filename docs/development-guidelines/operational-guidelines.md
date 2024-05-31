# Operational guidelines

The following guidelines are meant to help teams and contributors organize themselves and establish roles within template authoring flows to ensure consistency, quality, and collaboration.

## Template owner(s)

The template owner is the person who owns the template and acts as a point of contact to receive notification when applicable. Typically this role is filled by a core maintainer, who may or may not be code owner, but probably has some administrative persmissions in the overall repository.

This code owner may the one organizing the work and reporting progress. 

## Project (technical lead)

A project technical lead may be the template owner, or a designated team member representing the technology, tool or service to high-light with the purpose of providing technical direction, coherence, and quality across the project. 

A technical lead, depending on their seniority and competence, may be the ultimate decision maker and coordinator of all technical efforts, although when templates integrate multiple tools and service, we recommend to scope per technology and have experts on each category assuming this role.

## Case scenarios and specification

Before the team starts coding, make sure to have designed scenarios, use-cases and specification, answering this questions

- What services, tools and products are we showcasing?
- What are our technical requirements?
- What is the use case/functional specification? 
    * Who will interact with your application?
    * How does the user interact with the application?
    * What does the application take as input?
    * What is the application output?
- What are the non-functional specifications, apart from the perfomance, security and usability success criteria defined by the standardization effort?

## Standardization guidelines

When creating the repository using the automated tool by cloning the [TBD repository], a [Success criteria checklist or definition of done](./definition-of-done.md) issue will be open in your repo. 

After locally working on your application, and before publishing your template to the gallery, automatically or on demand, an automated worflow will run in your repo to validate the standardization success criteria is met.

The system will open related issues when found non-conforming or non-standard blockers in your repository. Once addressed, you can run the validation tool manually, to request reconsideration.

## Version control

Version control ensures code integrity and taceability. Our open-source teams typically collaborate using repositories hosted on Azure-Samples or other organizations.

We advise teams to losely use [GitHub](https://docs.github.com/en/get-started/using-github/github-flow) branching flows and development cycles, always branching out from default branch (default is `main`) and only integrating with a pull-request, with at least 1 reviewer. We strongly recommend you secure your default branch with as a [protected branch](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)

- Main Branch: The main branch for stable, production-ready code.
- Development Branch: A dev branch for integrating features before merging into main.
- Feature Branches: Create individual branches for new features or bug fixes (e.g., feature/login).

### Repository access

Before you start working, we recommend you create a team with the corresponding `read/write` permissions. Make sure all members of the team are added to this list. Make sure there are multiple maintainers.

We recommend the repositories to be public unless the product or service is still private, too, and subject to non-disclosure. We recommend to always work on open-source ogs and not have repositories under specific users. 

### Repository administration

Particularly in cross organization situations, and when collaborators and contributors are in different time-zones, that there are multiple contributors that can elevate permissions to control settings. We recommend they are added to an additional `core contributors` list. 

### Development practices

- *Commit Practices*:
    - *Atomic commits*: Make small, atomic commits with clear messages.
    - *Commit messages*: When possible, follow [Conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) for clarity. When this is not possible, follow the convention: type(scope): description (e.g., [identity]]: add identity logic).

- *Pull Requests (PRs)*:
    - *Code reviews*: Require at least one peer review for each PR. If there are multiple services or products involved, or specific language experts, request their review.
    - *Automated testing and validation*: Integrate automated end-to-end testing workflows to run on PR submissions. Make sure the automated validation pipeline [coming up] is in place.

- *Documentation*:
    - Make sure all the standadrization designated files like readme.md, licence.md, contributing.md, etc, are in place and follow the conventions.

## Code freeze

A team must agree to a roadmap and development cycle, including a code freeze. A sample is considered done, when the [Success Criteria](./definition-of-done.md) is met and the validation is passed.


By following these operational guidelines, your cross-functional/cross org. v-team can work cohesively, ensuring high-quality application samples that are consistent, reliable, and aligned with our best practices for high quality samples.