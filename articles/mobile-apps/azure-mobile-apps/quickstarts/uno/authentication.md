---
title: Add authentication to your Uno Platform app
description: Add authentication to your Uno Platform app using Azure Mobile Apps with our tutorial.
author: adrianhall
ms.service: mobile-services
ms.topic: article
ms.date: 02/24/2023
ms.author: adhal
---

# Add authentication to your Uno Platform app

In this tutorial, you add Microsoft authentication to the TodoApp project using Azure Active Directory. Before completing this tutorial, ensure you've [created the project and deployed the backend](./index.md).

> [!TIP]
> Although we use Azure Active Directory for authentication, you can use any authentication library you wish with Azure Mobile Apps.  

[!INCLUDE [Register with AAD for the backend](~/mobile-apps/azure-mobile-apps/includes/quickstart/common/register-aad-backend.md)]

[!INCLUDE [Configure the service for authentication](~/mobile-apps/azure-mobile-apps/includes/quickstart/windows/configure-auth-backend.md)]

## Add authentication to the app

The Microsoft Datasync Framework has built-in support for any authentication provider that uses a Json Web Token (JWT) within a header of the HTTP transaction.  This application uses the [Microsoft Authentication Library (MSAL)](/azure/active-directory/develop/msal-overview) to request such a token and authorize the signed in user to the backend service.  For more information on integrating MSAL into an Uno Platform project, review [their documentation](https://platform.uno/docs/articles/interop/MSAL.html);

[!INCLUDE [Configure a native app for authentication](~/mobile-apps/azure-mobile-apps/includes/quickstart/common/register-aad-client.md)]

Open the `TodoApp.sln` solution in Visual Studio. Add the [Uno.WinUI.MSAL]([/azure/active-directory/develop/msal-overview](https://platform.uno/docs/articles/interop/MSAL.html) to each of the `TodoApp.Uno` projects:

1. Right-click on the solution, then select **Manage NuGet Packages...**.
2. Select the **Browse** tab.
3. Enter `Uno.WinUI.MSAL` in the search box, then press Enter.
4. Select the `Uno.WinUI.MSAL` result.
5. In the right hand panel, select each of the `TodoApp.Uno` projects.
6. Select **Install**.
   
   ![Screenshot of selecting the Uno M S A L NuGet in Visual Studio.](./media/select-msal-nuget.png)

7. Accept the license agreement to continue the installation.

Add the native client ID and backend scope to the configuration.

Open the `TodoApp.Data` project and edit the `Constants.cs` file. Add constants for `ApplicationId` and `Scopes`:

``` csharp
  public static class Constants
  {
      /// <summary>
      /// The base URI for the Datasync service.
      /// </summary>
      public static string ServiceUri = "https://demo-datasync-quickstart.azurewebsites.net";

      /// <summary>
      /// The application (client) ID for the native app within Azure Active Directory
      /// </summary>
      public static string ApplicationId = "<client-id>";

      /// <summary>
      /// The list of scopes to request
      /// </summary>
      public static string[] Scopes = new[]
      {
          "<scope>"
      };
  }
```

Replace the `<client-id>` with the _Native Client Application ID_ you received when registering the client application in Azure Active Directory, and the `<scope>` with the _Web API Scope_ you copied when you used **Expose an API** while registering the service application.

Ensure you add the Microsoft Identity Library (MSAL) to all the `TodoApp.Uno` projects, including `TodoApp.Uno.Mobile`.  Repeat this proce

Open the `MainPage.xaml.cs` file in the top folder of the `TodoApp.Uno` project.  

Add the following `using` statements to the top of the file:

``` csharp
using Microsoft.Datasync.Client;
using Microsoft.Identity.Client;
using System.Diagnostics;
using System.Linq;
using Uno.UI.MSAL;
```

Remove the `TodoService` property and replace it with the following code:

``` csharp
    private readonly IPublicClientApplication _identityClient;
    private readonly TodoListViewModel _viewModel;
    private readonly ITodoService _service;

    public MainPage() {
        this.InitializeComponent();

#if ANDROID || IOS
        string redirectUri = $"msal{Constants.ApplicationId}://auth";
#else
        string redirectUri = "https://login.microsoftonline.com/common/oauth2/nativeclient";
#endif
        _identityClient = PublicClientApplicationBuilder
            .Create(Constants.ApplicationId)
            .WithAuthority(AzureCloudInstance.AzurePublic, "common")
            .WithRedirectUri(redirectUri)
            .WithUnoHelpers()
            .Build();
        _service = new RemoteTodoService();
        _viewModel = new TodoListViewModel(this, _service);
        mainContainer.DataContext = _viewModel;
    }
```

Add the following method into the `MainPage` class:

``` csharp
    public async Task<AuthenticationToken> GetAuthenticationToken()
    {
        var accounts = await _identityClient.GetAccountsAsync();
        AuthenticationResult result = null;
        bool tryInteractiveLogin = false;

        try
        {
            result = await _identityClient
                .AcquireTokenSilent(Constants.Scopes, accounts.FirstOrDefault())
                .ExecuteAsync();
        }
        catch (MsalUiRequiredException)
        {
            tryInteractiveLogin = true;
        }
        catch (Exception ex)
        {
            System.Diagnostics.Debug.WriteLine($"MSAL Silent Error: {ex.Message}");
        }

        if (tryInteractiveLogin)
        {
            try
            {
                result = await _identityClient
                    .AcquireTokenInteractive(Constants.Scopes)
                    .WithUnoHelpers()
                    .ExecuteAsync();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"MSAL Interactive Error: {ex.Message}");
            }
        }

        return new AuthenticationToken
        {
            DisplayName = result?.Account?.Username ?? string.Empty,
            ExpiresOn = result?.ExpiresOn ?? DateTimeOffset.MinValue,
            Token = result?.AccessToken ?? string.Empty,
            UserId = result?.Account?.Username ?? string.Empty
        };
    }
```

The `GetAuthenticationToken()` method works with the Microsoft Identity Library (MSAL) to get an access token suitable for authorizing the signed-in user to the backend service.  This function is then passed to the `RemoteTodoService` for creating the client.  If the authentication is successful, the `AuthenticationToken` is produced with data necessary to authorize each request.  If not, then an expired bad token is produced instead.

## Configure the Android app for authentication

Open the `TodoApp.Uno.Mobile` project and expand the `Android` folder.  Edit the `AndroidManifest.xml` file with the following contents:

``` xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
  <application>
          <activity android:name="microsoft.identity.client.BrowserTabActivity" android:configChanges="orientation|screenSize" android:exported="true">
                  <intent-filter>
                          <action android:name="android.intent.action.VIEW" />
                          <category android:name="android.intent.category.DEFAULT" />
                          <category android:name="android.intent.category.BROWSABLE" />
                          <data android:scheme="msal{Native Application ID}" android:host="auth" />
                  </intent-filter>
          </activity>
  </application>
</manifest>
```

Replace `{Native Application ID}` with the application ID of the native client (which is the same as `Constants.ApplicationId`).

Edit the `MainActivity.Android.cs` class, and add the following method:

``` csharp
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
    }
```

## Test the app

Run or restart the app.

When the app starts, a browser is opened to ask you for authentication.  If you haven't authenticated with the app before, the app asks for consent.  Once authentication is complete, the browser closes and your app continues as before.

## Next steps

Next, configure your application to operate offline by [implementing an offline store](./offline.md).

## Further reading

* [Quickstart: Protect a web API with the Microsoft identity platform](/azure/active-directory/develop/web-api-quickstart?pivots=devlang-aspnet-core)
  