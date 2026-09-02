# C#/.NET guidance

## Purpose and scope

Use this page as adaptable guidance for C#/.NET azd templates. It describes common conventions that work well for template repositories, but it does **not** require one fixed architecture or folder taxonomy for every app.

Choose a structure that matches the hosting model, application size, and team workflow. Keep the app, CI, deployment, devcontainer, and documentation aligned on the same supported runtime and toolchain decisions.

## Adaptable example layout

This example shows a common multi-project layout for a .NET repository. It is intentionally illustrative rather than prescriptive.

```text
/
├── .devcontainer/
│   └── ...
├── .github/
│   └── workflows/
│       └── ...
├── infra/
│   └── ...
├── src/
│   ├── MyApp.Api/
│   │   └── ...
│   ├── MyApp.Worker/
│   │   └── ...
│   └── MyApp.Shared/
│       └── ...
├── tests/
│   ├── MyApp.Api.Tests/
│   │   └── ...
│   └── MyApp.IntegrationTests/
│       └── ...
├── MyApp.sln
├── azure.yaml
└── README.md
```

Recommended conventions:

- Keep the solution file at the repository root for discoverability.
- Use a top-level `src/` directory for application projects.
- Use a top-level `tests/` directory for test projects instead of nesting tests under `src/`.
- Do not assume every repo needs layers such as `Core`, `Web`, or `Repositories`; prefer names that reflect the actual deployable apps and shared libraries in the template.

## Runtime support policy

Target a **supported .NET LTS release** that fits the hosting target and enterprise support expectations. Avoid hard-coding an exact runtime version in long-lived guidance.

For templates:

- Check the current [.NET support policy](https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core).
- Keep the selected runtime aligned across:
  - project target frameworks
  - local development prerequisites
  - CI build/test images
  - devcontainer configuration
  - deployment/runtime configuration
  - template documentation
- If a non-LTS release is used for a specific scenario, document why and make the support window explicit.

## Dependency management

Use SDK-style project files and `PackageReference`.

Recommended practices:

- Keep package references in project files unless there is a clear repo-wide reason to centralize them.
- For multi-project repositories, consider **Central Package Management** to simplify version alignment across projects.
- Keep dependency versions consistent across application and test projects.
- Prefer official Microsoft and Azure SDK packages where applicable.

## Formatting and static analysis

Use built-in .NET tooling first, then add repository-specific rules as needed.

Recommended baseline:

- `.editorconfig` for formatting and code-style settings.
- SDK analyzers enabled for the solution.
- `dotnet format` for formatting and analyzer/code-style enforcement in local and CI workflows.
- Additional analyzers may be added when the repo needs stronger architectural or security rules.

## Testing

Use `dotnet test` as the standard test entry point.

Recommended practices:

- Any of **xUnit**, **NUnit**, or **MSTest** can be valid; pick one and use it consistently within a repo.
- Separate unit and integration tests when that improves clarity or CI behavior.
- Collect coverage in CI when the repository policy requires it.
- Treat the **coverage threshold as repository policy**, not as a universal template mandate.
- Follow current .NET unit testing guidance for naming, isolation, and maintainability.

## Azure identity and configuration

Prefer Azure SDK clients that accept `TokenCredential`.

Recommended practices:

- Use **managed identity** for deployed Azure workloads whenever the hosting target supports it.
- `DefaultAzureCredential` is a common default for local-to-cloud flows, but it is not the only valid choice.
- Use a more explicit credential chain when a template needs tighter control, clearer failure behavior, or constrained authentication sources.
- Follow the layered .NET configuration model, such as:
  - appsettings files
  - environment-specific overrides
  - environment variables
  - user secrets for local development when appropriate
  - external configuration providers when the scenario requires them
- Keep secrets out of source control and template defaults.

## Observability and health

Use OpenTelemetry-based instrumentation as the primary observability approach.

Recommended practices:

- Emit structured logs, metrics, and traces.
- Integrate with Azure Monitor when the hosting scenario uses Azure-native monitoring.
- For ASP.NET Core apps, use built-in health checks for readiness/liveness-style endpoints.
- Document any expected telemetry/exporter configuration in the template README and deployment guidance.

## Framework notes

- ASP.NET Core apps commonly combine dependency injection, configuration providers, health checks, and OpenTelemetry with minimal custom infrastructure.
- Worker services, console apps, and Azure Functions may use different project composition patterns than web apps; the repository layout should reflect that difference.
- Keep framework-specific conventions documented in the template rather than implying one mandatory layered architecture.

## Authoritative references

- [.NET support policy](https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core)
- [Common project structure for .NET projects](https://learn.microsoft.com/en-us/dotnet/core/porting/project-structure)
- [Central Package Management](https://learn.microsoft.com/en-us/nuget/consume-packages/central-package-management)
- [Unit testing best practices in .NET](https://learn.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices)
- [Observability with OpenTelemetry for .NET](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-with-otel)