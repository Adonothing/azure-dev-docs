---
title: "Load a secret from Azure Key Vault in a Spring Boot application"
description: In this tutorial, you create a Spring Boot app that reads a value from Azure Key Vault, and you deploy the app to Azure App Service and Azure Spring Apps.
ms.date: 01/31/2022
ms.service: key-vault
ms.topic: tutorial
ms.custom: devx-track-java, devx-track-azurecli, spring-cloud-azure
author: KarlErickson
ms.author: edburns
---

# Load a secret from Azure Key Vault in a Spring Boot application

This tutorial shows you how to use Key Vault in Spring Boot applications to secure sensitive configuration data and retrieve configuration properties from Key Vault. [Key Vault](/azure/key-vault/general/overview) provides secure storage of generic secrets, such as passwords and database connection strings.

## Prerequisites

- An Azure subscription - [create one for free](https://azure.microsoft.com/free).
- [Java Development Kit (JDK)](/java/azure/jdk/) version 8 or above.
- [Apache Maven](https://maven.apache.org)
- [Azure CLI](/cli/azure/install-azure-cli)
- If you don't have a Key Vault instance, follow [Quickstart: Create a key vault using the Azure portal](/azure/key-vault/general/quick-create-portal) to create a Key Vault instance. Also, make a note of the URI of the Key Vault instance, as you'll need it for the test application for this tutorial.
- If you don't have a Spring Boot application, create a Maven project with the [Spring Initializr](https://start.spring.io/). Remember to select **Maven Project** and, under **Dependencies**, add the **Spring Web**, **Spring Data JPA**, and **H2 Database** dependencies, and select Java version 8 or higher.

> [!IMPORTANT]
> Spring Boot version 2.5 or higher is required to complete the steps in this article.

## Set a secret to Azure Key Vault

This tutorial describes how to read database credentials from Key Vault in a Spring Boot application. To read the credentials from Key Vault, you should first store database credentials in Key Vault.

To store the URL of an H2 database as a new secret in Key Vault, see [Quickstart: Set and retrieve a secret from Azure Key Vault using the Azure portal](/azure/key-vault/secrets/quick-create-portal). In this tutorial, you'll set a secret with name `h2url` and value `jdbc:h2:~/testdb;user=sa;password=password`.

> [!NOTE]
> After setting the secret, grant your app access to Key Vault by following the instructions in [Assign a Key Vault access policy](/azure/key-vault/general/assign-access-policy?tabs=azure-portal).

## Read a secret from Azure Key Vault

Now that database credentials have been stored in Key Vault, you can retrieve them with Spring Cloud Azure.

To install the Spring Cloud Azure Key Vault Starter module, add the following dependencies to your *pom.xml* file:

- The Spring Cloud Azure Bill of Materials (BOM):

  ```xml
   <dependencyManagement>
     <dependencies>
       <dependency>
         <groupId>com.azure.spring</groupId>
         <artifactId>spring-cloud-azure-dependencies</artifactId>
         <version>4.5.0</version>
         <type>pom</type>
         <scope>import</scope>
         </dependency>
     </dependencies>
   </dependencyManagement>
  ```

- The Spring Cloud Azure Key Vault Starter artifact:

  ```xml
  <dependency>
    <groupId>com.azure.spring</groupId>
    <artifactId>spring-cloud-azure-starter-keyvault</artifactId>
  </dependency>
  ```

Spring Cloud Azure has several methods for reading secrets from Key Vault. You can use the following methods independently or combine them for different use cases:

- Use Azure SDK for Key Vault.
- Use Spring KeyVault `PropertySource`.

### Use Azure SDK for Key Vault

Azure SDK for Key Vault provides `SecretClient` to manage secrets in Key Vault.

The following code example will show you how to use `SecretClient` to retrieve H2 database credentials from Azure Key Vault.

To read a secret using Azure SDK from Key Vault, configure the application by following these steps:

1. Configure a Key Vault endpoint in the *application.properties* configuration file.

   ```properties
   spring.cloud.azure.keyvault.secret.endpoint=https://<your-keyvault-name>.vault.azure.net/
   ```

   > [!TIP]
   > Be sure to sign in to Azure using the default authentication option `DefaultAzureCredential`. Otherwise, you won't be able to connect to Key Vault. For more information, see [Sign in with Azure CLI](/cli/azure/authenticate-azure-cli).

1. Inject the `SecretClient` bean in your Spring application and use the `getSecret` method to retrieve a secret, as shown in the following example:

   ```java
   import com.azure.security.keyvault.secrets.SecretClient;
   import org.springframework.boot.CommandLineRunner;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class SecretClientApplication implements CommandLineRunner {

       // Spring Cloud Azure will automatically inject SecretClient in your ApplicationContext.
       private final SecretClient secretClient;

       public SecretClientApplication(SecretClient secretClient) {
           this.secretClient = secretClient;
       }

       public static void main(String[] args) {
           SpringApplication.run(SecretClientApplication.class, args);
       }

       @Override
       public void run(String... args) {
           System.out.println("h2url: " + secretClient.getSecret("h2url").getValue());
       }
   }
   ```

1. Start the application. You'll see logs similar to the following example:

   ```output
   h2url: jdbc:h2:~/testdb;user=sa;password=password
   ```

You can build the `SecretClient` bean by yourself, but the process is complicated. In Spring Boot applications, you have to manage properties, learn the builder pattern, and register the client to your Spring application context. The following code example shows how you build a `SecretClient` bean:

```java
import com.azure.identity.DefaultAzureCredentialBuilder;
import com.azure.security.keyvault.secrets.SecretClient;
import com.azure.security.keyvault.secrets.SecretClientBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SecretClientConfiguration {

    @Bean
    public SecretClient createSecretClient() {
        return new SecretClientBuilder()
            .vaultUrl("https://<your-key-vault-url>.vault.azure.net/")
            .credential(new DefaultAzureCredentialBuilder().build())
            .buildClient();
    }

}
```

The following list shows some of the reasons why this code isn't flexible or graceful:

- The Key Vault endpoint is hard coded.
- If you use `@Value` to get configurations from the Spring environment, you can't have IDE hints in your *application.properties* file.
- If you have a microservice scenario, the code must be duplicated in each project, and it's easy to make mistakes and hard to be consistent.

Fortunately, building the `SecretClient` bean by yourself isn't necessary with Spring Cloud Azure. Instead, you can directly inject `SecretClient` and use the [configuration properties](spring-cloud-azure.md?tabs=maven#configuration-properties) that you're already familiar with to configure Key Vault.

Spring Cloud Azure also provides the following global configurations for different scenarios. For more information, see the [Global configuration for Azure Service SDKs](spring-cloud-azure.md?tabs=maven#global-configuration-for-azure-service-sdks) section of the [Spring Cloud Azure developer guide](spring-cloud-azure.md).

- Proxy options.
- Retry options.
- HTTP transport client options.

You can also connect to different Azure clouds. For more information, see [Connect to different Azure clouds](https://devblogs.microsoft.com/azure-sdk/announcing-the-stable-release-of-spring-cloud-azure-version-4-4-0/#connect-to-different-azure-clouds).

### Use Spring Key Vault PropertySource

The previous sections showed you how to use `SecretClient` in the `CommandLineRunner` to read the secret after the application started. In Spring Boot applications, however, reading secrets is required before the application starts. For example, the datasource password property is required before the application starts. The previous scenario won't work if you want to store the datasource password in Key Vault and still use the Spring auto-configuration to get a datasource.

In this case, Spring Cloud Azure provides Spring environment integration to load secrets from Key Vault before building the application context. You can use the secret to construct and configure the bean during Spring application context initialization. This approach is a transparent way for you to access secrets from Key Vault, and no code changes are required.

The following code example shows you how to use `PropertySource` to retrieve H2 database credentials to build the datasource from Azure Key Vault.

To retrieve the URL of an H2 database from Key Vault and store data from the H2 database using Spring Data JPA, configure the application by following these steps:

1. Add the following Key Vault endpoint and datasource properties to the *application.properties* configuration file.

   ```properties
   logging.level.org.hibernate.SQL=DEBUG

   spring.cloud.azure.keyvault.secret.property-sources[0].endpoint=https://<your-keyvault-name>.vault.azure.net/
   spring.datasource.url=${h2url}

   spring.jpa.hibernate.ddl-auto=create-drop
   spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
   ```

   > [!TIP]
   > For examples of Spring Cloud Azure property configuration, see the [Configuration examples](spring-cloud-azure.md#configuration-examples) section of the [Spring Cloud Azure developer guide](spring-cloud-azure.md).

   > [!TIP]
   > This example is a simple database scenario using an H2 database. We recommend using Azure Database for MySQL or Azure Database for PostgreSQL in a production environment and storing database URL, user name, and password in Azure Key Vault. If you want to avoid the password, passwordless connections is a good choice. For more information, see [Passwordless connections for Azure services](../../intro/passwordless-overview.md).

1. Create a new `Todo` Java class. This class is a domain model mapped onto the `todo` table that will be automatically created by JPA. The following code ignores the `getters` and `setters` methods.

   ```java
   import javax.persistence.Entity;
   import javax.persistence.GeneratedValue;
   import javax.persistence.Id;

   @Entity
   public class Todo {

       public Todo() {
       }

       public Todo(String description, String details, boolean done) {
           this.description = description;
           this.details = details;
           this.done = done;
       }

       @Id
       @GeneratedValue
       private Long id;

       private String description;

       private String details;

       private boolean done;

   }
   ```

1. Edit the startup class file to show the following content.

   ```java
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.boot.context.event.ApplicationReadyEvent;
   import org.springframework.context.ApplicationListener;
   import org.springframework.context.annotation.Bean;
   import org.springframework.data.jpa.repository.JpaRepository;

   import java.util.stream.Stream;

   @SpringBootApplication
   public class KeyvaultApplication {

       public static void main(String[] args) {
           SpringApplication.run(KeyvaultApplication.class, args);
       }

       @Bean
       ApplicationListener<ApplicationReadyEvent> basicsApplicationListener(TodoRepository repository) {
           return event->repository
               .saveAll(Stream.of("A", "B", "C").map(name->new Todo("configuration", "congratulations, you have set up "
                   + "correctly!", true)).toList())
               .forEach(System.out::println);
       }

   }

   interface TodoRepository extends JpaRepository<Todo, Long> {

   }
   ```

1. Start the application. The application will retrieve the URL of the H2 database from Key Vault, then connect to the H2 database, and store data to the database. You'll see logs similar to the following example:

   ```output
   2023-01-13 15:51:35.498 DEBUG 5616 --- [main] org.hibernate.SQL: insert into todo (description, details, done, id) values (?, ?, ?, ?)
   com.contoso.keyvault.Todo@1f
   ```

## Deploy to Azure Spring Apps

Azure Spring Apps is a fully managed platform for deploying and running your Spring Boot applications in Azure. For an overview of Azure Spring Apps, see [What is Azure Spring Apps?](/azure/spring-apps/overview)

This section will use your previous Spring Boot application and Key Vault instance. You'll deploy the app to Azure Spring Apps using Managed Identity authentication.

The following steps will show how to create an Azure Spring Apps resource and deploy the app to it.

1. If you haven't already done so, install the Azure Spring Apps extension by using the following command:

   ```azurecli
   az extension add --name spring
   ```

1. Decide on a name for the service instance. To use Azure Spring Apps within your Azure subscription, you must create an Azure resource of type Azure Spring Apps. As with all other Azure resources, the service instance must stay within a resource group. Use the resource group you've already created to hold the service instance, and choose a name for your Azure Spring Apps instance. Create the service instance with the following command.

   ```azurecli
   az spring create \
       --resource-group <your-resource-group-name> \
       --name <your-Azure-Spring-Apps-instance-name>
   ```

   This command takes several minutes to complete.

1. Create a Spring app within the service instance.

   ```azurecli
   az spring app create \
       --resource-group <your-resource-group-name> \
       --service <your-Azure-Spring-Apps-instance-name> \
       --name <your-Spring-Cloud-app-name> \
       --system-assigned \
       --assign-endpoint \
       --runtime-version Java_11
   ```

   This table explains the options shown.

   | Parameter         | Description                                                                           |
   |-------------------|---------------------------------------------------------------------------------------|
   | `resource-group`  | The name of the resource group where you created the existing service instance.       |
   | `service`         | The name of the existing service instance.                                            |
   | `name`            | The name of the app.                                                                  |
   | `system-assigned` | Enables system-assigned managed identity.                                             |
   | `assign-endpoint` | Assigns a public DNS domain name to the service.                                      |
   | `runtime-version` | The Java runtime version. The value must match the value chosen in Spring Initializr. |

1. Use the following command to get the managed identity for the created Spring app and use it to configure the existing Key Vault to allow access from this App.

   ```azurecli
   APP_IDENTITY_OBJECT_ID=$(az spring app show \
       --resource-group <your-resource-group-name> \
       --name <your-Spring-Cloud-app-name> \
       --service <your-Azure-Spring-Apps-instance-name> \
       --query identity.principalId \
       --output tsv)
   az keyvault set-policy \
       --name <your-Key-Vault-name> \
       --object-id $APP_IDENTITY_OBJECT_ID \
       --secret-permissions delete get list set purge
   ```

1. Add the following configuration to your application.

   Using Azure SDK for Key Vault:

   ```properties
   spring.cloud.azure.keyvault.secret.credential.managed-identity-enabled=true
   ```

   Using Spring KeyVault `PropertySource`:

   ```properties
   spring.cloud.azure.keyvault.secret.property-sources[0].credential.managed-identity-enabled=true
   ```

1. Deploy this app directly to the created Azure Spring Apps service instance by using the following command.

   ```azurecli
   az spring app deploy \
       --resource-group <your-resource-group-name> \
       --name <your-Spring-Cloud-app-name> \
       --artifact-path target/keyvault-0.0.1-SNAPSHOT.jar \
       --service <your-Azure-Spring-Apps-instance-name>
   ```

   This command creates a *deployment* within the app, within the service. For more information on the concepts of service instances, apps, and deployments see [App and deployment in Azure Spring Apps](/azure/spring-apps/concept-understand-app-and-deployment).

If the deployment isn't successful, configure the logs for troubleshooting as described in [Configure application logs](https://aka.ms/azure-spring-cloud-configure-logs). The logs will likely have useful information to diagnose and resolve the problem.

## Next steps

> [!div class="nextstepaction"]
> [Azure for Spring developers](../spring/index.yml)
> [Spring Cloud Azure KeyVault Samples](https://github.com/Azure-Samples/azure-spring-boot-samples/tree/main/keyvault)
