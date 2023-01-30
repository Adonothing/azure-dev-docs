---
title: Usage patterns with the Azure libraries for Python
description: An overview of common usage patterns in the Azure SDK libraries for Python
ms.date: 01/30/2023
ms.topic: conceptual
ms.custom: devx-track-python, py-fresh-zinc
---

# Azure libraries for Python usage patterns

The Azure SDK for Python is composed of many independent libraries, which are listed on the [Python SDK package index](azure-sdk-library-package-index.md).

All the libraries share certain common characteristics and usage patterns, such as installation and the use of inline JSON for object arguments.

## Set up your local development environment

If you haven't already, you can set up an environment where you can run this code. Here are some options:

[!INCLUDE [create_environment_options](../includes/create-environment-options.md)]

## Library installation

# [pip](#tab/pip)

To install a specific library package, use `pip install`:

```cmd
# Install the management library for Azure Storage
pip install azure-mgmt-storage
```

```cmd
# Install the client library for Azure Blob Storage
pip install azure-storage-blob
```

`pip install` retrieves the latest version of a library in your current Python environment.

You can also use `pip` to uninstall libraries and install specific versions, including preview versions. For more information, see [How to install Azure library packages for Python](azure-sdk-install.md).

# [conda](#tab/conda)

To install a specific library package in a Conda environment, use `conda install`:

```cmd
# Install the Azyre management library package
conda install azure-mgmt
```

```cmd
# Install the client library for Azure Storage
conda install azure-storage
```

`conda install` retrieves the latest version of a package in your current Conda environment.

For more information, including how to remove packages or install specific versions, see [How to install Azure library packages for Python](azure-sdk-install.md).

---

## Asynchronous operations

