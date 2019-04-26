---
title: Zugreifen auf die Graph-API
description: In diesem Dokument wird beschrieben, wie eine mobile Anwendung mit Xamarin erstellten Azure Active Directory-Authentifizierung hinzugefügt.
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c43dfa79831f22e55490b27c3c360602ae717627
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61189966"
---
# <a name="accessing-the-graph-api"></a>Zugreifen auf die Graph-API

Um die Graph-API aus, in einer Xamarin-Anwendung verwenden, gehen Sie wie folgt vor:

1. [Registrieren bei Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) auf die *windowsazure.com* Portal, klicken Sie dann
2. [Konfigurieren von Diensten](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Schritt 3 Hinzufügen von Active Directory-Authentifizierung zu einer app

Fügen Sie in Ihrer Anwendung einen Verweis auf **Azure Active Directory-Authentifizierungsbibliothek (Azure ADAL)** mit den NuGet-Paket-Manager in Visual Studio oder Visual Studio für Mac.
Stellen Sie sicher, dass Sie **vorabversionspakete anzeigen** dieses Paket eingeschlossen werden, da es sich noch in der Vorschauphase ist.

> [!IMPORTANT]
> Hinweis: Azure ADAL 3.0 ist derzeit eine Vorschauversion, und möglicherweise Änderungen, bevor die endgültige Version veröffentlicht wird. 


![](graph-images/06.-adal-nuget-package.jpg "Hinzufügen eines Verweises auf Azure Active Directory-Authentifizierungsbibliothek (ADAL für Azure)")

In Ihrer Anwendung müssen Sie jetzt die folgenden Variablen auf Klassenebene hinzufügen, die für den Authentifizierungsablauf erforderlich sind.

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

Dabei ist zu beachten: `commonAuthority`. Wenn der authentifizierungsendpunkt ist `common`, wird Ihre app **mit mehreren Mandanten**, was bedeutet, dass jeder Benutzer mit ihren Active Directory-Anmeldeinformationen Anmeldung verwenden kann. Nach der Authentifizierung dieses Benutzers funktioniert im Kontext von eigenen Active Directory – also sehen die Details im Zusammenhang mit seinen Active Directory aus.

### <a name="write-method-to-acquire-access-token"></a>Write-Methode, um Zugriffstoken zu erhalten.

Der folgende Code (für Android) starten Sie die Authentifizierung wird, und weisen Sie nach Abschluss die Ergebnisse im `authResult`. IOS- und Windows Phone-Implementierungen unterscheiden sich geringfügig: zweiten Parameter (`Activity`) unterscheidet sich unter iOS und fehlt auf Windows Phone.

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

Im obigen Code der `AuthenticationContext` ist verantwortlich für die Authentifizierung mit CommonAuthority. Es wurde ein `AcquireTokenAsync` -Methode, die Parameter als Ressource verwenden, die in diesem Fall zugegriffen werden, muss `graphResourceUri`, `clientId`, und `returnUri`. Die app wird nun wieder die `returnUri` wenn Authentifizierung abgeschlossen ist. Dieser Code bleibt unverändert für alle Plattformen, jedoch den letzten Parameter `AuthorizationParameters`, unterscheiden sich auf verschiedenen Plattformen und ist verantwortlich für die Verwaltung der Ablauf der Authentifizierung.

Im Fall von Android- oder iOS, übergeben wir `this` Parameter `AuthorizationParameters(this)` den Kontext, freigeben, während Windows es ohne Parameter als neue übergeben wird, `AuthorizationParameters()`.

### <a name="handle-continuation-for-android"></a>Behandeln von Fortsetzung für Android

Nach der Authentifizierung abgeschlossen ist, sollte der Flow zur app zurückzukehren. Im Fall von Android, es durch folgenden Code erfolgt, der hinzugefügt werden sollen **"mainactivity.cs"**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Behandeln von Fortsetzung für Windows Phone

Ändern Sie für Windows Phone die `OnActivated` -Methode in der die **"App.Xaml.cs"** -Datei mit den folgenden Code:

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

Nun, wenn Sie die Anwendung ausführen, sollte ein Authentifizierungsdialogfeld angezeigt werden.
Nach der erfolgreichen Authentifizierung werden sie Ihre Berechtigungen für den Zugriff auf die Ressourcen (in unserem Fall Graph-API) gefragt:

![](graph-images/08.-authentication-flow.jpg "Nach der erfolgreichen Authentifizierung werden sie Ihre Berechtigungen für den Zugriff auf die Ressourcen in unserem Fall Graph-API gefragt.")

Wenn die Authentifizierung erfolgreich ist und Sie haben die app Zugriff auf die Ressourcen autorisiert, sollten Sie eine `AccessToken` und `RefreshToken` Kombinationsfeld in `authResult`. Diese Token werden für weitere API-Aufrufe und für die Autorisierung mit Azure Active Directory hinter den Kulissen erforderlich.

![](graph-images/07.-access-token-for-authentication.jpg "Diese Tokens sind für weitere API-Aufrufe und für die Autorisierung mit Azure Active Directory hinter den Kulissen erforderlich")

Beispielsweise können mit der folgenden Code Ihnen eine Benutzerliste aus Active Directory abzurufen. Sie können die Web-API-URL mit Ihrer Web-API ersetzen, die von Azure AD geschützt wird.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

