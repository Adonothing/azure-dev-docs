---
title: Use Spring Data JDBC with Azure SQL Database
description: Learn how to use Spring Data JDBC with an Azure SQL Database.
documentationcenter: java
ms.date: 07/22/2022
ms.service: sql-database
ms.tgt_pltfrm: multiple
author: KarlErickson
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java, devx-track-azurecli, team=cloud_advocates, spring-cloud-azure
ms.contributors: judubois-01202022
---

# Use Spring Data JDBC with Azure SQL Database

This article demonstrates creating a sample application that uses [Spring Data JDBC](https://spring.io/projects/spring-data-jdbc) to store and retrieve information in [Azure SQL Database](/azure/sql-database/).

[JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) is the standard Java API to connect to traditional relational databases.

Azure SQL database also supports Azure Active Directory (Azure AD) authentication. Azure AD authentication is a mechanism for connecting to Azure SQL Database using identities defined in Azure AD. With Azure AD authentication, you can manage database user identities and other Microsoft services in a central location, which simplifies permission management. With Azure AD authentication, you can achieve passwordless connection. To learn more about deploying a Spring Data application to Azure Spring Apps and using managed identity, see [Tutorial: Deploy a Spring application to Azure Spring Apps with a passwordless connection to an Azure database](deploy-passwordless-spring-database-app.md?tabs=sqlserver).

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## Sample application

In this article, we will code a sample application. If you want to go faster, this application is already coded and available at [https://github.com/Azure-Samples/quickstart-spring-data-jdbc-sql-server](https://github.com/Azure-Samples/quickstart-spring-data-jdbc-sql-server).

[!INCLUDE [spring-data-sql-server-setup.md](includes/spring-data-sql-server-setup.md)]

## Generate the application by using Spring Initializr

Generate the application on the command line by running the following command:

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jdbc,sqlserver -d baseDir=azure-database-workshop -d bootVersion=2.7.7 -d javaVersion=1.8 | tar -xzvf -
```

## Configure Spring Boot to use Azure SQL Database

Open the *src/main/resources/application.properties* file, and add the following text:

```properties
logging.level.org.springframework.jdbc.core=DEBUG

spring.datasource.url=jdbc:sqlserver://${AZ_DATABASE_NAME}.database.windows.net:1433;database=demo;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
spring.datasource.username=spring@${AZ_DATABASE_NAME}
spring.datasource.password=${AZ_SQL_SERVER_PASSWORD}

spring.sql.init.mode=always
```

> [!WARNING]
> The configuration property `spring.sql.init.mode=always` means that Spring Boot will automatically generate a database schema, using the *schema.sql* file that we will create later, each time the server is started. This is great for testing, but remember that this will delete your data at each restart, so you shouldn't use it in production.

You should now be able to start your application by using the provided Maven wrapper as follows:

```bash
./mvnw spring-boot:run
```

Here's a screenshot of the application running for the first time:

:::image type="content" source="media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-01.png" alt-text="Screenshot of the running application." lightbox="media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-01.png":::

## Create the database schema

Spring Boot will automatically execute *src/main/resources/schema.sql* in order to create a database schema. Create that file and add the following content:

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT IDENTITY PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

Stop the running application, and start it again using the following command. The application will now use the `demo` database that you created earlier, and create a `todo` table inside it.

```bash
./mvnw spring-boot:run
```

## Code the application

Next, add the Java code that will use JDBC to store and retrieve data from your Azure SQL Database server.

[!INCLUDE [spring-data-jdbc-create-application.md](includes/spring-data-jdbc-create-application.md)]

Here's a screenshot of these cURL requests:

:::image type="content" source="media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-02.png" alt-text="Screenshot of the cURL test." lightbox="media/configure-spring-data-jdbc-with-azure-sql-server/create-sql-server-02.png":::

Congratulations! You've created a Spring Boot application that uses JDBC to store and retrieve data from Azure SQL Database.

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

## Additional resources

For more information about Spring Data JDBC, see Spring's [reference documentation](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference).

For more information about using Azure with Java, see [Azure for Java developers](../index.yml) and [Working with Azure DevOps and Java](/azure/devops/).
