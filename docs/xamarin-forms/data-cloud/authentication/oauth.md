---
title: Authentifialisierer mit Identitäts Anbieter
description: In diesem Artikel wird erläutert, wie Xamarin.Auth zum Verwalten des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung verwendet wird.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 12f34e7bc77fd3978ccfdfb57cc95747123c5603
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657243"
---
# <a name="authenticate-users-with-an-identity-provider"></a>Authentifizieren von Benutzern mit einem Identitäts Anbieter

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)

_Xamarin.Auth ist ein Plattform-SDK für die Authentifizierung von Benutzern, und speichern ihre Konten an. Es enthält die OAuth-Authentifikatoren, die Unterstützung bieten, für die Nutzung von Identitätsanbietern wie Google, Microsoft, Facebook und Twitter. In diesem Artikel wird erläutert, wie Xamarin.Auth zum Verwalten des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung verwendet wird._

OAuth ist ein offener Standard für die Authentifizierung und ermöglicht einen Besitzer der Ressource um einem Ressourcenanbieter zu benachrichtigen, dass es sich bei Berechtigungen gewährt werden soll, an Dritte weiter um ihre Informationen zugreifen, ohne die Identität der Besitzer der Ressource freigeben zu müssen. Ein Beispiel dafür wäre die einen Benutzer zum Identitätsanbieter (z. B. Google, Microsoft, Facebook oder Twitter) benachrichtigen, dass die Berechtigung gewährt werden soll, um eine Anwendung auf ihre Daten zugreifen, ohne die Identität des Benutzers freigeben zu müssen aktiviert werden. Es wird häufig als Ansatz für Benutzer verwendet, Websites und Anwendungen, die mit einem Identitätsanbieter, ohne dass ihr Kennwort auf die Website oder Anwendung anmelden.

Eine allgemeine Übersicht über den Ablauf der Authentifizierung bei der Nutzung von OAuth-Identitätsanbieter lautet wie folgt aus:

1. Die Anwendung navigiert, einen Browser mit einer URL des Identitätsanbieters.
1. Der Identitätsanbieter Benutzerauthentifizierung verarbeitet und an die Anwendung einen Autorisierungscode zurück.
1. Die Anwendung tauscht den Autorisierungscode gegen ein Zugriffstoken vom Identitätsanbieter an.
1. Die Anwendung verwendet das Zugriffstoken für den Identitätsanbieter, z. B. eine API zum Anfordern von Daten mit basic-Benutzer den Zugriff auf APIs.

