---
title: Authentifialisierer mit Identitäts Anbieter
description: In diesem Artikel wird erläutert, wie Xamarin.Auth zum Verwalten des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung verwendet wird.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2019
ms.openlocfilehash: 0fa433de7fd1acb6fb27741f1615a644315f373f
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725559"
---
# <a name="authenticate-users-with-an-identity-provider"></a>Authentifizieren von Benutzern mit einem Identitäts Anbieter

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)

_Xamarin. auth ist ein plattformübergreifendes SDK zum Authentifizieren von Benutzern und Speichern Ihrer Konten. Es enthält OAuth-Authentifikatoren, die Unterstützung für die Nutzung von Identitäts Anbietern wie Google, Microsoft, Facebook und Twitter bieten. In diesem Artikel wird erläutert, wie Sie xamarin. auth verwenden, um den Authentifizierungsprozess in einer xamarin. Forms-Anwendung zu verwalten._

OAuth ist ein offener Standard für die Authentifizierung und ermöglicht einen Besitzer der Ressource um einem Ressourcenanbieter zu benachrichtigen, dass es sich bei Berechtigungen gewährt werden soll, an Dritte weiter um ihre Informationen zugreifen, ohne die Identität der Besitzer der Ressource freigeben zu müssen. Ein Beispiel dafür wäre die einen Benutzer zum Identitätsanbieter (z. B. Google, Microsoft, Facebook oder Twitter) benachrichtigen, dass die Berechtigung gewährt werden soll, um eine Anwendung auf ihre Daten zugreifen, ohne die Identität des Benutzers freigeben zu müssen aktiviert werden. Es wird häufig als Ansatz für Benutzer verwendet, Websites und Anwendungen, die mit einem Identitätsanbieter, ohne dass ihr Kennwort auf die Website oder Anwendung anmelden.

Eine allgemeine Übersicht über den Ablauf der Authentifizierung bei der Nutzung von OAuth-Identitätsanbieter lautet wie folgt aus:

1. Die Anwendung navigiert, einen Browser mit einer URL des Identitätsanbieters.
1. Der Identitätsanbieter Benutzerauthentifizierung verarbeitet und an die Anwendung einen Autorisierungscode zurück.
1. Die Anwendung tauscht den Autorisierungscode gegen ein Zugriffstoken vom Identitätsanbieter an.
1. Die Anwendung verwendet das Zugriffstoken für den Identitätsanbieter, z. B. eine API zum Anfordern von Daten mit basic-Benutzer den Zugriff auf APIs.

