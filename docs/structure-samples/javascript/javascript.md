## JavaScript project folder structure

The following guidelines and the folders included, represent the conventional structure for a JavaScript or TypeScript standard application, as an npm package.

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
│   ├── app/                        
│   │   ├── components/             
│   │   ├── services/               
│   │   ├── models/                 
│   │   └── ...
│   ├── styles/                     
│   ├── assets/                     
│   ├── utils/                      
│   └── core/
│
├── public/                         
│
├── tests/                          
│   └── ...                         
│
├── docs/                           
│   └── ...                         
│
├── package.json                    
├── README.md                       

```

# Recommended coding styleguide

TBD