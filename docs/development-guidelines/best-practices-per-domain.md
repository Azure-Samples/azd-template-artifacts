# Best practices per development domain

The following guidelines as a set of best-practices per domain, must be followed to ensure template functionality.

> [!IMPORTANT]
> This is a living guideline. Please check often for updates.

## General guidelines

- Set and read application configuration or settings using `environment variables` and not presets stored in files

## Security

- Use [Managed Identity](https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview) keyless integration for all Azure Services that support it.
- Instantiate credentials in code using `DefaultAzureCredential`.

## Data ingestion on initialization

If you need to restore a database for your application to work, follow these instructions:

### C# applications

- Implement a mechanism for the application to test if data in available and or restore it on startup

### Python and JavaScript

- Use a postprovision hook with the [Azure Developer CLI](https://learn.microsoft.com/azure/developer/azure-developer-cli/azd-extensibility) to run a script