Die beispielanwendung veranschaulicht, wie Xamarin.Auth verwenden, um eine systemeigene Authentifizierungsablauf für Google zu implementieren. Während Sie Google als Identitätsanbieter in diesem Thema verwendet wird, ist der Ansatz mit anderen Identitätsanbietern gleichermaßen anwendbar. Weitere Informationen zur Authentifizierung mithilfe des OAuth 2,0-Endpunkts von Google finden [Sie unter Verwenden von OAuth 2.0](https://developers.google.com/identity/protocols/OAuth2) für den Zugriff auf Google-APIs auf der Google-Website.

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
> ATS kann deaktiviert werden, wenn es nicht möglich ist, das `HTTPS` Protokoll und die sichere Kommunikation für Internetressourcen zu verwenden. Dies kann durch Aktualisieren der Datei " **Info. plist** " der APP erreicht werden. Weitere Informationen finden Sie unter [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Verwenden von Xamarin.Auth zum Authentifizieren von Benutzern

Xamarin.Auth unterstützt zwei Ansätze, um Anwendungen für die Interaktion mit autorisierungsendpunkt des Identitätsanbieters:

1. Verwenden eine eingebettete Webansicht. Während dies üblicherweise wurde, wird nicht mehr aus den folgenden Gründen empfohlen:

    - Die Anwendung, die Webansicht hostet, kann die Anmeldeinformationen des Benutzers vollständige Authentifizierung, nicht nur die OAuth-autorisierungsgewährung zugreifen, die für die Anwendung bestimmt ist. Dies verstößt gegen das Prinzip der geringsten Rechte, wie die Anwendung Zugriff auf leistungsfähigere Anmeldeinformationen, die hat als potenziell erhöht die Angriffsfläche der Anwendung erfordert.
    - Die hostanwendung konnte erfassen, Benutzernamen und Kennwörter, automatisch Senden von Formularen und umgehen Zustimmung des Benutzers, und Sitzungscookies zu kopieren und verwenden, um authentifizierte Aktionen als Benutzer ausführen.
    - Eingebettete Webansichten freigeben nicht des Authentifizierungsstatus mit anderen Anwendungen oder Webbrowser des Geräts, dass der Benutzer sich anmelden für jede autorisierungsanforderung ein tiefgestelltes Bedienungserlebnis angesehen wird.
    - Einige Autorisierungs-Endpunkten Maßnahmen zum Erkennen und Blockieren von autorisierungsanforderungen, die von Webansichten stammen.

1. Mithilfe der Webbrowser des Geräts, die der empfohlene Ansatz ist. Mithilfe des Geräte-Browsers für die OAuth-Anforderungen verbessert die Verwendbarkeit einer Anwendung, wie die Benutzer müssen nur den betreffenden Identitätsanbieter anzeigt, einmal pro Gerät, Verbessern der konvertierungsraten-Anmeldung und Autorisierung von Flows in der Anwendung anmelden. Der Gerätebrowser bietet verbesserte Sicherheit auch, wie Anwendungen sind in der Lage, um zu überprüfen und Ändern von Inhalt in eine Webansicht, aber nicht im Browser angezeigten Inhalt. Dies ist der Ansatz in diesem Artikel und Beispiel-Anwendung.

Ein allgemeinen Überblick darüber, wie die beispielanwendung Xamarin.Auth zum Authentifizieren von Benutzern und ihren grundlegenden Datenabruf verwendet, wird im folgenden Diagramm dargestellt:

![](oauth-images/google-auth.png "Using Xamarin.Auth to Authenticate with Google")

Die Anwendung sendet mithilfe der `OAuth2Authenticator`-Klasse eine Authentifizierungsanforderung an Google. Eine Authentifizierungsantwort wird zurückgegeben, sobald der Benutzer erfolgreich mit Google über ihre Anmeldeseite authentifiziert hat ein Zugriffstoken enthält. Die Anwendung sendet dann eine Anforderung an Google für grundlegende Benutzerdaten, wobei die `OAuth2Request`-Klasse verwendet wird, wobei das Zugriffs Token in der Anforderung enthalten ist.

### <a name="setup"></a>Einrichten

Ein Projekt für die Google-API-Konsole muss erstellt werden, um Google-Anmeldung in einer Xamarin.Forms-Anwendung zu integrieren. Dies kann folgendermaßen erfüllt werden:

1. Wechseln Sie zur [Google API Console](https://console.developers.google.com) -Website, und melden Sie sich mit den Anmelde Informationen für Google-Konto an.
1. Wählen Sie aus den Projekt-Dropdown-Pfeil ein vorhandenes Projekt oder erstellen Sie eine neue.
1. Klicken Sie in der Rand Leiste unter "API-Manager" auf **Anmelde**Informationen, und wählen Sie dann die **Registerkarte OAuth-Zustimmungs Bildschirm** Wählen Sie eine **e-Mail-Adresse**aus, geben Sie einen **Produktnamen an, der Benutzern angezeigt**wird
1. Wählen Sie auf der Registerkarte **Anmelde** Informationen die Dropdown Liste **Anmelde Informationen erstellen** aus, und wählen Sie **OAuth-Client-ID**aus.
1. Wählen Sie unter **Anwendungstyp**die Plattform aus, auf der die Mobile Anwendung ausgeführt werden soll (**IOS** oder **Android**).
1. Geben Sie die erforderlichen Details ein, und klicken Sie auf die Schaltfläche **Erstellen** .

> [!NOTE]
> Eine Client-ID kann eine Anwendung, die Zugriff auf Google-APIs aktiviert, und für mobile Anwendungen auf einer einzigen Plattform eindeutig ist. Daher sollte eine **OAuth-Client-ID** für jede Plattform erstellt werden, die die Google-Anmeldung verwendet.

Nach der Durchführung dieser Schritte kann Xamarin.Auth zum Initiieren einer OAuth2-authentifizierungsfluss bei Google verwendet werden.

### <a name="creating-and-configuring-an-authenticator"></a>Erstellen und konfigurieren einen Authentifikator

Die `OAuth2Authenticator` Klasse von xamarin. auth ist für die Verarbeitung des OAuth-Authentifizierungs Flusses zuständig. Im folgenden Codebeispiel wird die Instanziierung der `OAuth2Authenticator`-Klasse beim Ausführen der Authentifizierung mit dem Webbrowser des Geräts veranschaulicht:

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

Die `OAuth2Authenticator`-Klasse benötigt eine Reihe von Parametern, die wie folgt lauten:

- **Client-ID** – Hiermit wird der Client identifiziert, von dem die Anforderung stammt, und er kann aus dem Projekt in der [Google API-Konsole](https://console.developers.google.com)abgerufen werden.
- **Geheimer Client** Schlüssel – dies sollte `null` oder `string.Empty`sein.
- **Bereich** – Hiermit wird der API-Zugriff identifiziert, der von der Anwendung angefordert wird, und der Zustimmungs Bildschirm, der dem Benutzer angezeigt wird, wird durch den Wert informiert. Weitere Informationen zu Bereichen finden Sie unter [Autorisieren von Anforderungen](https://developers.google.com/docs/api/how-tos/authorizing) auf der Website von Google.
- **Autorisierungs-URL** – Hiermit wird die URL identifiziert, von der der Autorisierungs Code abgerufen wird.
- **Umleitungs-URL** – Hiermit wird die URL identifiziert, an die die Antwort gesendet wird. Der Wert dieses Parameters muss mit einem der Werte, die auf der Registerkarte **Anmelde** Informationen für das Projekt in der [Google Developers Console](https://console.developers.google.com/)angezeigt werden, identisch sein.
- **Access** Token-URL – Hiermit wird die URL identifiziert, die zum Anfordern von Zugriffs Token verwendet wird, nachdem ein Autorisierungs Code abgerufen wurde.
- **Getusernameasync Func** – ein optionales `Func`, das zum asynchronen Abrufen des Benutzernamens des Kontos verwendet wird, nachdem es erfolgreich authentifiziert wurde.
- **Native Benutzeroberfläche verwenden** – ein `boolean` Wert, der angibt, ob der Webbrowser des Geräts verwendet werden soll, um die Authentifizierungsanforderung auszuführen.

### <a name="setup-authentication-event-handlers"></a>Ereignishandler für die Authentifizierung einrichten

Vor der Darstellung der Benutzeroberfläche muss ein Ereignishandler für das `OAuth2Authenticator.Completed`-Ereignis registriert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
authenticator.Completed += OnAuthCompleted;
```

Dieses Ereignis wird ausgelöst, wenn der Benutzer erfolgreich authentifiziert oder bricht die Anmeldung ab.

Optional kann auch ein Ereignishandler für das `OAuth2Authenticator.Error` Ereignis registriert werden.

### <a name="presenting-the-sign-in-user-interface"></a>Darstellen der Benutzeroberfläche-Anmeldung

Die Schnittstelle des angemeldeten Benutzers kann mit der eine Darstellung der Xamarin.Auth-Anmeldung, die in jedes plattformprojekt initialisiert werden muss, die dem Benutzer angezeigt. Im folgenden Codebeispiel wird gezeigt, wie ein Login Presenter in der `AppDelegate`-Klasse im IOS-Projekt initialisiert wird:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Im folgenden Codebeispiel wird gezeigt, wie ein Login Presenter in der `MainActivity`-Klasse im Android-Projekt initialisiert wird:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Die .NET Standard-Bibliotheksprojekts kann der Präsentator Anmeldung klicken Sie dann wie folgt aufrufen:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Beachten Sie, dass das Argument für die `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login`-Methode die `OAuth2Authenticator` Instanz ist. Wenn die `Login`-Methode aufgerufen wird, wird der Benutzer die Anmelde Benutzeroberfläche auf einer Registerkarte im Webbrowser des Geräts angezeigt. Dies wird in den folgenden Screenshots veranschaulicht:

![](oauth-images/login.png "Google Sign-In")

### <a name="processing-the-redirect-url"></a>Verarbeitet die Umleitungs-URL

Nachdem der Benutzer den Authentifizierungsprozess abgeschlossen hat, wird die Steuerung von der Registerkarte des Webbrowsers an die Anwendung zurückgegeben. Dies wird erreicht, indem ein benutzerdefiniertes URL-Schema für die Umleitungs-URL registriert wird, die vom Authentifizierungsprozess zurückgegeben wird, und dann die benutzerdefinierte URL nach dem Senden ermittelt und verarbeitet wird.

Bei der Auswahl eines benutzerdefinierten URL-Schemas, eine Anwendung zugeordnet werden soll, müssen Anwendungen ein Schema basierend auf einem an ihre jeweiligen verwenden. Dies kann erreicht werden, indem Sie den Namen der Bundle-Bezeichner unter iOS und den Paketnamen unter Android und umkehren, um das URL-Schema stellen Sie dann. Allerdings weisen einige Identitätsanbietern wie Google, Client-IDs, die basierend auf den Domänennamen, die dann rückgängig gemacht und als URL-Schema verwendet werden. Wenn Google z. b. eine Client-ID `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`erstellt, wird das URL-Schema `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Beachten Sie, dass nach der Schema Komponente nur ein einziger `/` angezeigt werden kann. Daher ist ein umfassendes Beispiel für eine Umleitungs-URL, die ein benutzerdefiniertes URL-Schema nutzt, `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Wenn der Webbrowser eine Antwort vom Identitätsanbieter, die ein benutzerdefiniertes URL-Schema enthält erhält, versucht, die URL, zu laden, was die fehl. Stattdessen wird die benutzerdefinierte URL-Schema für das Betriebssystem gemeldet, durch Auslösen eines Ereignisses. Das Betriebssystem prüft dann, ob registrierte Schemas, und falls vorhanden, das Betriebssystem wird starten Sie die Anwendung, die das Schema registriert, und die umleitungs-URL senden.

Der Mechanismus zum Registrieren eines benutzerdefinierten URL-Schemas mit dem Betriebssystem, und behandeln das Schema ist für jede Plattform spezifisch.

#### <a name="ios"></a>iOS

Unter IOS wird ein benutzerdefiniertes URL-Schema in " **Info. plist**" registriert, wie im folgenden Screenshot zu sehen:

![](oauth-images/info-plist.png "URL Scheme Registration")

Der **Bezeichnerwert** kann beliebiger Wert sein, und der **Rollen** Wert muss auf **Viewer**festgelegt sein. Der Wert der **URL-Schemas** , der mit `com.googleusercontent.apps`beginnt, kann von der IOS-Client-ID für das Projekt in der [Google API-Konsole](https://console.developers.google.com)abgerufen werden.

Wenn der Identitätsanbieter die autorisierungsanforderung abgeschlossen ist, erfolgt eine Umleitung an den umleitungs-URL der Anwendung. Da die URL ein benutzerdefiniertes Schema verwendet, führt dies dazu, dass IOS die Anwendung startet und die URL als Startparameter übergibt, wo Sie von der `OpenUrl` Überschreibung der `AppDelegate` Klasse der Anwendung verarbeitet wird, die im folgenden Codebeispiel gezeigt wird:

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

Die `OpenUrl`-Methode konvertiert die empfangene URL von einer `NSUrl` in eine .net-`Uri`, bevor die Umleitungs-URL mit der `OnPageLoading`-Methode eines öffentlichen `OAuth2Authenticator` Objekts verarbeitet wird. Dies bewirkt, dass Xamarin.Auth auf die Registerkarte des Webbrowsers zu schließen, und klicken Sie zum Analysieren der empfangenen OAuth-Daten.

#### <a name="android"></a>Android

Unter Android wird ein benutzerdefiniertes URL-Schema durch Angeben eines [`IntentFilter`](xref:Android.App.IntentFilterAttribute) -Attributs auf dem `Activity` registriert, das das Schema behandelt. Wenn der Identitätsanbieter die autorisierungsanforderung abgeschlossen ist, erfolgt eine Umleitung an den umleitungs-URL der Anwendung. Wenn die URL ein benutzerdefiniertes Schema verwendet, führt dies dazu, dass Android die Anwendung startet und die URL als Startparameter übergibt, wo Sie von der `OnCreate`-Methode des `Activity` verarbeitet wird, das für die Handhabung des benutzerdefinierten URL-Schemas registriert ist. Das folgende Codebeispiel zeigt die Klasse aus der beispielanwendung, die die benutzerdefinierte URL-Schema behandelt:

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

Die `DataSchemes`-Eigenschaft des [`IntentFilter`](xref:Android.App.IntentFilterAttribute) muss auf den umgekehrten Client Bezeichner festgelegt werden, der von der Android-Client-ID für das Projekt in der [Google API-Konsole](https://console.developers.google.com)abgerufen wird.

Die `OnCreate`-Methode konvertiert die empfangene URL von einer `Android.Net.Url` in eine .net-`Uri`, bevor die Umleitungs-URL mit der `OnPageLoading`-Methode eines öffentlichen `OAuth2Authenticator` Objekts verarbeitet wird. Dies bewirkt, dass Xamarin.Auth zum Schließen der Registerkarte des Webbrowsers, und analysieren Sie die empfangenen OAuth-Daten.

> [!IMPORTANT]
> Unter Android verwendet xamarin. auth die `CustomTabs`-API für die Kommunikation mit dem Webbrowser und dem Betriebssystem. Es ist jedoch nicht sichergestellt, dass ein `CustomTabs` kompatibler Browser auf dem Gerät des Benutzers installiert wird.

### <a name="examining-the-oauth-response"></a>Untersuchen die OAuth-Antwort

Nach dem Auswerten der empfangenen OAuth-Daten löst xamarin. auth das `OAuth2Authenticator.Completed`-Ereignis aus. Im Ereignishandler für dieses Ereignis kann die `AuthenticatorCompletedEventArgs.IsAuthenticated`-Eigenschaft verwendet werden, um zu ermitteln, ob die Authentifizierung erfolgreich war, wie im folgenden Codebeispiel gezeigt:

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

Die Daten, die von einer erfolgreichen Authentifizierung gesammelt werden, sind in der `AuthenticatorCompletedEventArgs.Account`-Eigenschaft verfügbar. Dies schließt ein Zugriffstoken, die zum Signieren von Anforderungen für Daten an eine API bereitgestellt, die vom Identitätsanbieter verwendet werden kann.

### <a name="making-requests-for-data"></a>Anfragen für Daten

Nachdem die Anwendung ein Zugriffs Token abgerufen hat, wird Sie verwendet, um eine Anforderung an die `https://www.googleapis.com/oauth2/v2/userinfo`-API zu senden, um grundlegende Benutzerdaten vom Identitäts Anbieter anzufordern. Diese Anforderung wird mit der `OAuth2Request`-Klasse von xamarin. auth hergestellt, die eine Anforderung darstellt, die mithilfe eines Kontos authentifiziert wird, das von einer `OAuth2Authenticator` Instanz abgerufen wird, wie im folgenden Codebeispiel gezeigt:

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

Ebenso wie die HTTP-Methode und die API-URL gibt die `OAuth2Request` Instanz auch eine `Account` Instanz an, die das Zugriffs Token enthält, mit dem die Anforderung an die von der `Constants.UserInfoUrl`-Eigenschaft angegebene URL signiert wird. Der Identitätsanbieter zurückgegeben grundlegende Benutzerdaten als eine JSON-Antwort, einschließlich der Benutzer und e-Mail-Adresse, vorausgesetzt, dass das Zugriffstoken als gültig erkennt. Die JSON-Antwort wird dann gelesen und in die `user` Variable deserialisiert.

Weitere Informationen finden Sie unter [Aufrufen einer Google-API](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) im Google-Entwickler Portal.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Speichern und Abrufen von Informationen auf Geräten

Xamarin. auth speichert `Account` Objekte sicher in einem Konto Speicher, damit Anwendungen nicht immer die Benutzer erneut authentifizieren müssen. Die `AccountStore`-Klasse ist für die Speicherung von Kontoinformationen zuständig und wird durch Keychain-Dienste in IOS und die `KeyStore`-Klasse in Android unterstützt.

> [!IMPORTANT]
> Die `AccountStore`-Klasse in xamarin. auth ist veraltet, und stattdessen sollte die xamarin. Essentials-`SecureStorage` Klasse verwendet werden. Weitere Informationen finden Sie unter [Migrieren von accountstore zu xamarin. Essentials securestorage](https://github.com/xamarin/Xamarin.Auth/wiki/Migrating-from-AccountStore-to-Xamarin.Essentials-SecureStorage).

Im folgenden Codebeispiel wird gezeigt, wie ein `Account`-Objekt sicher gespeichert wird:

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Gespeicherte Konten werden mithilfe eines Schlüssels eindeutig identifiziert, der aus der `Username`-Eigenschaft des Kontos und einer Dienst-ID besteht. Hierbei handelt es sich um eine Zeichenfolge, die beim Abrufen von Konten aus dem Konto Speicher verwendet wird. Wenn eine `Account` zuvor gespeichert wurde, wird Sie durch das erneute Aufrufen der `Save` Methode überschrieben.

`Account` Objekte für einen Dienst können durch Aufrufen der `FindAccountsForService`-Methode abgerufen werden, wie im folgenden Codebeispiel gezeigt:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

Die `FindAccountsForService`-Methode gibt eine `IEnumerable` Auflistung von `Account` Objekten zurück, wobei das erste Element in der Auflistung als übereinstimmendes Konto festgelegt wird.

## <a name="troubleshooting"></a>Problembehandlung

- Wenn Sie unter Android eine Popup Benachrichtigung erhalten, wenn Sie den Browser nach der Authentifizierung schließen und die Popup Benachrichtigung beenden möchten, fügen Sie dem Android-Projekt nach dem Initialisieren von xamarin. auth den folgenden Code hinzu:

```csharp
Xamarin.Auth.CustomTabsConfiguration.CustomTabsClosingMessage = null;
```

- Wenn der Browser unter Android nicht automatisch geschlossen wird, besteht eine vorübergehende Problem Umgehung darin, das xamarin. auth-Paket auf Version 1.5.0.3 herabzusetzen. Fügen Sie dann dem Android-Projekt die [PCL-kryptografiev-2.0.147](https://www.nuget.org/packages/PCLCrypto/2.0.147) hinzu.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Xamarin.Auth zu verwenden, um die Verwaltung des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung ermöglichen. Xamarin. auth stellt die `OAuth2Authenticator`-und `OAuth2Request` Klassen bereit, die von xamarin. Forms-Anwendungen zum Nutzen von Identitäts Anbietern wie Google, Microsoft, Facebook und Twitter verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Oauthnativeflow (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)
- [OAuth 2,0 für Native apps](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Verwenden von OAuth 2.0 für den Zugriff auf Google-APIs](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin. auth (nuget)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin. auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
