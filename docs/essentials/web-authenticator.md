---
title: 'Xamarin.Essentials: Web Authenticator'
description: In diesem Dokument wird die WebAuthenticator-Klasse in Xamarin.Essentials beschrieben, mit der Sie browserbasierte Authentifizierungsflows starten können, die auf einen Rückruf an die App lauschen.
ms.assetid: 3D95371E-5D59-440E-8D31-F3C04E493DC1
author: redth
ms.author: jodick
ms.date: 03/26/2020
ms.openlocfilehash: 82c136e1de7d2aa7f2d7f132b8ee1639ee44aba4
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "80638155"
---
# <a name="xamarinessentials-web-authenticator"></a>Xamarin.Essentials: Web Authenticator

Mit der **WebAuthenticator**-Klasse können Sie browserbasierte Flows initiieren, die auf einen Rückruf an eine bestimmte URL lauschen, die für die App registriert ist.

## <a name="overview"></a>Übersicht

Viele Apps erfordern das Hinzufügen einer Benutzerauthentifizierung. Das bedeutet häufig, dass Sie den Benutzern die Anmeldung mit ihren vorhandenen Microsoft-, Facebook- oder Google-Konten und jetzt auch mit der Apple-Anmeldung ermöglichen.

[Microsoft Authentication Library (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview) bietet eine hervorragende, sofort einsetzbare Lösung zum Hinzufügen einer Authentifizierung zu Ihrer App. Es werden sogar Xamarin-Apps im jeweiligen NuGet-Paket des Clients unterstützt.

Wenn Sie an der Verwendung eines eigenen Webdiensts für die Authentifizierung interessiert sind, können Sie mit **WebAuthenticator** die clientseitige Funktionalität implementieren.

## <a name="why-use-a-server-back-end"></a>Gründe für die Verwendung eines Server-Back-Ends

Viele Authentifizierungsanbieter sind dazu übergegangen, nur explizite oder zweibeinige Authentifizierungsflows anzubieten, um höhere Sicherheit zu gewährleisten. Das bedeutet, dass Sie einen _geheimen Clientschlüssel_ vom Anbieter benötigen, um den Authentifizierungsflow durchzufühen. Leider sind mobile Apps nicht besonders gut für die Speicherung geheimer Schlüssel geeignet, und Elemente, die im Code, in Binärdateien oder an anderer Stelle in einer mobilen App gespeichert sind, werden allgemein als unsicher angesehen.

Die bewährte Methode hier ist die Verwendung eines Web-Back-Ends als mittlere Schicht zwischen der mobilen App und dem Authentifizierungsanbieter.

> [!IMPORTANT]
> Es wird dringend davon abgeraten, ältere Authentifizierungsbibliotheken und -muster allein auf mobiler Basis zu verwenden, bei denen aufgrund der fehlenden Sicherheit für das Speichern geheimer Clientschlüssel kein Web-Back-End im Authentifizierungsflow genutzt wird.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

Für den Zugriff auf die **WebAuthenticator**-Funktion ist die folgende plattformspezifische Einrichtung erforderlich.

# <a name="android"></a>[Android](#tab/android)

Für Android ist die Einrichtung eines Intent-Filters zur Verarbeitung des Rückruf-URI erforderlich. Dies lässt sich problemlos erreichen, indem Unterklassen der Klasse `WebAuthenticatorCallbackActivity` erstellt werden:

> [!NOTE]
> Sie sollten die Implementierung von [Android-App-Links](https://developer.android.com/training/app-links/) in Betracht ziehen, um den Rückruf-URI zu verarbeiten und sicherzustellen, dass sich Ihre Anwendung als Einzige für die Verarbeitung des Rückruf-URI registrieren kann.

```csharp
const string CALLBACK_SCHEME = "myapp";

[Activity(NoHistory = true, LaunchMode = LaunchMode.SingleTop)]
[IntentFilter(new[] { Intent.ActionView },
    Categories = new[] { Intent.CategoryDefault, Intent.CategoryBrowsable },
    DataScheme = CALLBACK_SCHEME)]
public class WebAuthenticationCallbackActivity : Xamarin.Essentials.WebAuthenticatorCallbackActivity
{
}
```

Sie müssen auch über die `OnResume`-Überschreibung in `MainActivity` einen Rückruf an Essentials durchführen:

```csharp
protected override void OnResume()
{
    base.OnResume();

    Xamarin.Essentials.Platform.OnResume();
}
```

# <a name="ios"></a>[iOS](#tab/ios)

Unter iOS müssen Sie das Muster der Rückruf-URI Ihrer App zu „Info.plist“ hinzufügen.

> [!NOTE]
> Sie sollten die Verwendung [universeller App-Links](https://developer.apple.com/documentation/uikit/inter-process_communication/allowing_apps_and_websites_to_link_to_your_content) in Erwägung ziehen, um den Rückruf-URI Ihrer App als empfohlene Vorgehensweise zu registrieren.

Außerdem müssen Sie die `OpenUrl`-Methode von `AppDelegate` überschreiben, um einen Rückruf an Essentials durchführen:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    if (Xamarin.Essentials.Platform.OpenUrl(app, url, options))
        return true;

    return base.OpenUrl(app, url, options);
}
```

# <a name="uwp"></a>[UWP](#tab/uwp)

Für UWP müssen Sie den Rückruf-URI in der Datei `Package.appxmanifest` deklarieren:

```xml
    <Extensions>
        <uap:Extension Category="windows.protocol">
            <uap:Protocol Name="myapp">
                <uap:DisplayName>My App</uap:DisplayName>
            </uap:Protocol>
        </uap:Extension>
    </Extensions>
```

-----

## <a name="using-webauthenticator"></a>Verwenden von WebAuthenticator

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die API besteht hauptsächlich aus einer einzelnen `AuthenticateAsync`-Methode, die zwei Parameter verwendet: Die URL, die zum Starten des Webbrowser-Flow verwendet werden soll, und der URI, für den der Flow letztendlich einen Rückruf durchführen soll und für dessen Verarbeitung die App registriert ist.

Das Ergebnis ist ein `WebAuthenticatorResult`, das alle vom Rückruf-URI analysierte Abfrageparameter enthält:

```csharp
var authResult = await WebAuthenticator.AuthenticateAsync(
        new Uri("https://mysite.com/mobileauth/Microsoft"),
        new Uri("myapp://"));

var accessToken = authResult?.AccessToken;
```

Die `WebAuthenticator`-API übernimmt das Starten der URL im Browser und wartet, bis der Rückruf empfangen wird:

![Typischer Webauthentifizierungsflow](images/web-authenticator.png)

Wenn der Benutzer den Flow an einem beliebigen Punkt abbricht, wird ein `null`-Ergebnis zurückgegeben.

## <a name="platform-differences"></a>Plattformunterschiede

# <a name="android"></a>[Android](#tab/android)

Sofern verfügbar, werden benutzerdefinierte Registerkarten verwendet, andernfalls wird ein Intent für die URL gestartet.

# <a name="ios"></a>[iOS](#tab/ios)

Unter iOS 12 oder höher wird `ASWebAuthenticationSession` verwendet.  Unter iOS 11 wird `SFAuthenticationSession` verwendet.  Bei älteren iOS-Versionen wird `SFSafariViewController` verwendet (sofern verfügbar), andernfalls wird Safari verwendet.

# <a name="uwp"></a>[UWP](#tab/uwp)

Auf der UWP wird `WebAuthenticationBroker` verwendet (sofern unterstützt), andernfalls wird der Systembrowser verwendet.

-----

## <a name="apple-sign-in"></a>Apple-Anmeldung

Gemäß den [Überprüfungsrichtlinien von Apple](https://developer.apple.com/app-store/review/guidelines/#sign-in-with-apple) muss Ihre App, wenn sie für die Authentifizierung einen Anmeldedienst für soziale Netzwerke verwendet, auch eine Apple-Anmeldung als Option anbieten.

Um Ihren Apps die Apple-Anmeldung hinzuzufügen, müssen Sie zunächst die [App für die Verwendung der Apple-Anmeldung konfigurieren](https://docs.microsoft.com/xamarin/ios/platform/ios13/sign-in).

Unter iOS 13 und höher sollten Sie die `AppleSignInAuthenticator.AuthenticateAsync()`-Methode aufrufen. Dadurch werden die nativen APIs für die Apple-Anmeldung im Hintergrund verwendet, damit den Benutzern auf diesen Geräten das bestmögliche Nutzungserlebnis geboten wird. Sie können den freigegebenen Code so schreiben, dass die richtige API zur Laufzeit verwendet wird:

```csharp
var scheme = "..."; // Apple, Microsoft, Google, Facebook, etc.
WebAuthenticatorResult r = null;

if (scheme.Equals("Apple")
    && DeviceInfo.Platform == DevicePlatform.iOS
    && DeviceInfo.Version.Major >= 13)
{
    // Use Native Apple Sign In API's
    r = await AppleSignInAuthenticator AuthenticateAsync();
}
else
{
    // Web Authentication flow
    var authUrl = new Uri(authenticationUrl + scheme);
    var callbackUrl = new Uri("xamarinessentials://");

    r = await WebAuthenticator.AuthenticateAsync(authUrl, callbackUrl);
}

var accessToken = r?.AccessToken;
```

> [!TIP]
> Bei Nicht-iOS 13-Geräten wird dadurch der Webauthentifizierungsflow gestartet, der auch zum Aktivieren der Apple-Anmeldung auf Ihren Android- und UWP-Geräten verwendet werden kann.

-----

## <a name="aspnet-core-server-back-end"></a>ASP.NET Core-Server-Back-End

Die `WebAuthenticator`-API kann mit einem beliebigen Web-Back-End-Dienst verwendet werden.  Zur Verwendung mit einer ASP.NET Core-App müssen Sie zunächst die Web-App mit den folgenden Schritten konfigurieren:

1. Richten Sie die gewünschten [externen Anbieter für die Authentifizierung über ein soziales Netzwerk](https://docs.microsoft.com/aspnet/core/security/authentication/social/?view=aspnetcore-3.1&tabs=visual-studio) in einer ASP.NET Core-Web-App ein.
2. Legen Sie das Standardauthentifizierungsschema im Aufruf von `.AddAuthentication()` auf `CookieAuthenticationDefaults.AuthenticationScheme` fest.
3. Verwenden Sie `.AddCookies()` im Aufruf von `.AddAuthentication()` in „Startup.cs“.
4. Alle Anbieter müssen mit `.SaveTokens = true;` konfiguriert sein.

> [!TIP]
> Wenn Sie die Apple-Anmeldung einschließen möchten, können Sie das NuGet-Paket `AspNet.Security.OAuth.Apple` verwenden.  Die vollständige [Beispieldatei „Startup.cs“](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/Startup.cs#L32-L60) finden Sie im GitHub-Repository für Essentials.

### <a name="add-a-custom-mobile-auth-controller"></a>Hinzufügen eines benutzerdefinierten mobilen Authentifizierungscontrollers

Bei einem mobilen Authentifizierungsflow ist es in der Regel wünschenswert, den Flow direkt für einen vom Benutzer ausgewählten Anbieter zu initiieren (z. B. durch Klicken auf die Schaltfläche „Microsoft“ auf dem Anmeldebildschirm der App).  Es ist auch wichtig, dass relevante Informationen über einen bestimmten Rückruf-URI an Ihre App zurückgegeben werden können, um den Authentifizierungsflow zu beenden.

Verwenden Sie dazu einen benutzerdefinierten API-Controller:

```csharp
[Route("mobileauth")]
[ApiController]
public class AuthController : ControllerBase
{
    const string callbackScheme = "myapp";

    [HttpGet("{scheme}")] // eg: Microsoft, Facebook, Apple, etc
    public async Task Get([FromRoute]string scheme)
    {
        // 1. Initiate authentication flow with the scheme (provider)
        // 2. When the provider calls back to this URL
        //    a. Parse out the result
        //    b. Build the app callback URL
        //    c. Redirect back to the app
    }
}
```

Der Zweck dieses Controllers besteht darin, das von der App angeforderte Schema (Anbieter) abzuleiten und den Authentifizierungsflow mit dem Anbieter des sozialen Netzwerks zu initiieren. Wenn der Anbieter einen Rückruf an das Web-Back-End durchführt, analysiert der Controller das Ergebnis und nimmt eine Umleitung an den Rückruf-URI der App mit Parametern vor.

Manchmal möchten Sie vielleicht Daten (z. B. `access_token` des Anbieters) an die App zurückgeben. Dies ist über die Abfrageparameter des Rückruf-URI möglich. Vielleicht möchten Sie auch stattdessen eine eigene Identität auf dem Server erstellen und Ihr eigenes Token an die App zurückgeben. Sie können entscheiden, was und wie Sie es machen.

Sehen Sie sich das [vollständige Controller-Beispiel](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/Controllers/MobileAuthController.cs) im Essentials-Repository an.

-----
## <a name="api"></a>API

- [WebAuthenticator-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/WebAuthenticator)
- [Dokumentation zur WebAuthenticator-API](xref:Xamarin.Essentials.WebAuthenticator)
- [Beispiel für den ASP.NET Core-Server](https://github.com/xamarin/Essentials/blob/develop/Samples/Sample.Server.WebAuthenticator/)
