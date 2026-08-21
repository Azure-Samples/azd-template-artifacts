# JavaScript and TypeScript sample guidance

These guidelines describe a modern default for JavaScript and TypeScript templates in this repository. They are **recommendations, not one mandatory architecture**. Pick the structure, framework, and tools that suit the sample, then make the choice explicit in the repo.

- [Purpose and scope](#purpose-and-scope)
- [Adaptable example layout](#adaptable-example-layout)
- [Runtime support policy](#runtime-support-policy)
- [Dependencies](#dependencies)
- [Code quality](#code-quality)
- [Testing](#testing)
- [Azure identity and configuration](#azure-identity-and-configuration)
- [Observability](#observability)
- [Framework and platform notes](#framework-and-platform-notes)
- [Authoritative references](#authoritative-references)

## Purpose and scope

Use this guidance for Node.js-based applications, APIs, CLIs, web apps, monorepos, serverless functions, and libraries.

The goals are to:

- keep runtime support aligned across local and hosted environments
- encourage maintainable package management and test practices
- prefer TypeScript where complexity warrants it
- avoid over-prescribing frameworks or architectural patterns

## Adaptable example layout

The right structure depends on whether the sample is a web app, API, Azure Function, library, or monorepo. A common starting point for application samples is:

```text
~root
├── .devcontainer/
│   └── devcontainer.json
├── .github/
│   └── workflows/
├── infra/
├── src/
│   └── ...
├── tests/
├── public/
├── docs/
├── package.json
├── README.md
└── azure.yaml
```

Other valid patterns include:

- `app/` or framework-native source folders
- `packages/` in a monorepo
- colocated tests for libraries or frontend components
- minimal layouts for simple scripts or Functions samples

Keep the structure easy to navigate and consistent with the sample's tooling.

## Runtime support policy

- Target a **supported Node.js LTS release** and document the support policy by linking to Node.js release guidance instead of hardcoding a version list here.
- Align the chosen Node.js runtime across:
  - deployment configuration
  - CI workflows
  - devcontainer configuration
  - README and contributor docs
  - local tooling metadata such as `engines` when used
- Use language and platform features compatible with the runtime the sample claims to support.
- If the sample uses TypeScript, keep compiler settings and emitted module format consistent with the runtime and packaging model.

## Dependencies

- `npm`, `pnpm`, and Yarn are all valid choices. Pick one package manager per repo, declare `packageManager` in `package.json`, and commit the matching lockfile.
- Prefer TypeScript for non-trivial applications and libraries; use strict mode by default unless the sample has a documented reason not to.
- Keep dependencies purposeful, but do not impose blanket bans on frameworks or third-party libraries.
- Use the Node.js package system intentionally:
  - configure module format explicitly where relevant
  - align `main`, `exports`, or framework entrypoints with the chosen build/runtime model
  - document required build steps if sources are compiled before deployment

## Code quality

Every sample should define and document commands for linting, formatting, and tests.

Recommended approaches include:

- ESLint with `typescript-eslint` for TypeScript-aware linting
- Prettier for formatting
- or Biome where it fits the sample

Additional guidance:

- Keep code quality rules tool-neutral in this guidance; the repo should document the exact chosen stack.
- Run code quality checks in CI.
- Prefer TypeScript compiler strictness for non-trivial TypeScript codebases.

## Testing

- Use an appropriate automated test runner such as:
  - `node:test`
  - Vitest
  - Jest
  - framework-native tooling when justified
- Run tests in CI for the environments the sample claims to support.
- Include coverage reporting, but do not impose a universal 100% threshold.
- Use end-to-end testing when it materially improves confidence:
  - Playwright is a strong option for browser-based E2E coverage
  - it is a recommendation, not a blanket requirement
- Add accessibility, integration, API, or load tests when they are useful for the sample, not because every template must include them.

## Azure identity and configuration

- Prefer **Azure Identity** and **managed identity** for Azure-hosted applications and services.
- Use layered, non-secret configuration:
  - committed defaults for non-sensitive settings
  - local environment variables for developer setup where appropriate
  - platform configuration, Key Vault, or App Configuration for deployed environments
- Do not commit secrets.
- Ensure CI includes dependency scanning and secret scanning where supported.

## Observability

- Prefer structured logging for server applications and services.
- For server-side workloads, prefer **OpenTelemetry** with **Azure Monitor** or another documented telemetry backend.
- Capture enough logs, traces, and metrics to support troubleshooting of requests, dependencies, and failures.
- Keep the local developer experience lightweight while still showing the intended production observability pattern.

## Framework and platform notes

These are examples, not universal mandates:

- **TypeScript**: preferred for non-trivial apps; keep `tsconfig` aligned with the module/runtime strategy.
- **Frontend frameworks**: React, Vue, Angular, Next.js, Svelte, and others are all valid when appropriate for the scenario.
- **APIs**: REST, GraphQL, RPC, or event-driven patterns may all be valid depending on the sample's purpose.
- **Azure Functions (Node.js)**:
  - new Node.js Functions samples should use **programming model v4**
  - use `@azure/functions` 4.x dependencies
  - register functions through code and ensure `package.json` entrypoints match the app structure
  - follow the official Azure Functions Node.js reference for current setup and deployment details

## Authoritative references

- [Node.js release schedule and support policy](https://nodejs.org/en/about/previous-releases)
- [Node.js packages documentation](https://nodejs.org/api/packages.html)
- [TypeScript guidance for choosing compiler options](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options.html)
- [Azure Functions Node.js developer reference](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-node)
