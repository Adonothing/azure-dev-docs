---
author: KarlErickson
ms.author: karler
ms.date: 5/26/2020
---

#### Determine whether and how the file system is used

Find any instances where your services write to and/or read from the local file system. Identify where short-term/temporary files are written and read and where long-lived files are written and read.

> [!NOTE]
> Azure Spring Apps provides 5 GB of temporary storage per Azure Spring Apps instance, mounted in `/tmp`. If temporary files are written in excess of that limit or into a different location, code changes will be required.

<!-- The following two "static content" sections should be identical to the contents of includes\static-content.md except that here we use H5 headings. -->

##### Read-only static content

If your application currently serves static content, you'll need an alternate location for it. You may wish to consider moving static content to Azure Blob Storage and adding Azure CDN for lightning-fast downloads globally. For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website) and [Quickstart: Integrate an Azure storage account with Azure CDN](/azure/cdn/cdn-create-a-storage-account-with-cdn).

##### Dynamically published static content

If your application allows for static content that is uploaded/produced by your application but is immutable after its creation, you can use Azure Blob Storage and Azure CDN as described above, with an Azure Function to handle uploads and CDN refresh. We've provided a sample implementation for your use at [Uploading and CDN-preloading static content with Azure Functions](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn).
