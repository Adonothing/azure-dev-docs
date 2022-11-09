---
title: Use JavaScript on Azure Cosmos DB with Azure Cosmos DB for NoSQL
description: To create a SQL database to Azure, you need an Azure Cosmos DB resource. 
ms.topic: how-to
ms.date: 08/08/2022
ms.custom: devx-track-js, devx-track-azurecli
---

# Develop a JavaScript application for Azure Cosmos DB for NoSQL

To create or use Azure Cosmos DB for NoSQL, create an Azure Cosmos DB resource. Learn how to create the Azure Cosmos DB resource and use your database.

<a name="locally-develop-with-the-cosmosdb-emulator"></a>

## Locally develop with the Azure Cosmos DB emulator

Learn how to install the [Azure Cosmos DB emulator](/azure/cosmos-db/local-emulator) and [start the emulator for Azure Cosmos DB for NoSQL development](/azure/cosmos-db/local-emulator?tabs=cli%2Cssl-netstd21#sql-api).

## Create a resource for an Azure Cosmos DB for NoSQL database

You can create a resource with:

* Azure CLI
* [Azure portal](https://portal.azure.com)
* Visual Studio Code extension - [Azure Databases](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)

[!INCLUDE [Azure CLI commands](../../includes/azure-cli-cosmos-db-sql-api.md)]

## View and use your Azure Cosmos DB for NoSQL database

While developing your Azure Cosmos DB for NoSQL database with JavaScript, use [Azure Cosmos DB explorer](https://cosmos.azure.com/) to work with your database.

:::image type="content" source="../../media/howto-database/cosmos-explorer-sql-api.png" alt-text="Use the Azure Cosmos DB explorer, found at `https://cosmos.azure.com/`, to view and work with your database.":::

The Azure Cosmos DB explorer is also available in the Azure portal, for your resource, as the **Data Explorer**.

## Use @azure/cosmos SDK to connect to database

Connect to your Azure Cosmos DB for NoSQL database using the following Azure SDK:

* [@azure/cosmos](https://www.npmjs.com/package/@azure/cosmos)

To connect and use your Azure Cosmos DB for NoSQL database with JavaScript, use the following procedure.

1. Make sure Node.js and npm are installed.
1. Create a Node.js project in a new folder:

    ```bash
    mkdir dataDemo && \
        cd dataDemo && \
        npm init -y && \
        npm install @azure/cosmos && \
        touch index.js && \
        code .
    ```

    The command:
    * creates a project folder named `dataDemo`
    * changes the Bash terminal into that folder
    * initializes the project, which creates the `package.json` file
    * creates the `index.js` script file
    * opens the project in Visual Studio Code

1. Copy the following JavaScript code into `index.js`:

    ```javascript
    const CosmosClient = require("@azure/cosmos").CosmosClient;

    // CHANGE THESE VALUES
    const COSMOS_DB_RESOURCE_NAME = "YOUR-RESOURCE-NAME";
    const COSMOS_DB_RESOURCE_KEY = "YOUR-RESOURCE-KEY";

    let client = null;      // Azure Cosmos DB connection object
    let db = null;          // DB object
    let container = null;   // Container object

    // data
    const DATABASE_DOCS = [
        { name: "Joe", job: "banking" },
        { name: "Jack", job: "security" },
        { name: "Jill", job: "pilot" }];
        
    const ALL_DOCS = null;

    // Azure Cosmos DB config
    const config = {
        COSMOSDB_SQL_API_URI: `https://${COSMOS_DB_RESOURCE_NAME}.documents.azure.com:443/`,
        COSMOSDB_SQL_API_KEY: COSMOS_DB_RESOURCE_KEY,
        COSMOSDB_SQL_API_DATABASE_NAME: "DemoDb",
        COSMOSDB_SQL_API_CONTAINER_NAME: "DemoContainer"
    }

    // Unique Id = Guid
    const newGuid = () => {
        const s4 = () => Math.floor((1 + Math.random()) * 0x10000).toString(16).substring(1);
        return `${s4() + s4()}-${s4()}-${s4()}-${s4()}-${s4() + s4() + s4()}`;
    }

    // insert array
    const insert = async (newItems) => {

        const results = [];
        for (const item of newItems) {

            item.id = newGuid();
            const result = await container.items.create(item);
            results.push(result.item);
        }
        return results;
    };
    // find all or by id
    const find = async (query) => {

        if (query == null) {
            query = "SELECT * from c"
        } else {
            query = `SELECT * from c where c.id = ${query}`
        }

        const result = await container.items
            .query(query)
            .fetchAll();

        return result && result.resources ? result.resources : [];
    }
    // remove all or by id
    const remove = async (id) => {

        // remove 1
        if (id) {
            await container.item(id).delete();
        } else {

            // get all items
            const items = await find();

            // remove all
            for await (const item of items) {
                await container.item(item.id).delete();
            }
        }

        return;
    }
    // connection with SDK
    const connect = () => {
        try {

            const connectToCosmosDB = {
                endpoint: config.COSMOSDB_SQL_API_URI,
                key: config.COSMOSDB_SQL_API_KEY
            }

            return new CosmosClient(connectToCosmosDB);

        } catch (err) {
            console.log('Azure Cosmos DB - can\'t connect - err');
            console.log(err);
        }
    }
    const connectToDatabase = async () => {

        client = connect();

        if (client) {

            // get DB
            const databaseResult = await client.databases.createIfNotExists({ id: config.COSMOSDB_SQL_API_DATABASE_NAME });
            db = databaseResult.database;

            if (db) {
                // get Container
                const containerResult = await db.containers.createIfNotExists({ id: config.COSMOSDB_SQL_API_CONTAINER_NAME });
                container = containerResult.container;
                return !!db;
            }
        } else {
            throw new Error("can't connect to database");
        }


    }

    // use Database
    const dbProcess = async (docs) => {

        // connect
        const db = await connectToDatabase();
        if (!db) throw Error("db not working")
        console.log("connected to " + config.COSMOSDB_SQL_API_DATABASE_NAME + "/" + config.COSMOSDB_SQL_API_CONTAINER_NAME)
        
        // insert new docs
        const insertResult = await insert(docs);
        console.log("inserted " + insertResult.length)

        // get all docs
        const findResult = await find(ALL_DOCS);
        console.log("found " + findResult.length);

        // remove all then make sure they are gone
        await remove(ALL_DOCS);
        const findResult3 = await find(ALL_DOCS);
        console.log("removed all, now have " + findResult3.length);

        return;

    }

    dbProcess(DATABASE_DOCS)
    .then(() => {
        console.log("done")
    }).catch(err => {
        console.log(err)
    })
    ```

1. Replace the following variables in the script:
    * `YOUR-RESOURCE-NAME` - the name you used when creating your Azure Cosmos DB resource
    * `YOUR-RESOURCE-KEY` - one of the read/write keys for your resource

1. Run the script.

    ```bash
    node index.js
    ```

    The results are:

    ```console
    connected to DemoDb/DemoContainer4
    inserted 3
    found 3
    removed all, now have 0
    done
    ```

## Next steps

* How to [deploy a JavaScript web app](../deploy-web-app.md)
* [Azure Cosmos DB for NoSQL documentation](/azure/cosmos-db)
* [Azure Cosmos DB for NoSQL quickstart](/azure/cosmos-db/create-sql-api-nodejs)
