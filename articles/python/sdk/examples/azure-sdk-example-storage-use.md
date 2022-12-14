---
title: Use Azure Storage with the Azure SDK for Python
description: Use the Azure SDK for Python libraries to access an existing blob container in an Azure Storage account and then upload a file to that container.
ms.date: 12/14/2022
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli, py-fresh-zinc
---

# Example: Access Azure Storage using the Azure libraries for Python

This example demonstrated how to use the Azure client libraries in Python application code to upload a file to that Blob storage container. The example assumes you've created the resources shown in [Example: Provision Azure Storage](azure-sdk-example-storage.md).

All the commands in this article work the same in Linux/macOS bash and Windows command shells unless noted.

## 1: Set up your local development environment

If you haven't already, set up an environment where you can run this code. Here are some options:

[!INCLUDE [create-environment-options](../../includes/create-environment-options.md)]

## 2: Install library packages

1. In your *requirements.txt* file, add line for the needed client library package and save the file:

    :::code language="txt" source="~/../python-sdk-docs-examples/storage/requirements_use.txt":::

1. In your terminal or command prompt, reinstall requirements:

    ```cmd
    pip install -r requirements.txt
    ```

## 3: Create a file to upload

Create a source file named *sample-source.txt* (as the code expects), with contents like the following text:

:::code language="txt" source="~/../python-sdk-docs-examples/storage/sample-source.txt":::

## 4: Use blob storage from app code

The following sections (numbered 4a and 4b) demonstrate two means to access the blob container created through [Example: Provision Azure Storage](azure-sdk-example-storage.md).

The [first method (section 4a below)](#4a-use-blob-storage-with-authentication) authenticates the app with `DefaultAzureCredential` as described in [Authenticate Azure hosted applications with DefaultAzureCredential](../authentication-local-development-service-principal.md). With this method, you must first assign the appropriate permissions to the app identity, which is the recommended practice.

The [second method (section 4b below)](#4b-use-blob-storage-with-a-connection-string) uses a connection string to access the storage account directly. Although this method seems simpler, it has two significant drawbacks:

- A connection string inherently authenticates the connecting agent with the Storage *account* rather than with individual resources within that account. As a result, a connection string provides grants broader authorization than may be required.

- A connection string contains an access key in plain text and therefore presents potential vulnerabilities if it's improperly constructed or improperly secured. If such a connection string is exposed, it can be used to access a wide range of resources within the Storage account.

For these reasons, we recommend using the authentication method in production code.

### 4a: Use blob storage with authentication

1. Create an environment variable named `AZURE_STORAGE_BLOB_URL`:

    # [cmd](#tab/cmd)

    ```cmd
    set AZURE_STORAGE_BLOB_URL=https://pythonazurestorage12345.blob.core.windows.net
    ```

    # [bash](#tab/bash)

    ```bash
    AZURE_STORAGE_BLOB_URL=https://pythonazurestorage12345.blob.core.windows.net
    ```

    ---

    Replace "pythonazurestorage12345" with the name of your specific storage account.

    This `AZURE_STORAGE_BLOB_URL` environment variable is used only by this example and it not used by the Azure libraries.

1. Create a file named *use_blob_auth.py* with the following code. The comments explain the steps.

    :::code language="python" source="~/../python-sdk-docs-examples/storage/use_blob_auth.py":::

    Reference links:
      - [DefaultAzureCredential (azure.identity)](/python/api/azure-identity/azure.identity.defaultazurecredential)
      - [BlobClient (azure.storage.blob)](/python/api/azure-storage-blob/azure.storage.blob.blobclient)

1. Attempt to run the code (which fails intentionally):

    ```cmd
    python use_blob_auth.py
    ```

1. Observe the error "This request is not authorized to perform this operation using this permission." The error is expected because the local service principal that you're using doesn't yet have permission to access the blob container.

1. Grant container permissions to the service principal using the Azure CLI command [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create) (it's a long one!):

    # [cmd](#tab/cmd)

    ```azurecli
    az role assignment create --assignee %AZURE_CLIENT_ID% ^
        --role "Storage Blob Data Contributor" ^
        --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
    ```

    # [bash](#tab/bash)

    ```azurecli
    az role assignment create --assignee $AZURE_CLIENT_ID \
        --role "Storage Blob Data Contributor" \
        --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
    ```

    ---

    The `--scope` argument identifies where this role assignment applies. In this example, you grant the "Storage Blob Data Contributor" role to the *specific* container named "blob-container-01".

    Replace `pythonazurestorage12345` with the exact name of your storage account. You can also adjust the name of the resource group and blob container, if necessary. If you use the wrong name, you see the error, "Can not perform requested operation on nested resource. Parent resource 'pythonazurestorage12345' not found."

    If needed, also replace `PythonAzureExample-Storage-rg` with the name of the resource group that contains your storage account. The resource group shown here is used in [Example: Provision Azure Storage](azure-sdk-example-storage.md).

    The `--scope` argument in this command also uses the AZURE_CLIENT_ID and AZURE_SUBSCRIPTION_ID environment variables, which you should already have set in your local environment for your service principal by following [Configure your local Python dev environment for Azure](../../configure-local-development-environment.md).

1. **Wait a minute or two for the permissions to propagate**, then run the code again to verify that it now works. If you see the permissions error again, wait a little longer, then try the code again.

For more information on role assignments, see [How to assign role permissions using the Azure CLI](/azure/role-based-access-control/role-assignments-cli).

### 4b: Use blob storage with a connection string

1. Create an environment variable named `AZURE_STORAGE_CONNECTION_STRING`, the value of which is the full connection string for the storage account. (This environment variable is also used by various Azure CLI comments.)

1. Create a Python file named *use_blob_conn_string.py* with the following code. The comments explain the steps.

    :::code language="python" source="~/../python-sdk-docs-examples/storage/use_blob_conn_string.py":::

1. Run the code:

    ```bash
    python use_blob_conn_string.py
    ```

Again, although this method is simple, a connection string authorizes all operations in a storage account. With production code, it's better to use specific permissions as described in the previous section.

## 5. Verify blob creation

After running the code of either method, go to the [Azure portal](https://portal.azure.com), navigate into the blob container to verify that a new blob exists named *sample-blob.txt* with the same contents as the *sample-source.txt* file:

![Azure portal page for the blob container, showing the uploaded file](../../media/azure-sdk-example-storage/portal-blob-container-file.png)

## 6: Clean up resources

```azurecli
az group delete -n PythonAzureExample-Storage-rg  --no-wait
```

Run this command if you don't need to keep the resources provisioned in this example and would like to avoid ongoing charges in your subscription.

[!INCLUDE [resource_group_begin_delete](../../includes/resource-group-begin-delete.md)]

## See also

- [Example: Provision a resource group](azure-sdk-example-resource-group.md)
- [Example: List resource groups in a subscription](azure-sdk-example-list-resource-groups.md)
- [Example: Provision a web app and deploy code](azure-sdk-example-web-app.md)
- [Example: Provision Azure Storage](azure-sdk-example-storage.md)
- [Example: Provision and query a database](azure-sdk-example-database.md)
- [Example: Provision a virtual machine](azure-sdk-example-virtual-machines.md)
- [Use Azure Managed Disks with virtual machines](azure-sdk-samples-managed-disks.md)
- [Complete a short survey about the Azure SDK for Python](https://microsoft.qualtrics.com/jfe/form/SV_bNFX0HECjzPWMiG?Q_CHL=docs)
