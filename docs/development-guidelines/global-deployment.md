# Global Deployment

This document outlines the migration guidance for transitioning to a global deployment SKU for GPT models in Azure OpenAI, as well as troubleshooting steps for handling resource deletion conflicts.

## Migration Guidance

To migrate to a global deployment SKU, follow these steps:

1. Add a parameter for each model used in the infrastructure to set the sku name, like chatGptDeploymentSkuName. Use that parameter as the sku input for the OpenAI deployments.

    In main.bicep:

    ```
        param chatGptDeploymentSkuName string = ''
        ...
        module openAi 'br/public:avm/res/cognitive-services/account:0.5.4' = {
            ...
            params: {
                ...
                deployments: [
                    {
                        ...
                        sku: {
                            name: chatGptDeploymentSkuName
                            ...
                        }
                    }
                ]
            }
        }
    ```
   
1. Add a parameter mapping to main.parameters.json or main.bicepparam that maps an azd environment variable to that parameter. This allows developers to customize the sku per azd environment easily. In the parameter mapping, set the default to GlobalStandard.

    In main.parameters.json:    
    ```
        "chatGptDeploymentSkuName": {
            "value": "${CHAT_GPT_DEPLOYMENT_SKU_NAME=GlobalStandard}"
        }
    ````

    In main.bicepparam
    ```
        param chatGptDeploymentSkuName = readEnvironmentVariable('CHAT_GPT_DEPLOYMENT_SKU_NAME', 'GlobalStandard'))
    ```

1. Update the allowed location list in main.bicep because global standard SKU is not supported in some region for some models. See the [global-standard-model-availability](https://learn.microsoft.com/azure/ai-services/openai/concepts/models?tabs=python-secure#global-standard-model-availability) for more details.

    ```
    @description('Location for the OpenAI resource group')
    @allowed([
    'eastus'
    'eastus2'
    'northcentralus'
    'southcentralus'
    'swedencentral'
    'westus'
    'westus3'
    ])
    @metadata({
    azd: {
        type: 'location'
    }
    })
    param openAiResourceGroupLocation string
    ```

1. In the documentation somewhere, provide guidance about how to set the sku back to standard, for developers that need it:
   
    ```shell
        azd env set CHAT_GPT_DEPLOYMENT_SKU_NAME Standard
    ```

1. Test `azd up` and `azd down` to ensure everything works properly. 

## Trouble-Shooting

No issues active. If you experience any issues with global deployments, let us know opening an issue in this repository.
