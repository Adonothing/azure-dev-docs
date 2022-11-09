---
author: KarlErickson
ms.author: karler
ms.date: 8/25/2020
---

Now that you've completed your migration, verify that your application works as you expect. You can then make your application more cloud-native by using the following recommendations.

* Consider enabling your application to work with Spring Cloud Registry. This will enable your application to be dynamically discovered by other deployed Spring applications and clients. For more information, see [Prepare an application for deployment in Azure Spring Apps](/azure/spring-apps/how-to-prepare-app-deployment). Then, modify any application clients to use the Spring Client Load balancer. This allows the client to obtain addresses of all the running instances of the application and find an instance that works if another instance becomes corrupted or unresponsive. For more information, see [Spring Tips: Spring Cloud Load Balancer](https://spring.io/blog/2020/03/25/spring-tips-spring-cloud-loadbalancer) in the Spring Blog.

* Instead of making your application public, consider adding a [Spring Cloud Gateway](https://cloud.spring.io/spring-cloud-gateway/reference/html/) instance. Spring Cloud Gateway provides a single endpoint for all applications deployed in your Azure Spring Apps instance. If a Spring Cloud Gateway is already deployed, ensure that it's configured to route traffic to your newly deployed application.

* Consider adding a Spring Cloud Config server to centrally manage and version-control configuration for all your Spring Cloud applications. First, create a Git repository to house the configuration and configure the Azure Spring Apps instance to use it. For more information, see [Set up a Spring Cloud Config Server instance for your service](/azure/spring-apps/how-to-config-server). Then, migrate your configuration using the following steps:

  1. Inside the application's *src/main/resources* directory, create a *bootstrap.yml* file with the following contents:

        ```yml
          spring:
            application:
              name: <your-application-name>
        ```

  1. In the configuration Git repository, create a *\<your-application-name>.yml* file, where `your-application-name` is the same as in the preceding step. Move the settings from *application.yml* file in *src/main/resources* to the new file you just created. If the settings were previously in a *.properties* file, converted them to YAML first. You can find online tools or IntelliJ plugins to perform this conversion.

  1. Create an *application.yml* file in the directory above. You can use this file to define settings and resources that will be shared among all applications on the Azure Spring Apps instance. Such settings typically include data sources, logging settings, Spring Boot Actuator configuration, and others.

  1. Commit and push these changes to the Git repository.

  1. Remove the *application.properties* or *application.yml* file from the application.

* Consider adding a deployment pipeline for automatic, consistent deployments. Instructions are available [for Azure Pipelines](/azure/spring-apps/how-to-cicd), [for GitHub Actions](/azure/spring-apps/how-to-github-actions), and [for Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service).

* Consider using staging deployments to test code changes in production before they're available to some or all of your end users. For more information, see [Set up a staging environment in Azure Spring Apps](/azure/spring-apps/how-to-staging-environment).

* Consider adding service bindings to connect your application to supported Azure databases. These service bindings would eliminate the need for you to provide connection information, including credentials, to your Spring Cloud applications.

* Consider using Azure Application Insights to monitor performance and interactions of your applications. For more information, see [Application Insights Java In-Process Agent in Azure Spring Apps](/azure/spring-apps/how-to-application-insights).

* Consider adding Azure Monitor alert rules and action groups to quickly detect and address aberrant conditions. For more information, see [Tutorial: Monitor Spring Cloud resources using alerts and action groups](/azure/spring-apps/tutorial-alerts-action-groups).

* Consider replicating the Azure Spring Apps deployment in another region for lower latency and higher reliability and fault tolerance. Use [Azure Traffic Manager](/azure/traffic-manager) to load balance among deployments or use [Azure Front Door](/azure/frontdoor) to add SSL offloading and Web Application Firewall with DDoS protection.

* If geo-replication isn't necessary, consider adding an [Azure Application Gateway](/azure/application-gateway) to add SSL offloading and Web Application Firewall with DDoS protection.
