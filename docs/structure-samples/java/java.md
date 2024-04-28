## Java project folder structure

The following guidelines and the folders included, represent the conventional structure for a Java standard application.

```bash

~root
│
├── .devcontainer.json
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
├── docs/                           
│
├── build/                          
│
├── lib/                            
│
└── scripts/                        


```

# Recommended coding styleguide

TBD