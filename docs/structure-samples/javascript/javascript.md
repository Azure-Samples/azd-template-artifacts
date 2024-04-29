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
├── * docs/                           
│   └── ...                         
│
├── package.json                    
├── README.md                       

```
* optional additional docs folder for extended documentation files

## Additional recommendations

- TypeScript is preferred over vanilla JavaScript

- 3rd party dependencies will be used to a minimum and Web Platform APIs are preferred

- Except in the cases when we're showcasing a specific framework, vanilla over frameworks is preferred to reduce maintenance efforts

- Server side JavaScript will be written for the latest Node.js LTS version

- When writing serverless APIs to be deployed to Azure Functions, they should use the model v4 when possible/supported

- REST is preferred over GraphQL to avoid Apollo conflict resolution across packages

- Preferred package manager and monorepo tool is npm 

- Prettier and ESLint configuration should be in place 


# Recommended code style guidelines 

The following styles and conventions are not strictly mandatory but highly recommended. Large deltas from this recommendations may block a template from being added to the gallery.

[TS Style Guide](https://ts.dev/style/#identifiers)

[Guidelines for writing JavaScript code examples - The MDN Web Docs project | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/MDN/Writing_guidelines/Writing_style_guide/Code_style_guide/JavaScript)
 