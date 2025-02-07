## C# and .NET project folder structure

The following guidelines and the folders included, represent the conventional structure for a C#/.NET standard application.

```bash
~root
│
├── .devcontainer
│    ├── devcontainer.json
│    └── post-create-command.sh
│      
├── .github/
│   └── workflows/
│       ├── workflow1.yml
│       ├── workflow2.yml
│       └── ...                     
│
├── infra/
│   ├── main.bicep
│   ├── main.parameters.bicep
│   ├── abbreviations.json
│   └── ...                         
│
├── src/
│   ├── ProjectName.Core/
│   │   ├── Models/
│   │   ├── Services/
│   │   ├── Repositories/
│   │   └── ...
│   ├── ProjectName.Web (Blazor)/
│   │   ├── Components/
|   |   |──── <ComponentName>
|   |   |──── Layout
|   |   |──── Pages
│   │   ├── Services/
│   │   ├── wwwroot/
│   │   └── ...
│   ├── ProjectName.Tests/
│   │   ├── Unit/
│   │   ├── Integration/
│   │   └── ...
│   └── ProjectName.sln
│
├── *docs/
│   └── ...
│
├── tools/
│
├── scripts/
│
├── .gitignore
├── azure.yaml
└── README.md

```
* optional additional docs folder for extended documentation files

# Recommended coding styleguide

C# AZD templates follow the [common C# code conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions) used for documentation & samples.
