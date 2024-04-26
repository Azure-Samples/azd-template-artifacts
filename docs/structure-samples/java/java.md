## Java project folder structure

The following guidelines and the folders included, represent the conventional structure for a Java standard application.

```bash

~root
│
├── src/                            # Source code files
│   ├── main/                       # Main application code
│   │   ├── java/                   # Java source files
│   │   │   ├── com/                # Package structure
│   │   │   │   ├── projectname/    # Project package
│   │   │   │   │   ├── controllers/ # Controllers (for MVC projects)
│   │   │   │   │   ├── services/    # Service classes
│   │   │   │   │   ├── models/      # Data models
│   │   │   │   │   └── ...
│   │   │   │   └── ...
│   │   │   └── ...
│   │   └── resources/              # Resource files (configurations, templates, etc.)
│   │
│   └── test/                       # Unit and integration tests
│
├── docs/                           # Documentation files
│
├── build/                          # Build output directory
│
├── lib/                            # Third-party libraries and dependencies
│
├── scripts/                        # Scripts for build, deployment, etc.
│
└── pom.xml                         # Maven project object model file

```

# Recommended coding styleguide

TBD