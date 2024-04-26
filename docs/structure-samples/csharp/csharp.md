## C# and .NET project folder structure

The following guidelines and the folders included, represent the conventional structure for a C#/.NET standard application.

```bash
ProjectName/
│
├── src/                            # Source code files
│   ├── ProjectName.Core/           # Core project (contains business logic)
│   │   ├── Models/                 # Data models
│   │   ├── Services/               # Business services
│   │   ├── Repositories/           # Data access layer
│   │   └── ...
│   │
│   ├── ProjectName.Web/            # Web project (ASP.NET Core MVC, Web API, or Razor Pages)
│   │   ├── Controllers/            # MVC controllers or Web API controllers
│   │   ├── Views/                  # Views for MVC or Razor Pages
│   │   ├── wwwroot/                # Static files (CSS, JavaScript, etc.)
│   │   └── ...
│   │
│   ├── ProjectName.Tests/          # Unit and integration tests
│   │   ├── Unit/                   # Unit tests
│   │   ├── Integration/            # Integration tests
│   │   └── ...
│   │
│   └── ProjectName.sln             # Visual Studio solution file
│
├── docs/                           # Documentation files (e.g., README.md, API documentation)
│
├── tools/                          # Tools and utilities
│
├── scripts/                        # Scripts for build, deployment, etc.
│
└── .gitignore                      # Git ignore file
```

# Recommended coding styleguide

TBD