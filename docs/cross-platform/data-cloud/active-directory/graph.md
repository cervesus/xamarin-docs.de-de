---
title: Zugreifen auf die Graph-API
description: Mithilfe von Active Directory zum Abfragen der Graph-API, die mithilfe von Xamarin
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e177ac680a100a2723732c2ee7252ea0c16ea972
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="accessing-the-graph-api"></a>Zugreifen auf die Graph-API

_Mithilfe von Active Directory zum Abfragen der Graph-API, die mithilfe von Xamarin_

Gehen Sie zum Verwenden der Graph-API aus in einer Xamarin-Anwendung wie folgt vor:

1. [Registrieren bei Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) auf die *windowsazure.com* Portal klicken Sie dann
2. [Konfigurieren von Diensten](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Schritt 3 Hinzufügen von Active Directory-Authentifizierung zu einer app

Fügen Sie in der Anwendung einen Verweis auf **Azure Active Directory-Authentifizierungsbibliothek (Azure ADAL)** mithilfe des NuGet-Paket-Managers in Visual Studio oder Visual Studio für Mac.
Stellen Sie sicher, dass die Auswahl **Vorabversion Pakete anzeigen, die** dieses Paket eingeschlossen werden, da er noch in der Vorschau ist.

> [!IMPORTANT]
> Hinweis: Azure ADAL 3.0 ist zurzeit eine Vorschaufunktion und möglicherweise gibt es wichtige Änderungen vor der endgültigen Version veröffentlicht wird. 


![](graph-images/06.-adal-nuget-package.jpg "Hinzufügen eines Verweises auf Azure Active Directory-Authentifizierungsbibliothek (Azure ADAL)")

In der Anwendung müssen Sie jetzt die folgenden Variablen auf Klassenebene hinzufügen, die für den Authentifizierungsablauf erforderlich sind.

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

Beachten Sie, sieht `commonAuthority`. Wenn der authentifizierungsendpunkt ist `common`, wird Ihre app **mit mehreren Mandanten**, was bedeutet, dass jeder Benutzer Anmeldung mit den Anmeldeinformationen ihres Active Directory verwenden kann. Nach der Authentifizierung dieser Benutzer funktioniert im Kontext der Active Directory – d. h. es werden Details im Zusammenhang mit seinen Active Directory angezeigt.

### <a name="write-method-to-acquire-access-token"></a>Write-Methode zum Abrufen von Zugriffstoken

Der folgende Code (für Android) startet die Authentifizierung und weisen Sie nach dem Abschluss das Ergebnis in `authResult`. IOS- und Windows Phone-Implementierungen unterscheiden sich geringfügig: der zweite Parameter (`Activity`) unterscheidet sich für iOS und abwesende auf Windows Phone.

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

Im obigen Code die `AuthenticationContext` ist verantwortlich für die Authentifizierung mit CommonAuthority. Es wurde ein `AcquireTokenAsync` -Methode, die Parameter als Ressource zu verwenden, der in diesem Fall zugegriffen werden, muss `graphResourceUri`, `clientId`, und `returnUri`. Die app wird nun wieder die `returnUri` wenn Authentifizierung abgeschlossen wurde. Dieser Code weiterhin dem für alle Plattformen, aber der letzte Parameter `AuthorizationParameters`, weicht auf verschiedenen Plattformen und ist zuständig für die Verwaltung der Authentifizierungsablauf.

Im Fall von Android oder iOS, übergeben wir `this` Parameter `AuthorizationParameters(this)` den Kontext freigeben, während in Windows ohne Parameter als neue Übergabe `AuthorizationParameters()`.

### <a name="handle-continuation-for-android"></a>Behandeln von Fortsetzung für Android

Nach abgeschlossener Authentifizierung, sollte der Datenfluss zur app zurückzukehren. Im Fall von Android, es durch folgenden Code erfolgt, der hinzugefügt werden sollen **MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Behandeln von Fortsetzung für Windows Phone

Für Windows Phone-Ändern der `OnActivated` Methode in der **App.xaml.cs** -Datei mit den folgenden Code:

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
Bei erfolgreicher Authentifizierung kann fragt sie Ihre Berechtigungen für die Ressourcen (in unserem Fall Graph-API) zugreifen:

![](graph-images/08.-authentication-flow.jpg "Bei erfolgreicher Authentifizierung kann fragt sie, Ihren Berechtigungen den Zugriff auf die Ressourcen in unserem Fall Graph-API")

Wenn die Authentifizierung erfolgreich ist und Sie haben die app Zugriff auf die Ressourcen berechtigt, erhalten Sie eine `AccessToken` und `RefreshToken` Kombinationsfeld in `authResult`. Diese Token sind erforderlich, für weitere API-Aufrufe und für die Autorisierung mit Azure Active Directory im Hintergrund.

![](graph-images/07.-access-token-for-authentication.jpg "Diese Token sind erforderlich, für weitere API-Aufrufe und für die Autorisierung mit Azure Active Directory im Hintergrund")

Beispielsweise können mit der folgenden Code Benutzer eine Liste von Active Directory abgerufen. Sie können die Web-API-URL durch Ihre Web-API ersetzen, die durch Azure AD geschützt ist.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

