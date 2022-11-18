---
title: Deploy Azure app from Azure container registry
description: Deploy Docker containers to Azure App Service from Visual Studio Code part 1, introduction, and prerequisites.
ms.topic: how-to
ms.date: 09/02/2022
ms.custom: devx-track-js, vscode-azure-extension-update-completed 
# Verified full run: diberry 09/02/2022
---

# 1. Introduction and prerequisites

In this tutorial, you use Visual Studio Code to:

* Create a containerized Node.js application using Docker.
* Push the container image to an Azure Container registry.
* Deploy the image to Azure App Service.

## Prerequisites

- An [Azure subscription](https://azure.microsoft.com/free/).
- [Azure CLI](/cli/azure/install-azure-cli-windows?tabs=azure-cli) or [Azure Cloud Shell in a browser](https://portal.azure.com/#cloudshell/)
- [Visual Studio Code](https://code.visualstudio.com/).
- Visual Studio Code extensions
    - [Azure Account extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)
    - [Azure App Service extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
    - [Azure Resources extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureresourcegroups)
    - [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).
- [Node.js LTS](https://nodejs.org/en/download).
- [Docker](https://www.docker.com/community-edition) - running on your workstation.

## Sign in to Azure

[!INCLUDE [azure-sign-in](../../includes/azure-sign-in.md)]

## Verify Docker install

Verify that you have Docker installed properly by running the following command in a terminal or command prompt:

```bash
docker --version
```

The output should include the version number for docker.

## Next steps

* [Create Azure Container registry](tutorial-vscode-docker-node-02.md)
