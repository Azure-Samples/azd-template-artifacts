## Java project folder structure

The following guidelines and the folders included, represent the conventional structure for a Java standard application.

```bash

~root
│
├── .devcontainer
│    ├── devcontainer.json
│    └── post-create-command.sh
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
├── azure.yaml
├── pom.xml
├── README.md
├── src/                            
│   ├── main/                       
│   │   ├── java/                   
│   │   │   ├── com/                
│   │   │   │   ├── projectname/    
│   │   │   │   │   ├── controllers/ 
│   │   │   │   │   ├── services/    
│   │   │   │   │   ├── models/      
│   │   │   │   │   └── ...
│   │   │   │   └── ...
│   │   │   └── ...
│   │   └── resources/              
│   │
│   └── test/                       
│
├── *docs/
|   └── ...                          
│
├── build/                          
│
├── lib/                            
│
└── scripts/                        


```
* optional additional docs folder for extended documentation files

# Recommended coding styleguide

TBD