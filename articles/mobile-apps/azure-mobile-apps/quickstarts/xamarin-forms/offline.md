---
title: Add offline data sync to your Xamarin.Forms app
description: Add offline data sync to your Xamarin.Forms app using Azure Mobile Apps with our tutorial.
author: adrianhall
ms.service: mobile-services
ms.topic: article
ms.date: 06/17/2022
ms.author: adhal
zone_pivot_group_filename: developer/mobile-apps/azure-mobile-apps/zumo-zone-pivot-groups.json
zone_pivot_groups: vs-platform-options
---

# Add offline data sync to your Xamarin.Forms app

This tutorial covers the offline sync feature of Azure Mobile Apps for Xamarin Forms. Offline sync allows end users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there's no network connection. Changes are stored in a local database. Once the device is back online, these changes are synced with the remote backend.

Prior to starting this tutorial, you should have completed the [Xamarin.Forms Quickstart Tutorial](./index.md), which includes creating a suitable backend service.

To learn more about the offline sync feature, see the topic [Offline Data Sync in Azure Mobile Apps](../../howto/data-sync.md).

## Update the app to support offline sync

In online operation, you read to and write from a `IRemoteTable<T>`.  When using offline sync, you read to and write from a `IOfflineTable<T>` instead.  The `IOfflineTable` is backed by an on-device SQLite database, and synchronized with the backend database.

::: zone pivot="vs2022-windows"

[!INCLUDE[Update NuGet Dependencies on Windows.](~/mobile-apps/azure-mobile-apps/includes/quickstart/windows/add-offline-nuget.md)]

::: zone-end

::: zone pivot="vs2022-mac"

[!INCLUDE[Update NuGet Dependencies on macOS.](~/mobile-apps/azure-mobile-apps/includes/quickstart/mac/add-offline-nuget.md)]

::: zone-end

[!INCLUDE[Update RemoteService](~/mobile-apps/azure-mobile-apps/includes/quickstart/windows/add-offline-code.md)]

### Set the offline database location

In the `TodoApp.Forms` project, edit the `App.xaml.cs` file.  Change the definition of the `RemoteTodoService` as follows:

``` csharp
TodoService = new RemoteTodoService(async () => await GetAuthenticationToken())
{
    OfflineDb = Xamarin.Essentials.FileSystem.AppDataDirectory + "/offline.db"
};
```

If you haven't completed the [authentication tutorial](./authentication.md), the definition should look like this instead:

``` csharp
TodoService = new RemoteTodoService()
{
    OfflineDb = Xamarin.Essentials.FileSystem.AppDataDirectory + "/offline.db"
};
```

[!INCLUDE [Instructions for testing offline mode.](~/mobile-apps/azure-mobile-apps/includes/quickstart/common/test-offline-app.md)]

[!INCLUDE [Instructions to clean up resources.](~/mobile-apps/azure-mobile-apps/includes/quickstart/common/clean-up.md)]

## Next steps

* Review the HOW TO documentation:
  * [ASP.NET6 service documentation](~/mobile-apps/azure-mobile-apps/howto/server/dotnet-core.md)
  * [.NET client documentation](~/mobile-apps/azure-mobile-apps/howto/client/dotnet.md)