Many operations that you invoke through client and management client objects (such as [`ComputeManagementClient.virtual_machines.begin_create_or_update`](/python/api/azure-mgmt-compute/azure.mgmt.compute.v2022_08_01.operations.virtualmachinesoperations#azure-mgmt-compute-v2022-08-01-operations-virtualmachinesoperations-begin-create-or-update) and [`WebSiteManagementClient.web_apps.begin_create_or_update`](/python/api/azure-mgmt-web/azure.mgmt.web.v2021_02_01.operations.webappsoperations#azure-mgmt-web-v2021-02-01-operations-webappsoperations-begin-create-or-update) return a poller for long running operations, `LROPoller[<type>]`, where `<type>` is specific to the operation in question. These methods are asynchronous.

> [!NOTE]
> You may notice differences in method names in a library, which is due to
version differences. Older libraries that aren't based on azure.core typically use names like `create_or_update`. Libraries based on azure.core add the `begin_` prefix to method names to better indicate that they are asynchronous. Migrating old code to a newer azure.core-based library typically means adding the `begin_` prefix to method names, as most method signatures remain the same.

The [`LROPoller`](/python/api/azure-core/azure.core.polling.lropoller) return type means that the operation is asynchronous. Accordingly, you must call that poller's `result` method to wait for the operation to finish and obtain its result.

The following code, taken from [Example: Provision and deploy a web app](./examples/azure-sdk-example-web-app.md), shows an example of using the poller to wait for a result:

:::code language="python" source="~/../python-sdk-docs-examples/webapp/provision_deploy_web_app.py" range="59-70":::

In this case, the return value of `begin_create_or_update` is of type `AzureOperationPoller[Site]`, which means that the return value of `poller.result()` is a [Site](/python/api/azure-mgmt-web/azure.mgmt.web.v2021_02_01.models.site) object.

## Exceptions

In general, the Azure libraries raise exceptions when operations fail to perform as intended, including failed HTTP requests to the Azure REST API. For app code, you can use `try...except` blocks around library operations.

For more information on the type of exceptions that may be raised, see the documentation for the operation in question.

## Logging

The most recent Azure libraries use the Python standard `logging` library to generate log output. You can set the logging level for individual libraries, groups of libraries, or all libraries. Once you register a logging stream handler, you can then enable logging for a specific client object or a specific operation. For more information, see [Logging in the Azure libraries](azure-sdk-logging.md).

## Proxy configuration

To specify a proxy, you can use environment variables or optional arguments. For more information, see [How to configure proxies](azure-sdk-configure-proxy.md).

## Optional arguments for client objects and methods

In the library reference documentation, you often see a `**kwargs` or `**operation_config` argument in the signature of a client object constructor or a specific operation method. These placeholders indicate that the object or method in question may support other named arguments. Typically, the reference documentation indicates the specific arguments you can use. There are also some general arguments that are often supported as described in the following sections.

### Arguments for libraries based on azure.core

These arguments apply to those libraries listed on [Python - New Libraries](https://azure.github.io/azure-sdk/releases/latest/#python). For example, here are a subset of the keyword arguments for `azure-core`. For a complete list, see the GitHub Readme for [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/main/sdk/core/azure-core#configurations).

| Name                       | Type | Default     | Description |
| ---                        | ---  | ---         | ---         |
| logging_enable             | bool | False       | Enables logging. For more information, see [Logging in the Azure libraries](azure-sdk-logging.md). |
| proxies                    | dict | {}          | Proxy server URLs. For more information, see [How to configure proxies](azure-sdk-configure-proxy.md). |
| use_env_settings           | bool | True        | If True, allows use of `HTTP_PROXY` and `HTTPS_PROXY` environment variables for proxies. If False, the environment variables are ignored. For more information, see [How to configure proxies](azure-sdk-configure-proxy.md). |
| connection_timeout         | int  | 300         | The timeout in seconds for making a connection to Azure REST API endpoints. |
| read_timeout               | int  | 300         | The timeout in seconds for completing an Azure REST API operation (that is, waiting for a response). |
| retry_total                | int  | 10          | The number of allowable retry attempts for REST API calls. Use `retry_total=0` to disable retries. |
| retry_mode                 | enum | exponential | Applies retry timing in a linear or exponential manner. If 'single', retries are made at regular intervals. If 'exponential', each retry waits twice as long as the previous retry. |

Individual libraries aren't obligated to support any of these arguments, so always consult the reference documentation for each library for exact details. Also, each library may support other arguments. For example, for blob storage specific keyword arguments, see the GitHub Readme for [azure-storage-blob](https://github.com/Azure/azure-sdk-for-python/tree/main/sdk/storage/azure-storage-blob#optional-configuration).

### Arguments for non-core libraries

| Name               | Type | Default | Description |
| ---                | ---  | ---     | ---         |
| verify             | bool | True    | Verify the SSL certificate. |
| cert               | str  | None    | Path to local certificate for client-side verification. |
| timeout            | int  | 30      | Timeout for establishing a server connection in seconds. |
| allow_redirects    | bool | False   | Enable redirects. |
| max_redirects      | int  | 30      | Maximum number of allowed redirects. |
| proxies            | dict | {}      | Proxy server URL. For more information, see [How to configure proxies](azure-sdk-configure-proxy.md). |
| use_env_proxies    | bool | False   | Enable reading of proxy settings from local environment variables. |
| retries            | int  | 10      | Total number of allowable retry attempts. |
| enable_http_logger | bool | False   | Enable logs of HTTP in debug mode. |

## Inline JSON pattern for object arguments

Many operations within the Azure libraries allow you to express object arguments either as discrete objects or as inline JSON.

For example, suppose you have a [`ResourceManagementClient`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.resourcemanagementclient) object through which you create a resource group with its [`create_or_update`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations#create-or-update-resource-group-name--parameters--custom-headers-none--raw-false----operation-config-)) method. The second argument to this method is of type [`ResourceGroup`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.models.resourcegroup).

To call `create_or_update` you can create a discrete instance of `ResourceGroup` directly with its required arguments (`location` in this case):

:::code language="python" source="~/../python-sdk-docs-examples/resource_group/provision_rg_objs.py" range="17-20":::

Alternately, you can pass the same parameters as inline JSON:

:::code language="python" source="~/../python-sdk-docs-examples/resource_group/provision_rg.py" range="16-21":::

When using JSON, the Azure libraries automatically convert the inline JSON to the appropriate object type for the argument in question.

Objects can also have nested object arguments, in which case you can also use nested JSON.

For example, suppose you have an instance of the [`KeyVaultManagementClient`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.keyvaultmanagementclient) object, and are calling its [`create_or_update`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.operations.vaultsoperations#create-or-update-resource-group-name--vault-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-) method. In this case, the third argument is of type [`VaultCreateOrUpdateParameters`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultcreateorupdateparameters), which itself contains an argument of type [`VaultProperties`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultproperties). `VaultProperties`, in turn, contains object arguments of type [`Sku`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.sku) and [`list[AccessPolicyEntry]`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.accesspolicyentry). A `Sku` contains a [`SkuName`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.skuname) object, and each `AccessPolicyEntry` contains a [`Permissions`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.permissions) object.

To call `begin_create_or_update` with embedded objects, you use code like the following (assuming `tenant_id` and `object_id` are already defined). You can also create the necessary objects before the function call.

:::code language="python" source="~/../python-sdk-docs-examples/key_vault/provision_key_vault.py" range="66-92":::

The same call using inline JSON appears as follows:

:::code language="python" source="~/../python-sdk-docs-examples/key_vault/provision_key_vault.py" range="97-121":::

Because both forms are equivalent, you can choose whichever you prefer and even intermix them. (The full code for these examples can be found on [GitHub](https://github.com/MicrosoftDocs/python-sdk-docs-examples/blob/main/key_vault/provision_key_vault.py).)

If your JSON isn't formed properly, you typically get the error, "DeserializationError: Unable to deserialize to object: type, AttributeError: 'str' object has no attribute 'get'". A common cause of this error is that you're providing a single string for a property when the library expects a nested JSON object. For example, using `"sku": "standard"` in the previous example generates this error because the `sku` parameter is a `Sku` object that expects inline object JSON, in this case `{ "name": "standard"}`, which maps to the expected `SkuName` type.

## Next steps

Now that you understand the common patterns for using the Azure libraries for Python, see the following standalone examples to explore specific management and client library scenarios. You can try these examples in any order as they're not sequential or interdependent.

- [Example: Create a resource group](./examples/azure-sdk-example-resource-group.md)
- [Example: Use Azure Storage](./examples/azure-sdk-example-storage.md)
- [Example: Provision a web app and deploy code](./examples/azure-sdk-example-web-app.md)
- [Example: Provision and query a database](./examples/azure-sdk-example-database.md)
- [Example: Provision a virtual machine](./examples/azure-sdk-example-virtual-machines.md)
- [Use Azure Managed Disks with virtual machines](./examples/azure-sdk-samples-managed-disks.md)

- [Complete a short survey about the Azure SDK for Python](https://microsoft.qualtrics.com/jfe/form/SV_bNFX0HECjzPWMiG?Q_CHL=docs)
