---
title: Zugreifen auf die Graph-API
description: In diesem Dokument wird beschrieben, wie Sie Azure Active Directory Authentifizierung einer mobilen Anwendung hinzufügen, die mit xamarin erstellt wurde.
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 3b8852b088470a3db74a35d852ce62d2852dd737
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931013"
---
# <a name="accessing-the-graph-api"></a>Zugreifen auf die Graph-API

Führen Sie die folgenden Schritte aus, um die Graph-API in einer xamarin-Anwendung zu verwenden:

1. [Registrieren bei Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) im *windowsazure.com* -Portal, dann
2. [Konfigurieren von Diensten](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Schritt 3: Hinzufügen Active Directory Authentifizierung zu einer APP

Fügen Sie in Ihrer Anwendung mithilfe des nuget-Paket-Managers in Visual Studio oder Visual Studio für Mac einen Verweis auf **Azure Active Directory Authentifizierungs Bibliothek (Azure Adal)** hinzu.
Stellen Sie sicher, dass Sie **Vorabversions Pakete anzeigen** , um dieses Paket einzuschließen, da es sich noch in der Vorschau Phase befindet.

> [!IMPORTANT]
> Hinweis: Azure Adal 3,0 ist zurzeit eine Vorschauversion, und es liegen möglicherweise wichtige Änderungen vor, bevor die endgültige Version veröffentlicht wird. 

![Hinzufügen eines Verweises auf Azure Active Directory Authentifizierungs Bibliothek (Azure Adal)](graph-images/06.-adal-nuget-package.jpg)

In Ihrer Anwendung müssen Sie nun die folgenden Variablen auf Klassenebene hinzufügen, die für die Authentifizierung erforderlich sind.

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

Beachten Sie hier Folgendes `commonAuthority` : Wenn der Authentifizierungs Endpunkt ist `common` , wird Ihre APP **mehr**Instanzen fähig, was bedeutet, dass jeder Benutzer die Anmeldung mit seinen Active Directory Anmelde Informationen verwenden kann. Nach der Authentifizierung wird der Benutzer im Kontext seines eigenen Active Directory arbeiten, d. –., er erhält Details zu seinen Active Directory.

### <a name="write-method-to-acquire-access-token"></a>Write-Methode zum Abrufen des Zugriffs Tokens

Der folgende Code (für Android) startet die Authentifizierung und weist nach Abschluss das Ergebnis in zu `authResult` . Die IOS-und Windows Phone-Implementierungen unterscheiden sich geringfügig: der zweite Parameter ( `Activity` ) unterscheidet sich in IOS und fehlt bei Windows phone.

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

Im obigen Code `AuthenticationContext` ist für die Authentifizierung mit commonauthority verantwortlich. Es verfügt über eine- `AcquireTokenAsync` Methode, die Parameter als Ressource übernimmt, auf die zugegriffen werden muss, in diesem Fall, `graphResourceUri` `clientId` und `returnUri` . Die APP kehrt zu zurück, `returnUri` Wenn die Authentifizierung abgeschlossen ist. Dieser Code bleibt für alle Plattformen unverändert, aber der letzte Parameter, `AuthorizationParameters` , unterscheidet sich auf verschiedenen Plattformen und ist für die Steuerung des Authentifizierungs Flusses verantwortlich.

Im Fall von Android oder IOS übergeben wir `this` den Parameter an, `AuthorizationParameters(this)` um den Kontext gemeinsam zu verwenden, wohingegen in Windows er ohne einen Parameter als New übergeben wird `AuthorizationParameters()` .

### <a name="handle-continuation-for-android"></a>Fortsetzung für Android behandeln

Nach Abschluss der Authentifizierung sollte der Flow an die APP zurückgegeben werden. Im Fall von Android wird dies durch folgenden Code behandelt, der **MainActivity.cs**hinzugefügt werden sollte:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
```

### <a name="handle-continuation-for-windows-phone"></a>Fortsetzung für Windows Phone behandeln

Ändern Sie Windows Phone die- `OnActivated` Methode in der Datei **app.XAML.cs** mit dem folgenden Code:

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

Wenn Sie die Anwendung ausführen, sollte nun ein Authentifizierungs Dialogfeld angezeigt werden.
Nach erfolgreicher Authentifizierung werden Ihre Berechtigungen zum Zugriff auf die Ressourcen (in diesem Fall Graph-API) angefordert:

![Nach erfolgreicher Authentifizierung werden Ihre Berechtigungen zum Zugriff auf die Ressourcen in unserem Fall Graph-API](graph-images/08.-authentication-flow.jpg)

Wenn die Authentifizierung erfolgreich ist und Sie die APP für den Zugriff auf die Ressourcen autorisiert haben, sollten Sie eine `AccessToken` -und- `RefreshToken` Kombination in erhalten `authResult` . Diese Token werden für weitere API-Aufrufe und für die Autorisierung mit Azure Active Directory im Hintergrund benötigt.

![Diese Token werden für weitere API-Aufrufe und für die Autorisierung mit Azure Active Directory im Hintergrund benötigt.](graph-images/07.-access-token-for-authentication.jpg)

Beispielsweise können Sie mit dem folgenden Code eine Benutzerliste aus Active Directory erhalten. Sie können die Web-API-URL durch Ihre Web-API ersetzen, die durch Azure AD geschützt wird.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```
