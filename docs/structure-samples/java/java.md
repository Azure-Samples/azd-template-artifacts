# Java guidance

## Purpose and scope

Use this page as adaptable guidance for Java azd templates. It describes common conventions that fit many template repositories, but it does **not** prescribe one mandatory architecture or build layout for every application.

Choose the structure, framework, and build tooling that best match the hosting target and the app type. Keep the chosen Java runtime aligned across local development, CI, devcontainer configuration, deployment, and documentation.

## Adaptable example layout

Java templates commonly use either Maven or Gradle. Support the canonical layout for the selected build tool rather than inventing a custom convention.

### Maven-style example

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
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/myapp/
│   │   │       └── ...
│   │   └── resources/
│   │       └── ...
│   └── test/
│       ├── java/
│       │   └── com/example/myapp/
│       │       └── ...
│       └── resources/
│           └── ...
├── pom.xml
├── azure.yaml
└── README.md
```

### Gradle-style example

```text
/
├── .devcontainer/
│   └── ...
├── .github/
│   └── workflows/
│       └── ...
├── gradle/
│   └── ...
├── infra/
│   └── ...
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/myapp/
│   │   │       └── ...
│   │   └── resources/
│   │       └── ...
│   └── test/
│       ├── java/
│       │   └── com/example/myapp/
│       │       └── ...
│       └── resources/
│           └── ...
├── build.gradle[.kts]
├── settings.gradle[.kts]
├── azure.yaml
└── README.md
```

Recommended conventions:

- Use the standard Maven or Gradle source layout.
- Use a stable root package convention such as `com.example.myapp` or an organization-owned domain-based package name.
- Do **not** treat generated output directories such as `build/` as checked-in structure conventions.
- Do **not** introduce a generic checked-in `lib/` directory unless the template has a specific, documented need for it.

## Runtime support policy

Target a **supported LTS JDK** appropriate to the hosting target and support requirements.

For templates:

- Check the current [Microsoft Build of OpenJDK support guidance](https://learn.microsoft.com/en-us/java/openjdk/support).
- Keep the selected JDK aligned across:
  - build configuration
  - local development prerequisites
  - CI runners
  - devcontainer images
  - deployment/runtime settings
  - template documentation
- If a framework or hosting environment narrows the supported JDK range, document that relationship in the template.

## Dependency management

Use the native dependency management features of the chosen build tool.

Recommended practices:

- For Maven, use `dependencyManagement` for version alignment across modules and shared dependencies.
- For Gradle, use platforms and/or version catalogs for consistent dependency versions.
- Use a BOM or platform when a library ecosystem publishes one.
- When Azure SDK libraries are used together, consider the Azure SDK BOM to keep versions aligned.
- Keep test dependency versions aligned with the production dependency strategy.

## Formatting and static analysis

Choose a formatter and static-analysis stack deliberately, then document it in the repository.

Recommended baseline:

- Use one formatter consistently in local development and CI.
- For Spring-based templates, **Spring Java Format** is a valid option when the repository wants Spring-aligned formatting.
- Add static analysis such as Checkstyle, SpotBugs, PMD, Error Prone, or framework-specific rules when the repo benefits from them.
- Prefer build-integrated enforcement so formatting and analysis run consistently in CI.

## Testing

Use **JUnit Jupiter** on the current JUnit Platform as the standard default unless a template has a clear reason to choose differently.

Recommended practices:

- Keep unit and integration tests organized clearly within the canonical test layout.
- Make test execution a normal part of CI for template validation.
- Collect coverage when the repository policy requires it.
- Do **not** impose an arbitrary universal coverage threshold in this guidance; define thresholds at the repository level if needed.

## Azure identity and configuration

Prefer Azure SDK clients that accept `TokenCredential`.

Recommended practices:

- `DefaultAzureCredential` is a common default for local and deployed scenarios.
- Use managed identity in Azure-hosted environments when supported by the target platform.
- If the app needs tighter control over authentication sources, use a more explicit credential configuration.
- For Spring apps, prefer externalized configuration and environment-specific profiles instead of hard-coded environment behavior.
- Keep secrets out of source control and out of template defaults.

## Observability and health

Use OpenTelemetry-based instrumentation for logs, metrics, and traces where the framework and hosting model support it.

Recommended practices:

- Integrate with Azure Monitor when the deployment model uses Azure-native observability.
- For Spring Boot apps, use **Actuator** for health, metrics, and operational endpoints.
- Document any required telemetry exporters, sampling choices, and environment variables in the template README.

## Framework notes

- Spring Boot templates often use externalized configuration, profiles, Actuator, and framework-driven dependency injection.
- Non-Spring Java apps may use different packaging, configuration, and observability approaches; reflect those differences in the template instead of forcing a Spring-style architecture everywhere.
- Keep root package conventions intentional and stable to support component scanning, packaging, and long-term maintainability.

## Authoritative references

- [Microsoft Build of OpenJDK support](https://learn.microsoft.com/en-us/java/openjdk/support)
- [Maven standard directory layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)
- [Gradle project organization](https://docs.gradle.org/current/userguide/organizing_gradle_projects.html)
- [Azure Identity for Java](https://learn.microsoft.com/en-us/java/api/overview/azure/identity-readme?view=azure-java-stable)
- [Spring Boot Actuator reference](https://docs.spring.io/spring-boot/reference/actuator/index.html)