## C# and .NET project folder structure

The following guidelines and the folders included, represent the conventional structure for a C#/.NET standard application.

```bash
~root
│
├── .devcontainer.json          # Moved to top
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
│   ├── ProjectName.Web/
│   │   ├── Controllers/
│   │   ├── Views/
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

TBD