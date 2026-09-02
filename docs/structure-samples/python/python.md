# Python sample guidance

These guidelines describe a modern default for Python-based templates in this repository. They are **recommendations, not a single required architecture**. Choose the structure and tooling that fit the sample's purpose, then document that choice clearly in the repo.

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

Use this guidance for Python application samples, libraries, CLIs, APIs, background workers, and Azure-hosted services.

The goals are to:

- keep runtime expectations clear and supportable
- align local development, CI, containers, and deployment configuration
- encourage modern packaging and test practices
- leave room for framework-specific conventions where appropriate

## Adaptable example layout

Not every sample needs the same folders. The following layout is a good starting point for many application-oriented repositories:

```text
~root
├── .devcontainer/
│   └── devcontainer.json
├── .github/
│   └── workflows/
├── infra/
├── src/
│   └── sample_app/
│       ├── __init__.py
│       └── ...
├── tests/
├── docs/
├── pyproject.toml
├── README.md
└── azure.yaml
```

Recommendations:

- Prefer a `pyproject.toml`-first setup.
- A `src/` layout is recommended for non-trivial packages and applications because it helps catch import-path mistakes and matches common packaging guidance.
- Add or omit folders such as `docs/`, `scripts/`, `samples/`, `migrations/`, or `tests/integration/` based on the sample's needs.
- If the sample is intentionally minimal, a flatter structure can still be valid if the repo remains easy to understand.

## Runtime support policy

- Target **upstream-supported CPython releases** instead of freezing a hardcoded version list in this document.
- Align the chosen Python runtime across:
  - deployment configuration
  - CI workflows
  - devcontainer configuration
  - README and contributor docs
  - local tool configuration when relevant
- Set `requires-python` in `pyproject.toml` so packaging metadata and tooling agree on the supported range.
- Use only the language features available in the runtime versions the sample claims to support.
- Test only the runtime and OS combinations the repository explicitly supports. Broad version or OS matrices are optional, not universal requirements.
- In commands and documentation, prefer `python -m pip ...` over invoking `pip` directly.

Example:

```toml
[project]
requires-python = ">=3.11"
```

Choose the lower bound that matches the oldest Python version the sample actually supports.

## Dependencies

- Prefer declaring application dependencies in `pyproject.toml`, typically using the `project.dependencies` metadata defined by modern packaging standards.
- Use optional dependencies, dependency groups, requirements files, or lock files when they fit the workflow:
  - extras for installable feature sets
  - dependency groups for development or contributor workflows
  - requirements files for deployment, export, or compatibility scenarios
  - lock files when reproducibility is important for the chosen toolchain
- Do **not** require one recursive `requirements-dev.txt` pattern for every sample.
- Keep dependency update tooling and dependency scanning enabled in CI or repository settings where supported.
- Document the install path the sample expects, for example:
  - `python -m pip install -e .`
  - `python -m pip install -e ".[dev]"`
  - `python -m pip install -r requirements.txt`

## Code quality

Every sample should define and document commands for linting, formatting, and tests.

Recommended approach:

- Use a modern Python code quality stack such as:
  - Ruff for linting and optionally formatting
  - or a healthy Flake8/Black/isort-style stack
- Follow standard Python naming and style conventions, including PEP 8 where applicable.
- Keep formatting and linting rules explicit in project configuration rather than relying on undocumented defaults.
- Run code quality checks locally and in CI.
- Prefer concise, maintainable rules over opinionated one-off bans. Avoid universal mandates for arbitrary ternary or string-formatting preferences.

## Testing

- Use `pytest` for most projects, or a framework-native runner when that is the better fit.
- Configure automated test execution in CI.
- Include coverage reporting for the code the sample owns.
- Set a coverage threshold only if the repository defines one; the threshold is **repo policy**, not a universal 100% requirement.
- Test the environments the sample claims to support rather than requiring every Python version or every operating system.
- Add integration, end-to-end, accessibility, property-based, or load tests when they materially improve confidence for that sample.

Examples of valid approaches:

- unit and integration tests with `pytest`
- Django tests with Django's test tooling
- FastAPI tests with `pytest` and HTTP client fixtures
- additional tools such as Hypothesis, Schemathesis, Playwright, Axe, or Locust when the sample's scope justifies them

## Azure identity and configuration

- Prefer **Azure Identity** and **managed identity** for Azure-hosted samples.
- Use layered, non-secret configuration:
  - committed defaults for non-sensitive settings
  - environment variables or local `.env`-style developer configuration where appropriate
  - Azure App Configuration, Key Vault, or platform settings for deployed environments
- Do not commit secrets.
- For frameworks that require an application secret, keep guidance narrowly scoped:
  - generate the secret securely
  - store production secrets outside source control, such as in Key Vault or platform-managed configuration
  - do not ship an insecure production default

## Observability

- Prefer structured logging over ad hoc `print()` statements for long-running services and server applications.
- For server-side applications, prefer **OpenTelemetry** with **Azure Monitor** or another documented telemetry backend.
- Capture enough logs, traces, and metrics to troubleshoot startup, request flow, dependencies, and failures.
- Keep local developer observability simple, but ensure deployed samples show the intended production pattern.
- Enable CI checks for dependency scanning and secret scanning where supported.

## Framework and platform notes

These are examples, not universal mandates:

- **Flask / FastAPI / Django**: document the recommended local run command and any framework-specific settings the sample depends on.
- **Gunicorn or other production servers**: include runtime server configuration only when the hosting model needs it.
- **CLI or script samples**: a smaller structure may be more appropriate than a web-app layout.
- **Azure Functions for Python**: follow the current Functions packaging and runtime guidance for the specific worker model the sample uses.

## Authoritative references

- [Python release status - Python Developer's Guide](https://devguide.python.org/versions/)
- [Writing your `pyproject.toml` - Packaging Python Projects](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/)
- [Dependency Groups - PyPA specifications](https://packaging.python.org/en/latest/specifications/dependency-groups/)
- [pytest good integration practices](https://docs.pytest.org/en/stable/explanation/goodpractices.html)
