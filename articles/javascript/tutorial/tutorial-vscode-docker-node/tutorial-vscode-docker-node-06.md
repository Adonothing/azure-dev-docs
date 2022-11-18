---
title: Redeploy a container to Azure App Service after making changes in Visual Studio Code
description: Docker Tutorial step 6, the simple steps to rebuild and redeploy a container image.
ms.topic: how-to
ms.date: 09/02/2022
ms.custom: devx-track-js, vscode-azure-extension-update-completed 
# Verified full run: diberry 09/02/2022
---

# 6. Make changes and redeploy a container using Visual Studio Code

Because you inevitably make changes to your app, you end up rebuilding and redeploying your container many times. Fortunately, the process is simple:

## Change and deploy your app again

1. Change the Welcome message, in `./public/client.html`, to `Welcome 2 Express`.

1. In Visual Studio Code, open the **Command Palette** (**F1**) and run **Docker Images: Build Image** to rebuild the image. If you change only app code, the build should take only a few seconds.

1. To push the image to your Azure container registry, open the **Command Palette** (**F1**) again and run **Docker Images: Push**, choosing the image you just built. As before, because a change to your app code is small, only that layer needs to be pushed and the process typically completes in a few seconds.

1. In the **Azure** explorer  (<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>A</kbd>), in the **Resources** section, find your App Service resource and right-click to select **Restart**. Restarting an app service automatically pulls the latest container image from the registry.

1. After about 15-20 seconds, visit the App Service URL again to check the updates.

## Next steps

* [Stream logs](tutorial-vscode-docker-node-07.md)