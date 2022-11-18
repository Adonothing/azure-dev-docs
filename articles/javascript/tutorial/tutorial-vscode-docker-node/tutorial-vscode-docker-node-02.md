---
title: Use a container registry from Visual Studio Code
description: Part 2, set up a suitable container registry for your app image. 
ms.topic: how-to
ms.date: 09/02/2022
ms.custom: devx-track-js, vscode-azure-extension-update-completed 
# Verified full run: diberry 09/02/2022
---

# 2. Set up Azure Container registry

In this step, you set up a suitable container registry for your app image. Container-capable hosting services like Azure App Service can then pull images from your registry.

This tutorial uses [Azure Container Registry](https://azure.microsoft.com/services/container-registry/), a private, secure, hosted registry for your images. However, the tools and processes shown here also work with other registries like [Docker Hub](https://hub.docker.com/).

## Create an Azure container registry

1. In Visual Studio Code, select **F1** or **CTRL+SHIFT+P** to open the command palette.

1. Enter **registry** in the search box. From the results, select **Azure Container Registry: Create Registry**.

   ![Screenshot that shows searching for the term registry in Visual Studio Code Explorer.](../../media/deploy-containers/docker-create-registry.jpg)

1. Enter or select the following values:

   |Prompt|Value|
   |--|--|
   |**Registry name**|Enter a name that is unique in Azure and contains from 5 to 50 alphanumeric characters.|
   |**SKU**|**Basic**|
   |**Resource group**|Create a new resource group that is unique within your subscription. Create all remaining Azure resources in this resource group.|
   |**Location**|Select a region close to you.|

    Visual Studio Code creates the registry in Azure. After it finishes, you'll see a notification like the following one. This notification confirms the registry was successfully created.

   ![Confirmation in Visual Studio Code that the registry was created](../../media/deploy-containers/registry-created.jpg)

1. If you receive an error that the namespace `Microsoft.ContainerRegistry` is not registered, run the following Azure CLI command to register the namespace: `az provider register --namespace Microsoft.ContainerRegistry`.

1. Open **Docker** Explorer. Ensure that the registry endpoint you just set up is visible under **Registries**.

   ![Verification that the registry appears in Docker Explorer](../../media/deploy-containers/docker-explorer-registry.jpg)

## Sign in to Azure Container Registry

While you can see your Azure registries in the Docker extension, you can't push images to them until you sign in to Container Registry.

1. In Visual Studio, open the Integrated Terminal (<kbd>Ctrl</kbd> + <kbd>`</kbd>) to open the integrated terminal.

1. Run the following Azure CLI command to sign in to Azure CLI. 

    ```azurecli
    az login 
    ```

1. Run the following Azure CLI command to sign in to Container Registry. Replace `<your-registry-name>` with the name of the registry you created.

    ```azurecli
    az acr login --name <your-registry-name>
    ```

## Next steps

* [Create and run a local Node.js app from Visual Studio Code](tutorial-vscode-docker-node-03.md)
 
