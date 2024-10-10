# Global Deployment

This document outlines the migration guidance for transitioning to a global deployment SKU for GPT models in Azure OpenAI, as well as troubleshooting steps for handling resource deletion conflicts.

## Migration Guidance

To migrate to a global deployment SKU, follow these steps:

1. **Add a Parameter for DeploymentSkuName**: Update your deployment configuration to include a parameter for `DeploymentSkuName`.

2. **Default Sku to GlobalStandard**: Set the default value of the `DeploymentSkuName` parameter to `GlobalStandard` when possible. See the [global-standard-model-availability](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models?tabs=python-secure#global-standard-model-availability) for more details.

3. **Test**: After step number 2, test `azd up` and `azd down` to ensure everything works properly. There is currently a known issue with destroying resources properly in this scenario. If you have this issue, see the [troubleshooting section](#trouble-shooting) for more details on implementing the workaround (pre-down hooks to delete the model if needed. This ensures that the `azd down` process is not blocked and all resources are destroyed as expected.) Adding the pre-down hooks is only recommended until the known issue is resolved.

4. **Test the Functionality**: Run the `azd up` and `azd down` commands to test the functionality and ensure that the migration is successful.

## Trouble-Shooting

### Issue

In certain scenarios, a global-standard type Azure OpenAI deployment may prevent the deletion of the OpenAI account, resulting in a conflict error when running the `azd down` command. Specifically, users may encounter a `409 Conflict` error with the error code `ResourceGroupDeletionBlocked`, leaving the OpenAI account resource undeleted.

### Solution

To address this issue, you can add pre-down hooks to the `azd down` command. These hooks can utilize the Azure CLI or Azure SDK to delete the deployment resources before executing the `azd down` command. This ensures that the `azd down` process is not blocked and all resources are destroyed as expected. Once this bug is resolved, it's recommended to remove this script.

### Implementation Steps

1. **Create Pre-Down Hooks**: Implement pre-down hooks that will be executed prior to the `azd down` command.

    An example of using Python SDK:

    ./scripts/pre-down.sh
    ```sh
        #!/bin/bash

        # Get the directory of the current script
        script_dir=$(dirname "$0")

        # Load environment variables from azd env
        subscription_id=$(azd env get-value AZURE_SUBSCRIPTION_ID)
        resource_name=$(azd env get-value AZURE_OPENAI_RESOURCE_NAME)
        resource_group=$(azd env get-value AZURE_RESOURCE_GROUP)
        deployment_name=$(azd env get-value AZURE_OPENAI_CHATGPT_DEPLOYMENT)

        # Run the Python script with the retrieved values
        python "$script_dir/pre-down.py" --subscription-id $subscription_id --resource-name $resource_name --resource-group $resource_group --deployment-name $deployment_name
    ```

    ./scripts/pre-down.ps1
    ```powershell
        # Get the directory of the current script
        $scriptDir = Split-Path -Parent $MyInvocation.MyCommand.Definition

        # Load environment variables from azd env
        $subscriptionId = azd env get-value AZURE_SUBSCRIPTION_ID
        $resourceName = azd env get-value AZURE_OPENAI_RESOURCE_NAME
        $resourceGroup = azd env get-value AZURE_RESOURCE_GROUP
        $deploymentName = azd env get-value AZURE_OPENAI_CHATGPT_DEPLOYMENT

        # Run the Python script with the retrieved values
        python "$scriptDir/pre-down.py" --subscription-id $subscriptionId --resource-name $resourceName --resource-group $resourceGroup --deployment-name $deploymentName
    ```

    ./scripts/pre-down.py
    ```python
        import argparse

        from azure.identity import DefaultAzureCredential
        from azure.mgmt.cognitiveservices import CognitiveServicesManagementClient

        # Set up argument parsing
        parser = argparse.ArgumentParser(description="Delete an Azure OpenAI deployment.")
        parser.add_argument("--resource-name", required=True, help="The name of the Azure OpenAI resource.")
        parser.add_argument("--resource-group", required=True, help="The name of the Azure resource group.")
        parser.add_argument("--deployment-name", required=True, help="The name of the deployment to delete.")
        parser.add_argument("--subscription-id", required=True, help="The Azure subscription ID.")

        args = parser.parse_args()

        # Authenticate using DefaultAzureCredential
        credential = DefaultAzureCredential()

        # Initialize the Cognitive Services client
        client = CognitiveServicesManagementClient(credential, subscription_id=args.subscription_id)

        # Begin delete the deployment
        poller = client.deployments.begin_delete(
            resource_group_name=args.resource_group, account_name=args.resource_name, deployment_name=args.deployment_name
        )

        # Wait for the delete operation to complete
        poller.result()

        print(f"Deployment {args.deployment_name} deleted successfully.")
    ```

2. **Integrate with `azd down`**: Ensure that these hooks are integrated with the `azd down` command to execute automatically, by adding the hooks in `azure.yaml` file

    ```yaml
    hooks:
        predown:
            windows:
            shell: pwsh
            run: ./scripts/pre-down.ps1
            continueOnError: true
            posix:
            shell: sh
            run: ./scripts/pre-down.sh
            continueOnError: true
    ```

By following these steps, you can ensure a smooth and conflict-free resource deletion process for global-standard type Azure OpenAI deployments.
