## Python project folder structure

The following guidelines and the folders included, represent the conventional structure for a Python standard application.

```bash

~root
│
├── .devcontainer.json              # Added file
├── azure.yaml                      # Added file
│
├── .github/
│   └── workflows/
│       ├── workflow1.yml           # Added file
│       ├── workflow2.yml           # Added file
│       └── ...                     
│
├── infra/
│   ├── main.bicep                  # Added file
│   ├── main.parameters.bicep       # Added file
│   ├── abbreviations.json          # Added file
│   └── ...                         
│
├── src/                            
│   ├── projectname/                
│   │   ├── __init__.py             
│   │   ├── core/                   
│   │   ├── models/                 
│   │   ├── services/               
│   │   └── ...
│   └── scripts/                    
│
├── tests/                          
│   └── ...                         
│
├── docs/                           
│   └── ...                         
│
├── requirements.txt                
├── setup.py                        
├── README.md                       
└── .gitignore                      

```

# Recommended coding styleguide

TBD