Die beispielanwendung veranschaulicht, wie Xamarin.Auth verwenden, um eine systemeigene Authentifizierungsablauf für Google zu implementieren. Während Sie Google als Identitätsanbieter in diesem Thema verwendet wird, ist der Ansatz mit anderen Identitätsanbietern gleichermaßen anwendbar. Weitere Informationen zur Authentifizierung mithilfe Googles OAuth 2.0-Endpunkts finden Sie unter [mithilfe von oauth2. 0, Google-APIs zugreifen](https://developers.google.com/identity/protocols/OAuth2) Googles-Website.

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Verwenden von Xamarin.Auth zum Authentifizieren von Benutzern

Xamarin.Auth unterstützt zwei Ansätze, um Anwendungen für die Interaktion mit autorisierungsendpunkt des Identitätsanbieters:

1. Verwenden eine eingebettete Webansicht. Während dies üblicherweise wurde, wird nicht mehr aus den folgenden Gründen empfohlen:

    - Die Anwendung, die Webansicht hostet, kann die Anmeldeinformationen des Benutzers vollständige Authentifizierung, nicht nur die OAuth-autorisierungsgewährung zugreifen, die für die Anwendung bestimmt ist. Dies verstößt gegen das Prinzip der geringsten Rechte, wie die Anwendung Zugriff auf leistungsfähigere Anmeldeinformationen, die hat als potenziell erhöht die Angriffsfläche der Anwendung erfordert.
    - Die hostanwendung konnte erfassen, Benutzernamen und Kennwörter, automatisch Senden von Formularen und umgehen Zustimmung des Benutzers, und Sitzungscookies zu kopieren und verwenden, um authentifizierte Aktionen als Benutzer ausführen.
    - Eingebettete Webansichten freigeben nicht des Authentifizierungsstatus mit anderen Anwendungen oder Webbrowser des Geräts, dass der Benutzer sich anmelden für jede autorisierungsanforderung ein tiefgestelltes Bedienungserlebnis angesehen wird.
    - Einige Autorisierungs-Endpunkten Maßnahmen zum Erkennen und Blockieren von autorisierungsanforderungen, die von Webansichten stammen.

1. Mithilfe der Webbrowser des Geräts, die der empfohlene Ansatz ist. Mithilfe des Geräte-Browsers für die OAuth-Anforderungen verbessert die Verwendbarkeit einer Anwendung, wie die Benutzer müssen nur den betreffenden Identitätsanbieter anzeigt, einmal pro Gerät, Verbessern der konvertierungsraten-Anmeldung und Autorisierung von Flows in der Anwendung anmelden. Der Gerätebrowser bietet verbesserte Sicherheit auch, wie Anwendungen sind in der Lage, um zu überprüfen und Ändern von Inhalt in eine Webansicht, aber nicht im Browser angezeigten Inhalt. Dies ist der Ansatz in diesem Artikel und Beispiel-Anwendung.

Ein allgemeinen Überblick darüber, wie die beispielanwendung Xamarin.Auth zum Authentifizieren von Benutzern und ihren grundlegenden Datenabruf verwendet, wird im folgenden Diagramm dargestellt:

![](oauth-images/google-auth.png "Verwenden von Xamarin.Auth zum Authentifizierung über Google")

Die Anwendung sendet eine Authentifizierungsanforderung an Google nutzen die `OAuth2Authenticator` Klasse. Eine Authentifizierungsantwort wird zurückgegeben, sobald der Benutzer erfolgreich mit Google über ihre Anmeldeseite authentifiziert hat ein Zugriffstoken enthält. Klicken Sie dann die Anwendung sendet eine Anforderung an Google für die basic-Benutzerdaten mithilfe der `OAuth2Request` Klasse, mit dem Zugriffstoken wird in der Anforderung enthalten.

### <a name="setup"></a>Setup

Ein Projekt für die Google-API-Konsole muss erstellt werden, um Google-Anmeldung in einer Xamarin.Forms-Anwendung zu integrieren. Dies kann folgendermaßen erfüllt werden:

1. Wechseln Sie zu der [Google-API-Konsole](http://console.developers.google.com) Website, und melden Sie sich mit Anmeldeinformationen von Google-Konto an.
1. Wählen Sie aus den Projekt-Dropdown-Pfeil ein vorhandenes Projekt oder erstellen Sie eine neue.
1. Wählen Sie in der Randleiste unter "API-Manager" **Anmeldeinformationen**, und wählen Sie dann die **OAuth Consent Bildschirmregisterkarte**. Wählen Sie eine **e-Mail-Adresse**, geben Sie eine **Produktnamen, die Benutzern angezeigt wird,** , und drücken Sie die **speichern**.
1. In der **Anmeldeinformationen** Registerkarte die **Anmeldeinformationen erstellen** Dropdown-Liste aus, und wählen Sie **OAuth-Client-ID**.
1. Klicken Sie unter **Anwendungstyp**, wählen Sie die Plattform, die die mobile Anwendung ausgeführt wird (**iOS** oder **Android**).
1. Geben Sie die erforderlichen Details ein, und wählen Sie die **erstellen** Schaltfläche.

> [!NOTE]
> Eine Client-ID kann eine Anwendung, die Zugriff auf Google-APIs aktiviert, und für mobile Anwendungen auf einer einzigen Plattform eindeutig ist. Aus diesem Grund eine **OAuth-Client-ID** erstellt werden soll, für jede Plattform, die Google-Anmeldung verwenden.

Nach der Durchführung dieser Schritte kann Xamarin.Auth zum Initiieren einer OAuth2-authentifizierungsfluss bei Google verwendet werden.

### <a name="creating-and-configuring-an-authenticator"></a>Erstellen und konfigurieren einen Authentifikator

Die Xamarin.Auth `OAuth2Authenticator` -Klasse ist verantwortlich für die Behandlung von OAuth-authentifizierungsflusses. Das folgende Codebeispiel zeigt die Instanziierung der `OAuth2Authenticator` Klasse bei der Authentifizierung mithilfe der Webbrowser des Geräts ausführen:

```csharp
var authenticator = new OAuth2Authenticator(
    clientId,
    null,
    Constants.Scope,
    new Uri(Constants.AuthorizeUrl),
    new Uri(redirectUri),
    new Uri(Constants.AccessTokenUrl),
    null,
    true);
```

Die `OAuth2Authenticator` Klasse erfordert eine Reihe von Parametern, die Folgendes gilt:

- **Client-ID** – Dies wird vom Client aus dem Projekt im abgerufen werden kann, die die Anforderung identifiziert die [Google-API-Konsole](http://console.developers.google.com).
- **Geheimer Clientschlüssel** – Dies dürfte `null` oder `string.Empty`.
- **Bereich** – dies identifiziert die API-Zugriff, die von der Anwendung angefordert wird, und der Wert informiert im Zustimmungsdialogfeld angezeigt, die dem Benutzer angezeigt wird. Weitere Informationen zu geltungsbereichen finden Sie unter [autorisieren-API-Anforderung](https://developers.google.com/+/web/api/rest/oauth) Googles-Website.
- **Autorisieren Sie die URL** – dies identifiziert die URL, in dem der Autorisierungscode aus abgerufen werden wird.
- **Umleitungs-URL** – dies identifiziert die URL, in dem die Antwort gesendet wird. Der Wert dieses Parameters muss einer der Werte, die in angezeigt entsprechen der **Anmeldeinformationen** Registerkarte für das Projekt in der [Google Developers Console](https://console.developers.google.com/).
- **AccessToken Url** – dies identifiziert die URL zum Anfordern von Zugriffstoken, nachdem Sie ein Autorisierungscode abgerufen wird.
- **GetUserNameAsync Func** – ein optionales `Func` wird, wird für asynchron den Benutzernamen des Kontos abzurufen, nachdem sie erfolgreich authentifiziert wurde.
- **Verwenden Sie die Native Benutzeroberfläche** – `boolean` Wert, der angibt, ob der Webbrowser zu verwenden, um die Authentifizierungsanforderung auszuführen.

### <a name="setup-authentication-event-handlers"></a>Ereignishandler für die Authentifizierung einrichten

Bevor Sie die Präsentation der Benutzeroberfläche, die einen Ereignishandler für die `OAuth2Authenticator.Completed` Ereignis muss registriert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
authenticator.Completed += OnAuthCompleted;
```

Dieses Ereignis wird ausgelöst, wenn der Benutzer erfolgreich authentifiziert oder bricht die Anmeldung ab.

Optional, einen Ereignishandler für die `OAuth2Authenticator.Error` Ereignis auch registriert werden kann.

### <a name="presenting-the-sign-in-user-interface"></a>Darstellen der Benutzeroberfläche-Anmeldung

Die Schnittstelle des angemeldeten Benutzers kann mit der eine Darstellung der Xamarin.Auth-Anmeldung, die in jedes plattformprojekt initialisiert werden muss, die dem Benutzer angezeigt. Im folgenden Codebeispiel wird veranschaulicht, wie zum Initialisieren des Vortragenden Anmeldung in der `AppDelegate` Klasse im iOS-Projekt:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Im folgenden Codebeispiel wird veranschaulicht, wie zum Initialisieren des Vortragenden Anmeldung in der `MainActivity` Klasse im Android-Projekt:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Die .NET Standard-Bibliotheksprojekts kann der Präsentator Anmeldung klicken Sie dann wie folgt aufrufen:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Beachten Sie, dass das Argument für die `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` Methode ist die `OAuth2Authenticator` Instanz. Wenn die `Login` -Methode wird aufgerufen, die die Schnittstelle des angemeldeten Benutzers wird an den Benutzer auf einer Registerkarte angezeigt, über Webbrowser des Geräts, das in den folgenden Screenshots dargestellt ist:

![](oauth-images/login.png "Google-Anmeldung")

### <a name="processing-the-redirect-url"></a>Verarbeitet die Umleitungs-URL

Nachdem der Benutzer den Authentifizierungsprozess abgeschlossen ist, gibt die Kontrolle an die Anwendung aus der Registerkarte des Webbrowsers zurück. Dies erfolgt durch die Registrierung eines benutzerdefinierten URL-Schemas für die umleitungs-URL, die zurückgegeben wird, aus der Authentifizierungsprozess, und klicken Sie dann erkennen und behandeln die benutzerdefinierte URL ein, nach der sie gesendet wird.

Bei der Auswahl eines benutzerdefinierten URL-Schemas, eine Anwendung zugeordnet werden soll, müssen Anwendungen ein Schema basierend auf einem an ihre jeweiligen verwenden. Dies kann erreicht werden, indem Sie den Namen der Bundle-Bezeichner unter iOS und den Paketnamen unter Android und umkehren, um das URL-Schema stellen Sie dann. Allerdings weisen einige Identitätsanbietern wie Google, Client-IDs, die basierend auf den Domänennamen, die dann rückgängig gemacht und als URL-Schema verwendet werden. Wenn es sich bei Google Client-Id erstellt z. B. `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, das URL-Schema werden `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Beachten Sie, dass nur ein einzelnes `/` können angezeigt werden, nachdem die Schemakomponente. Ein vollständiges Beispiel für eine umleitungs-URL, die mit einem benutzerdefinierten URL-Schema aus diesem Grund wird `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Wenn der Webbrowser eine Antwort vom Identitätsanbieter, die ein benutzerdefiniertes URL-Schema enthält erhält, versucht, die URL, zu laden, was die fehl. Stattdessen wird die benutzerdefinierte URL-Schema für das Betriebssystem gemeldet, durch Auslösen eines Ereignisses. Das Betriebssystem prüft dann, ob registrierte Schemas, und falls vorhanden, das Betriebssystem wird starten Sie die Anwendung, die das Schema registriert, und die umleitungs-URL senden.

Der Mechanismus zum Registrieren eines benutzerdefinierten URL-Schemas mit dem Betriebssystem, und behandeln das Schema ist für jede Plattform spezifisch.

#### <a name="ios"></a>iOS

Unter iOS, wird ein benutzerdefiniertes URL-Schema in registriert **"Info.plist"** , wie im folgenden Screenshot gezeigt:

![](oauth-images/info-plist.png "URL-Schema-Registrierung")

Die **Bezeichner** Werte sind möglich, und die **Rolle** Wert muss festgelegt werden, um **Viewer**. Die **Url-Schemas** -Wert, der mit beginnt `com.googleusercontent.apps`, aus dem iOS-Client-Id für das Projekt abgerufen werden kann, um auf [Google-API-Konsole](http://console.developers.google.com).

Wenn der Identitätsanbieter die autorisierungsanforderung abgeschlossen ist, erfolgt eine Umleitung an den umleitungs-URL der Anwendung. Da die URL ein benutzerdefiniertes Schema verwendet, führt dies zu iOS, die die Anwendung gestartet, übergeben Sie in der URL als Parameter starten, in dem vom verarbeitet die `OpenUrl` außer Kraft setzen, von der Anwendung `AppDelegate` -Klasse, die im folgenden Codebeispiel gezeigt wird:

```csharp
public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
{
    // Convert NSUrl to Uri
    var uri = new Uri(url.AbsoluteString);

    // Load redirectUrl page
    AuthenticationState.Authenticator.OnPageLoading(uri);

    return true;
}
```

Die `OpenUrl` -Methode konvertiert die empfangene-URL aus einer `NSUrl` in ein .NET `Uri`, vor der Verarbeitung der umleitungs-URL mit der `OnPageLoading` Methode einer öffentlichen `OAuth2Authenticator` Objekt. Dies bewirkt, dass Xamarin.Auth auf die Registerkarte des Webbrowsers zu schließen, und klicken Sie zum Analysieren der empfangenen OAuth-Daten.

#### <a name="android"></a>Android

Unter Android wird ein benutzerdefiniertes URL-Schema registriert ist, durch Angeben einer [ `IntentFilter` ](xref:Android.App.IntentFilterAttribute) -Attribut für die `Activity` behandelt, die das Schema. Wenn der Identitätsanbieter die autorisierungsanforderung abgeschlossen ist, erfolgt eine Umleitung an den umleitungs-URL der Anwendung. Da die URL ein benutzerdefiniertes Schema verwendet, Android, die die Anwendung gestartet wird, übergeben Sie in der URL als Parameter starten, in dem sie vom verarbeitet wird der `OnCreate` Methode der `Activity` registriert, um die benutzerdefinierte URL-Schema zu behandeln. Das folgende Codebeispiel zeigt die Klasse aus der beispielanwendung, die die benutzerdefinierte URL-Schema behandelt:

```csharp
[Activity(Label = "CustomUrlSchemeInterceptorActivity", NoHistory = true, LaunchMode = LaunchMode.SingleTop )]
[IntentFilter(
    new[] { Intent.ActionView },
    Categories = new [] { Intent.CategoryDefault, Intent.CategoryBrowsable },
    DataSchemes = new [] { "<insert custom URL here>" },
    DataPath = "/oauth2redirect")]
public class CustomUrlSchemeInterceptorActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        // Convert Android.Net.Url to Uri
        var uri = new Uri(Intent.Data.ToString());

        // Load redirectUrl page
        AuthenticationState.Authenticator.OnPageLoading(uri);

        Finish();
    }
}
```

Die `DataSchemes` Eigenschaft der [ `IntentFilter` ](xref:Android.App.IntentFilterAttribute) muss festgelegt werden, um die umgekehrten Client-ID, die von der Android-Client-Id für das Projekt abgerufen werden, auf [Google-API-Konsole](http://console.developers.google.com).

Die `OnCreate` -Methode konvertiert die empfangene-URL aus einer `Android.Net.Url` in ein .NET `Uri`, vor der Verarbeitung der umleitungs-URL mit der `OnPageLoading` Methode einer öffentlichen `OAuth2Authenticator` Objekt. Dies bewirkt, dass Xamarin.Auth zum Schließen der Registerkarte des Webbrowsers, und analysieren Sie die empfangenen OAuth-Daten.

> [!IMPORTANT]
> Unter Android Xamarin.Auth verwendet die `CustomTabs` -API, um die Kommunikation mit dem Webbrowser und Betriebssystem. Allerdings es ist nicht sichergestellt, dass eine `CustomTabs` kompatiblen Browser auf dem Gerät des Benutzers installiert werden.

### <a name="examining-the-oauth-response"></a>Untersuchen die OAuth-Antwort

Nach der Analyse der empfangenen Daten OAuth Xamarin.Auth löst die `OAuth2Authenticator.Completed` Ereignis. Im Ereignishandler für dieses Ereignis das `AuthenticatorCompletedEventArgs.IsAuthenticated` Eigenschaft kann verwendet werden, ermitteln Sie, ob die Authentifizierung erfolgreich war, ein, wie im folgenden Codebeispiel gezeigt:

```csharp
async void OnAuthCompleted(object sender, AuthenticatorCompletedEventArgs e)
{
  ...
  if (e.IsAuthenticated)
  {
    ...
  }
}
```

Die Daten gesammelt, die einer erfolgreichen Authentifizierung werden in der `AuthenticatorCompletedEventArgs.Account` Eigenschaft. Dies schließt ein Zugriffstoken, die zum Signieren von Anforderungen für Daten an eine API bereitgestellt, die vom Identitätsanbieter verwendet werden kann.

### <a name="making-requests-for-data"></a>Anfragen für Daten

Nachdem die Anwendung ein Zugriffstoken abruft, es wird zum Anfordern der `https://www.googleapis.com/oauth2/v2/userinfo` -API zur Anforderung grundlegende Benutzerdaten vom Identitätsanbieter. Anforderung wird mit der Xamarin.Auth `OAuth2Request` -Klasse, die eine Anforderung darstellt, die mit einem Konto aus abgerufenen authentifiziert werden ein `OAuth2Authenticator` -Instanz, wie im folgenden Codebeispiel gezeigt:

```csharp
// UserInfoUrl = https://www.googleapis.com/oauth2/v2/userinfo
var request = new OAuth2Request ("GET", new Uri (Constants.UserInfoUrl), null, e.Account);
var response = await request.GetResponseAsync ();
if (response != null)
{
  string userJson = response.GetResponseText ();
  var user = JsonConvert.DeserializeObject<User> (userJson);
}
```

Sowie die HTTP-Methode und die API-URL die `OAuth2Request` Instanz gibt auch an eine `Account` -Instanz, die das Zugriffstoken enthält, die die Anforderung an die URL gemäß signiert die `Constants.UserInfoUrl` Eigenschaft. Der Identitätsanbieter zurückgegeben grundlegende Benutzerdaten als eine JSON-Antwort, einschließlich der Benutzer und e-Mail-Adresse, vorausgesetzt, dass das Zugriffstoken als gültig erkennt. Klicken Sie dann die JSON-Antwort gelesen und in deserialisiert die `user` Variable.

Weitere Informationen finden Sie unter [eine Google-API Aufrufen](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) im Google Entwickler-Portal.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Speichern und Abrufen von Informationen auf Geräten

Sicheres Speichern von Xamarin.Auth `Account` Objekte in einem Konto zu speichern, sodass Anwendungen nicht immer verfügen Benutzer erneut authentifizieren. Die `AccountStore` Klasse ist zuständig für die Speicherung von Kontoinformationen und wird von den Keychain-Diensten in iOS, gesichert und die `KeyStore` Klasse in Android.

Im folgenden Codebeispiel wird veranschaulicht, wie ein `Account` Objekt wird sicher gespeichert werden:

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Gespeicherte Konten werden eindeutig identifiziert mithilfe eines Schlüssels des Kontos bestehen `Username` -Eigenschaft und eine Dienst-ID, d. h. eine Zeichenfolge, die beim Abrufen von Konten aus dem Store Konto verwendet wird. Wenn ein `Account` zuvor gespeichert wurde, wird Aufrufen der `Save` Methode erneut überschreibt es.

`Account` Objekte für ein Dienst kann, durch den Aufruf abgerufen werden der `FindAccountsForService` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

Die `FindAccountsForService` Methode gibt ein `IEnumerable` Auflistung von `Account` Objekte, mit dem ersten Element in der Auflistung, der als das passende Konto festgelegt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Xamarin.Auth zu verwenden, um die Verwaltung des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung ermöglichen. Xamarin.Auth bietet die `OAuth2Authenticator` und `OAuth2Request` Klassen, die von Xamarin.Forms-Anwendungen verwendet werden, um Identitätsanbieter wie Google, Microsoft, Facebook und Twitter zu nutzen.


## <a name="related-links"></a>Verwandte Links

- [OAuthNativeFlow (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)
- [OAuth 2.0 für systemeigene Apps](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Mit OAuth 2.0, den Zugriff auf Google-APIs](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
