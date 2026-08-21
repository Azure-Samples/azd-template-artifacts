# azd template artifacts

Reference docs and copyable assets for authoring Azure Developer CLI (`azd`) templates and preparing them for collection submission.

This repository is **not** a deployable template. It is a guidance and standards package for template authors and maintainers.

## Start here

- [Guidance entry point](docs/readme.md) — navigation plus the shared meaning of **MUST**, **SHOULD**, and **MAY**
- [Template README](docs/template-readme.md) — copyable `README.md` authors can adapt for their own template repositories
- [Definition of done](docs/development-guidelines/definition-of-done.md) — measurable completion checklist
- [Development process](docs/development-guidelines/development-process.md) — default GitHub flow for building and reviewing template changes
- [Operational guidelines](docs/development-guidelines/operational-guidelines.md) — ownership, maintenance, and governance guidance
- [Recommended practices](docs/development-guidelines/recommended-practices-per-domain.md) — cross-cutting security, portability, testing, and operations guidance
- [Template configuration](docs/development-guidelines/template-configuration.md) — `azure.yaml`, hooks, services, pipelines, and dev containers
- [Troubleshooting](docs/development-guidelines/trouble-shooting.md) — durable diagnostics for `azd`, Azure, identity, and CI/CD failures
- [Structure samples](docs/structure-samples/structure-samples.md) — example repository layouts by language
- [Publishing guidelines](publishing-guidelines.md) — how to validate and submit to the public Awesome AZD collection

## Requirement layers

- **Core `azd` requirements**: an `azure.yaml` file plus the infrastructure and application assets needed for the template scenario.
- **Awesome AZD collection requirements**: documentation, metadata, and submission steps needed to list a template in the public collection.
- **Optional repository enhancements**: dev containers, GitHub Codespaces, CI/CD pipelines, and other maintainability improvements.

Use the normative language defined in [docs/readme.md](docs/readme.md) when reading the guidance in this repository.
