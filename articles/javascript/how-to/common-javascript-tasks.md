---
title: Top Azure tasks for JavaScript developers
description: Find an example of your current tasks.
ms.topic: how-to
ms.date: 05/24/2022
ms.custom: devx-track-js
---

# Top tasks for JavaScript developers

Find an example of your current task. If you can't find a task, leave feedback requesting your task. 

## Active Directory App 

Provide authentication. 

[AD App registration Documentation](/azure/active-directory/develop/quickstart-register-app)

|Task|using|
|--|--|
|Create app registration|[Portal](/azure/active-directory/develop/tutorial-v2-react)<br>[Azure CLI](/cli/azure/ad/app#az-ad-app-create)|
|Easy authentication|[Static Web Apps](with-web-app/static-web-app-with-swa-cli/add-authentication.md)<br>[Express.js](/azure/app-service/scenario-secure-app-authentication-app-service-as-user)|
|List app registration|[Azure CLI](/cli/azure/ad/app#az-ad-app-list)
|MSAL Login/Logoff button using `@azure/msal-browser`|[React/TypeScript](/azure/active-directory/develop/tutorial-v2-react)|
|MSAL React using `@azure/msal-browser` passing user credentials to Function API|[React and Azure Function API](with-authentication/static-web-app-with-api/add-mongodb-database-to-api.md#react-client-add-new-fetch-method-with-favoritecolor)|
|MSAL Express.js using `@azure/msal-node`|[Express.js](./with-web-app/deploy-msal-sdk-authentication-expressjs.md#run-your-app-locally-to-verify-msal-authentication)|
|Revoke Azure AD permission|[https://myapplications.microsoft.com/](https://myapplications.microsoft.com/)|
|Revoke Consumer permission|[https://account.live.com/consent/manage](https://account.live.com/consent/manage)

## Azure Resource Groups

|Task|using|
|--|--|
|Create resource group|[Azure CLI](../tutorial/static-web-app-image-analysis.md#create-your-resource-group)<br>[TypeScript](with-web-app/azure-function-resource-group-management/add-delete-functions-redeploy.md#add-typescript-code-to-add-and-delete-resource-groups)|
|Delete resource group|[Azure CLI](../tutorial/static-web-app-image-analysis.md)<br>[TypeScript](with-web-app/azure-function-resource-group-management/add-delete-functions-redeploy.md#add-typescript-code-to-add-and-delete-resource-groups)|
|List resource groups|[TypeScript](with-web-app/azure-function-resource-group-management/create-function-app.md#list-all-resource-groups-in-subscription-with-javascript)|

## Apps

### Static Web Apps

[Service documentation](/azure/static-web-apps/)

|Task|using|
|--|--|
|Create React app targeting JavaScript language|[Bash](/azure/static-web-apps/getting-started?tabs=react#create-a-static-web-app)|
|Create Vue app|[Bash](/azure/static-web-apps/getting-started?tabs=vue#create-a-static-web-app)|
|Create Static Web Apps|[Visual Studio Code extension](../tutorial/static-web-app-image-analysis.md#create-a-static-web-app-resource)<br>[Azure CLI](with-web-app/static-web-app-with-swa-cli/create-static-web-app.md?tabs=create-swa-azure-cli)|
|Browse site|[Visual Studio Code extension](../tutorial/static-web-app-image-analysis.md#view-azure-static-web-site-in-browser)|
|Proxy SWA locally with SWA CLI|[SWA CLI](./with-web-app/static-web-app-with-swa-cli/connect-client-to-api.md)|
|Authenticate SWA locally with SWA CLI|[SWA CLI](with-web-app/static-web-app-with-swa-cli/add-authentication.md#test-the-local-authentication-process-provided-by-swa-cli)|
|Set Static Web app local environment variables|[Bash](../tutorial/static-web-app-image-analysis.md#add-environment-variables-to-your-local-environment)|



### Function (serverless) apps

[Service documentation](/azure/azure-functions/)

|Task|using|
|--|--|
|Create Functions app locally|[Visual Studio Code extension](../tutorial/azure-function-cosmos-db-mongo-api.md)|
|HTTP trigger code|[JavaScript](../tutorial/azure-function-cosmos-db-mongo-api.md)|
|Debug/test function locally|[Visual Studio Code](../tutorial/azure-function-cosmos-db-mongo-api.md)|
|Deploy function to Azure cloud|[Visual Studio Code extension](../tutorial/azure-function-cosmos-db-mongo-api.md)|
|Verify secure function API is available |[Visual Studio Code extension/Browser](../tutorial/azure-function-cosmos-db-mongo-api.md)|
|Remove function app resource|[Visual Studio Code extension](../tutorial/azure-function-cosmos-db-mongo-api.md)|

## Cognitive Services

[Service group documentation](/azure/cognitive-services/)

|Task|using|
|--|--|
|Create Cognitive Services _ComputerVision_ resource|[Azure CLI](../tutorial/static-web-app-image-analysis.md#create-azure-resources)|
|Get Cognitive Services _ComputerVision_ resource|[Azure CLI](../tutorial/static-web-app-image-analysis.md#create-azure-resources)|
|Install Azure SDK|[Bash](../tutorial/static-web-app-image-analysis.md#add-computer-vision-to-local-react-app)|
|Analyze image with [`@azure/cognitiveservices-computervision`](https://www.npmjs.com/package/@azure/cognitiveservices-computervision)|[Visual Studio Code](../tutorial/static-web-app-image-analysis.md#add-computer-vision-code-as-separate-module)|

## Containers including Docker tasks

|Task|using|
|--|--|
|Add docker files to local project|[Visual Studio Code extension](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#add-docker-files)|
|Build docker image from local project|[Visual Studio Code extension](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#build-a-docker-image)|
|Create a container image from your local JavaScript project|[Visual Studio Code](./with-visual-studio-code/containerize-local-project.md#create-a-container)|
|Create container registry resource|[Visual Studio Code extension](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-02.md#create-an-azure-container-registry)<br>[Azure CLI](./with-azure-cli/create-container-registry-resource.md#create-a-container-registry)|
|Create Dockerfile|[Visual Studio Code extension](./with-visual-studio-code/containerize-local-project.md#create-a-dockerfile-in-your-project)|
|Deploy image to app service|[Visual Studio Code extension](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-05.md#deploy-image-to-azure-web-app-from-vs-code)|
|Get Azure container registry credentials|[Azure CLI](./with-azure-cli/create-container-registry-resource.md#get-container-registry-credentials)|
|Login to container registry|[BASH - Docker CLI](./with-azure-cli/create-container-registry-resource.md#login-to-container-registry-with-docker-cli)|
|Push image to Docker registry resource|[Visual Studio Code extension](./with-visual-studio-code/containerize-local-project.md#push-local-container-image-to-dockerhub)|
|Push image to Azure container registry resource|[Visual Studio Code extension](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-04.md#push-the-image-to-a-registry)<BR>[BASH - Docker CLI](./with-azure-cli/create-container-registry-resource.md#push-your-local-image-to-your-container-registry)|
|Run local container|[Visual Studio Code extension](with-visual-studio-code/containerize-local-project.md#build-image-from-your-project)|
|Tag your local image|[BASH - Docker CLI](./with-azure-cli/create-container-registry-resource.md#tag-your-local-image)|
|Verify Docker version|[Bash](../tutorial/tutorial-vscode-docker-node/tutorial-vscode-docker-node-01.md#verify-docker-install)|

## Databases

### Azure Cosmos DB for Apache Cassandra

[Service documentation](/azure/cosmos-db/)

|Task|Using|
|--|--|
|Create resource|[Azure portal](https://ms.portal.azure.com/#create/Microsoft.DocumentDB)<br>[Azure CLI](./with-database/use-cassandra-as-cosmos-db.md#create-a-cosmos-db-resource-for-cassandra-db)|
|Create keystore on resource|[Azure CLI](./with-database/use-cassandra-as-cosmos-db.md#create-a-keyspace-on-the-server-with-azure-cli)|
|Create table on keystore|[Azure CLI](./with-database/use-cassandra-as-cosmos-db.md#create-a-table-on-the-keyspace-with-azure-cli)|
|Get connection information|[Azure CLI](./with-database/use-cassandra-as-cosmos-db.md#get-the-cassandra-connection-string-with-azure-cli)|
|Use cassandra-driver API on Azure Cosmos DB|[JavaScript](./with-database/use-cassandra-as-cosmos-db.md#use-cassandra-driver-sdk-to-connect-to-cassandra-db-on-azure)|

### MariaDB

[Service documentation](/azure/mariadb/)

|Task|Using|
|--|--|
|Create MariaDB resource|[Azure portal](https://ms.portal.azure.com/#create/Microsoft.MariaDBServer)<br>[Azure CLI](./with-database/use-mysql-mariadb.md?tabs=MySQL#create-a-mariadb-resource-with-azure-cli)<br>[@azure/arm-mariadb](https://www.npmjs.com/package/@azure/arm-mariadb)|
|Create MariaDB database on resource|[Azure CLI](./with-database/use-mysql-mariadb.md?tabs=MySQL#create-a-mariadb-resource-with-azure-cli)|
|Get Connection string|[Azure CLI](./with-database/use-mysql-mariadb.md?tabs=MySQL#get-the-mariadb-connection-string-with-azure-cli)|
|Use and view database|[Azure Cloud Shell](https://shell.azure.com/)'s _mysql_ CLI<br>[MySQL Workbench](https://www.mysql.com/products/workbench/)<br>[Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools-driver-mysql)<br>[npm mariadb](https://www.npmjs.com/package/mariadb)<br>[JavaScript](./with-database/use-mysql-mariadb.md?tabs=MariaDB#use-mariadb-sdk-to-connect-to-mariadb-on-azure)|

### Azure Cosmos DB for MongoDB

[Service documentation](/azure/cosmos-db/)

|Task|using|
|--|--|
|Create an Azure Cosmos DB for MongoDB resource|[Visual Studio Code extension](/azure/app-service/tutorial-nodejs-mongodb-app?tabs=azure-portal%2cterminal-bash%2cvscode-deploy%2cdeploy-instructions-azportal%2cdeploy-zip-linux-mac%2cdeploy-instructions--zip-azcli)<br>[Azure CLI](./with-database/use-mongodb-as-cosmosdb.md?tabs=azure-cli%2cmongodb#create-a-cosmos-db-resource-for-mongodb)|
|Get Azure Cosmos DB connection string|[Visual Studio Code extension](/azure/app-service/tutorial-nodejs-mongodb-app?tabs=azure-portal%2cterminal-bash%2cvscode-deploy%2cdeploy-instructions-azportal%2cdeploy-zip-linux-mac%2cdeploy-instructions--zip-azcli#get-cosmos-db-connection-string)<br>[Azure CLI](./with-database/use-mongodb-as-cosmosdb.md?tabs=azure-cli%2cmongodb#get-the-mongodb-connection-string-for-your-resource)|
|View Azure Cosmos DB|[Azure Cosmos DB Explorer](https://cosmos.azure.com/)|
|Use Mongoose SDK with Azure Cosmos DB for MongoDB|[JavaScript](./with-database/use-mongodb-as-cosmosdb.md#use-mongoose-sdk-to-connect-to-mongodb-on-azure)

### MySQL

[Service documentation](/azure/mysql/)

|Task|Using|
|--|--|
|Create resource|[Azure portal](https://ms.portal.azure.com/#create/Microsoft.MySQLServer)<br>[Azure CLI](./with-database/use-mysql-mariadb.md?tabs=MySQL#create-an-azure-database-for-mysql-resource-with-azure-cli)<br>[@azure/arm-mysql](https://www.npmjs.com/package/@azure/arm-mysql)|
|Create database on resource|[Azure CLI](./with-database/use-mysql-mariadb.md?tabs=MySQL#create-a-database-on-the-server-with-azure-cli)|
|Get Connection string|[Azure CLI](./with-database/use-mysql-mariadb.md?tabs=MySQL#get-the-mysql-connection-string-with-azure-cli)|
|Use and view database|[MySQL Workbench](https://www.mysql.com/products/workbench/)<br>[Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools-driver-mysql)<br>[npm mysql](https://www.npmjs.com/package/mysql)<br>[npm promise-mysql](https://www.npmjs.com/package/promise-mysql)|
|Use promise-mysql API|[JavaScript](./with-database/use-mysql-mariadb.md?tabs=MySQL#use-promise-mysql-sdk-to-connect-to-mysql-on-azure)|

### PostgreSQL

[Service documentation](/azure/postgresql/)

|Task|using|
|--|--|
|Create resource|[Visual Studio Code extension](./with-visual-studio-code/create-azure-database.md#create-a-postgresql-server-for-cosmos-db)<br>[Azure CLI](./with-database/use-postgresql-db.md#create-an-azure-database-for-postgresql-resource)<br>[Azure portal](https://ms.portal.azure.com/#create/Microsoft.PostgreSQLServer)<br>[@azure/arm-postgresql](https://www.npmjs.com/package/@azure/arm-postgresql)|
|Get connection string|[Azure CLI](./with-database/use-postgresql-db.md#get-the-postgresql-connection-string-with-azure-cli)|
|View DB|[Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)<br>[Azure Cloud Shell's psql](https://shell.azure.com/)|
|Use pg API for DB|[JavaScript](./with-database/use-postgresql-db.md#use-pg-sdk-to-connect-to-postgresql-on-azure)

### Azure Cosmos DB for NoSQL

* [Service documentation](/azure/cosmos-db/)
* [@azure/cosmos](https://www.npmjs.com/package/@azure/cosmos) npm package

|Task|using|
|--|--|
|Add firewall rule for your client IP address|[Azure CLI](./with-database/use-sql-api-as-cosmos-db.md#add-firewall-rule-for-your-client-ip-address)
|Create an Azure Cosmos DB for NoSQL resource|[Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)<br>[Azure CLI](./with-database/use-sql-api-as-cosmos-db.md#create-a-cosmos-db-resource-for-sql-api)|
|Get Azure Cosmos DB keys|[Azure CLI](./with-database/use-sql-api-as-cosmos-db.md#get-the-cosmos-db-keys-for-your-resource)|
|Get Azure Cosmos DB connection string|[Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)|
|View Azure Cosmos DB|[Azure Cosmos DB Explorer](https://cosmos.azure.com/)|
|Use Azure Cosmos DB for NoSQL|[JavaScript](./with-database/use-sql-api-as-cosmos-db.md#use--sdk-to-connect-to-database)

### GraphQL

|Task|using|
|--|--|
|Deploy a `Hello World` GraphQL API as an Azure Function|[VSCode](with-web-app/graphql/azure-function-hello-world.md)|

## Deployment to hosting environment

|Task|using|
|--|--|
|Static Web App (SWA)|[VS Code](/azure/search/tutorial-javascript-deploy-static-web-app)<br>[From Framework (Next.js)](/azure/static-web-apps/deploy-nextjs#deploy-your-static-website)<br>[Azure Pipelines](/azure/static-web-apps/publish-devops#create-the-pipeline-task-in-azure-devops)|
|Functions|[VS Code](../tutorial/azure-function-cosmos-db-mongo-api.md)<br>[Azure Pipelines](/azure/azure-functions/functions-how-to-azure-devops?tabs=javascript%2Cwindows)<br>[GitHub Actions](/azure/azure-functions/functions-how-to-github-actions?tabs=javascript#deploy-the-function-app)|
|App service|[Express.js with VS Code](/azure/app-service/tutorial-nodejs-mongodb-app?tabs=azure-portal%2cterminal-bash%2cvscode-deploy%2cdeploy-instructions-azportal%2cdeploy-zip-linux-mac%2cdeploy-instructions--zip-azcli#5-create-app-service-resource-in-visual-studio-code)<br>[Git Push](/azure/app-service/tutorial-nodejs-mongodb-app?tabs=azure-portal%2cterminal-bash%2cvscode-deploy%2cdeploy-instructions-azportal%2cdeploy-zip-linux-mac%2cdeploy-instructions--zip-azcli#make-change-and-deploy-to-azure-app-service-from-local-git)<br>[GitHub Actions](/azure/app-service/deploy-github-actions?tabs=applevel)<br>[Azure DevOps](/azure/app-service/deploy-continuous-deployment?tabs=repos)|

## Git

|Task|using|
|--|--|
|Create a local branch|[Visual Studio Code with Command Palette](./with-visual-studio-code/clone-github-repository.md#create-a-branch-for-changes)|
|Clone project from GitHub to local computer|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md)|
|Push a local branch to remote repo|[Visual Studio Code](./with-visual-studio-code/clone-github-repository.md#push-a-local-branch-to-github)|

## GitHub 

### Actions 

|Task|using|
|--|--|
|Add secrets|[Visual Studio Code](../tutorial/static-web-app-image-analysis.md#create-a-static-web-app-resource)|
|View build process|[GitHub website](../tutorial/static-web-app-image-analysis.md#view-the-github-action-build-process)|

## Key Vault

|Task|using|
|--|--|
|Get secrets|[@azure/keyvault-secrets](https://github.com/Azure-Samples/msdocs-nodejs-mongodb-azure-sample-app/blob/main/config/keyvault.js)|


## Monitoring

|Task|using|
|--|--|
|Create resource|[Azure CLI](../tutorial/nodejs-virtual-machine-vm/create-azure-monitoring-application-insights-web-resource.md#create-azure-monitor-resource-with-azure-cli)|

## Storage

[Service documentation](/azure/storage/)

|Task|using|
|--|--|
|Create resource|[Visual Studio Code extension](../tutorial/browser-file-upload-azure-storage-blob.md#3-create-storage-resource-with-visual-studio-extension)|
|Delete resource|[Visual Studio Code extension](../tutorial/browser-file-upload-azure-storage-blob.md#clean-up-resources)|
|Create Storage container shared access signature (SAS) token|[Portal](../tutorial/browser-file-upload-azure-storage-blob.md#5-generate-your-shared-access-signature-sas-token)|
|Set SAS token in code|[TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#set-sas-token-in-code-file)|
|Configure CORS for Storage|[Portal](../tutorial/browser-file-upload-azure-storage-blob.md#6-configure-cors-for-azure-storage-resource)|
|Use Azurite Storage emulator|[Visual Studio Code](./with-web-app/azure-function-file-upload.md)|

### Blobs

|Task|using|
|--|--|
|Create container in storage with [`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob)|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#create-storage-client-and-manage-steps)|
|Upload file to storage with [`@azure/storage-blob`](https://www.npmjs.com/package/@azure/storage-blob)|[React/TypeScript](../tutorial/browser-file-upload-azure-storage-blob.md#upload-button-functionality)|
|Upload file to Storage with Function API|[TypeScript with out binding](./with-web-app/azure-function-file-upload.md)|

## Terminal usage

|Task|using|
|--|--|
|Integrated terminal|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md)|

## Virtual machines

[Service documentation](/azure/virtual-machines/)

|Task|using|
|--|--|
|Create - with cloud-init file|[YAML](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md#create-a-cloud-init-file-to-expedite-linux-virtual-machine-creation)|
|Create - with Azure CLI - linux VM resource|[Azure CLI](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md#create-a-virtual-machine-resource)|
|Connect - Open port of linux VM|[Azure CLI](../tutorial/nodejs-virtual-machine-vm/create-linux-virtual-machine-azure-cli.md#open-port-for-virtual-machine)|
|Connect - with SSH|[Bash](../tutorial/nodejs-virtual-machine-vm/connect-linux-virtual-machine-ssh.md#connect-with-ssh-and-change-web-app)|
|Get Status|[@azure/arm-compute](with-azure-sdk/stop-start-virtual-machine.md#get-status-of-subscription-virtual-machines)|
|List VMs|[@azure/arm-compute](with-azure-sdk/stop-start-virtual-machine.md#list-subscription-virtual-machines)|
|Stop VM|[@azure/arm-compute](with-azure-sdk/stop-start-virtual-machine.md#stop-a-virtual-machine)|
|Start VM|[@azure/arm-compute](with-azure-sdk/stop-start-virtual-machine.md#start-a-virtual-machine)|
|Logs - Install Monitoring SDK|[Bash](../tutorial/nodejs-virtual-machine-vm/connect-linux-virtual-machine-ssh.md#install-monitoring-sdk)|
|Logs - Add monitoring code to Express.js app|[applicationinsights](../tutorial/nodejs-virtual-machine-vm/azure-monitor-application-insights-nodejs-expressjs-code.md#edit-indexjs-for-logging-with-azure-monitor-application-insights)|
|View logs|[Azure CLI](../tutorial/nodejs-virtual-machine-vm/azure-monitor-application-insights-nodejs-expressjs-code.md#viewing-the-vm-logs-for-nginx-and-pm2)<br>[Portal](../tutorial/nodejs-virtual-machine-vm/azure-monitor-application-insights-logs.md#view-application-traces-in-azure-portal)|
|Delete|[@azure/arm-resources](./with-azure-sdk/create-manage-virtual-machine.md#clean-up-resources)|


## Visual Studio Code: Develop and debug JavaScript apps 

[Tool documentation](https://code.visualstudio.com/docs)

|Task|using|
|--|--|
|Code completion|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md#use-visual-studio-code-autocompletion-with-mongodb)|
|Debugging local Node.js app|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md#debugging-the-local-nodejs-app)|
|Local full-stack debugging|[Visual Studio Code](with-visual-studio-code/install-run-debug-nodejs.md#local-full-stack-debugging-in-visual-studio-code)|
|Navigate the project files and code|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md#navigate-the-project-files-and-code)|
|Running the local Node.js app|[Visual Studio Code](./with-visual-studio-code/install-run-debug-nodejs.md#running-the-local-nodejs-app)|

## Samples supporting these tasks

|Name | Description|
|--|--|
|React app with Function API|Locally build and deploy a React/TypeScript client application with an Azure Function API to an Azure Static Web App with a GitHub action.<br>[Tutorial](./with-web-app/static-web-app-with-swa-cli/introduction.md) - [Sample code](https://github.com/Azure-Samples/js-e2e-static-web-app-with-cli)|
|React app using Cognitive Services|Locally build and deploy a React/TypeScript client application to an Azure Static Web App with a GitHub action.<br>[Tutorial](../tutorial/static-web-app-image-analysis.md) - [Sample code](https://github.com/Azure-Samples/js-e2e-client-cognitive-services)|
|React app uploading file to Azure Storage Blobs|This sample project is a TypeScript React (create-react-app) framework client app with an HTML form to select a file for upload to Azure Storage Blobs.<br>[Tutorial](../tutorial/browser-file-upload-azure-storage-blob.md) - [Sample code](https://github.com/Azure-Samples/js-e2e-browser-file-upload-storage-blob)|
|React app with login button|The SPA built in this tutorial is a React app (create-react-app) with the following tasks:<br>* Log in using a Microsoft-supported login such as Office 365 or Outlook.com<br>* Log off from the application<br>[Tutorial](/azure/active-directory/develop/tutorial-v2-react)|
|Azure Function app with MongoDB|The MongoDB database functionality includes:<br>* Add item<br>* Delete item by ID<br>* Get item by ID<br>* Get all items<br><br>[Tutorial](../tutorial/azure-function-cosmos-db-mongo-api.md) - [Sample code](https://github.com/Azure-Samples/js-e2e-azure-function-mongodb)|
|Azure Function app upload file to Blob Storage|This article shows you how to create an Azure Function API, which uploads a file to Azure Storage using an _out_ binding to move the file contents from the API to Storage.<br><br>* Locally develop and run with Azurite Storage emulation and Azure Functions Core tools.<br>* Deploy to Azure Functions with a Storage resource<br>* Review logs in Application Insights<br><br>[Tutorial](./with-web-app/azure-function-file-upload.md) - [Sample code](https://github.com/azure-Samples/js-e2e-azure-function-upload-file)|
|Azure Function app to manage resource groups|In this article series, you'll create an Azure Function app with APIs to manage Azure resource groups.<br><br>Features and functionality of this article series:<br><br>* Create local Azure Function app project in Visual Studio Code<br>* Create function APIs boilerplate code in Visual Studio Code<br>* Deploy to Azure Functions<br>* Create service principal<br>* Configure local and remote application settings<br>* Use DefaultAzureCredential in both local and remote environments<br>* Use Azure SDKs to use Azure Identity and Azure Resource Management APIs<br>* Use your local and cloud APIs to create, delete, and list resource groups in your subscription.<br>[Article series](with-web-app/azure-function-resource-group-management/introduction.md) - [Sample code](https://github.com/Azure-Samples/js-e2e-azure-resource-management-functions)|
|Express.js app with Azure Cosmos DB for MongoDB|The tutorial demonstrates how to load and run the project locally with VSCode, using extensions, was well as how to run the code remotely on an App service. The tutorial includes creating an Azure Cosmos DB for MongoDB resource, getting the connection information, and setting that value in the app service configuration setting to connect to a cloud database.<br>[Tutorial](/azure/app-service/tutorial-nodejs-mongodb-app?tabs=azure-portal%2cterminal-bash%2cvscode-deploy%2cdeploy-instructions-azportal%2cdeploy-zip-linux-mac%2cdeploy-instructions--zip-azcli) - [Sample code](https://github.com/Azure-Samples/js-e2e-express-mongo)|
|Express.js app deployed to VM with cloud-init file|Create a Linux virtual machine (VM) for an Express.js app. The VM is configured with a cloud-init configuration file and includes NGINX and a GitHub repository for an Express.js app. Once the VM is running, you can connect to the VM with SSH, change the web app to including trace logging, and view the public Express.js server app in a web browser.<br>[Tutorial](../tutorial/nodejs-virtual-machine-vm/introduction.md) - [Sample code](https://github.com/Azure-Samples/js-e2e-express-mongo)|
|GraphQL serverless function| Hello world with TypeScript, ready to deploy to Azure Function. [Sample code](https://github.com/azure-samples/js-e2e-azure-function-graphql-hello)|
|GraphQL serverless CRUD function| CRUD operations with TypeScript, ready to deploy to Azure Function. [Sample code](https://github.com/azure-samples/js-e2e-azure-function-graphql-crud-operations)|
|GraphQL static web app|Static web app with React client and Azure Function, both with shared TypeScript models demonstrating a Trivia game, with data held in Azure Cosmos DB for NoSQL. [Sample code](https://github.com/azure-samples/js-e2e-graphql-cosmosdb-static-web-app)|

Use the JavaScript end-to-end snippets collection, [https://github.com/azure-samples/js-e2e](https://github.com/azure-samples/js-e2e), to find or submit JavaScript or TypeScript code examples. 

Use the [Azure Samples browser](/samples/browse/?languages=javascript%2cnodejs%2ctypescript) to find more samples supporting your specific use case. 

## Next steps

* [Deploy a web app](deploy-web-app.md)
