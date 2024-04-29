## Python project folder structure

The following guidelines and the folders included, represent the conventional structure for a Python standard application.

```bash

~root
│
├── .devcontainer.json              
├── azure.yaml                      
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
├── *docs/                           
│   └── ...                         
│
├── requirements.txt                
├── README.md                       
└── .gitignore                      
             

```
* optional additional docs folder for extended documentation files

# Recommended coding styleguide

TBD