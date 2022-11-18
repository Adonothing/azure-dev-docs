---
author: KarlErickson
ms.author: karler
ms.date: 7/17/2020
---


### Migrate all certificates to KeyVault

Azure Spring Apps doesn't provide access to the JRE keystore, so you must migrate certificates to Azure KeyVault, and change the application code to access certificates in KeyVault. For more information, see [Get started with Key Vault certificates](/azure/key-vault/certificates/certificate-scenarios) and [Azure Key Vault Certificate client library for Java](/java/api/overview/azure/security-keyvault-certificates-readme).