---
title: Authentifialisierer mit Identitäts Anbieter
description: In diesem Artikel wird erläutert, wie Sie xamarin. auth verwenden, um den Authentifizierungsprozess in einer xamarin. Forms-Anwendung zu verwalten.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2019
ms.openlocfilehash: 83fbad8a9bbb9afef5ee80705fe9e86e51284e7d
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842985"
---
# <a name="authenticate-users-with-an-identity-provider"></a>Authentifizieren von Benutzern mit einem Identitäts Anbieter

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)

_Xamarin. auth ist ein plattformübergreifendes SDK zum Authentifizieren von Benutzern und Speichern Ihrer Konten. Es enthält OAuth-Authentifikatoren, die Unterstützung für die Nutzung von Identitäts Anbietern wie Google, Microsoft, Facebook und Twitter bieten. In diesem Artikel wird erläutert, wie Sie xamarin. auth verwenden, um den Authentifizierungsprozess in einer xamarin. Forms-Anwendung zu verwalten._

OAuth ist ein offener Standard für die Authentifizierung und ermöglicht es einem Ressourcenanbieter, einen Ressourcenanbieter zu benachrichtigen, dass die Berechtigung für einen Drittanbieter erteilt werden muss, um auf seine Informationen zuzugreifen, ohne die Identität des Ressourcen Besitzers freigeben zu müssen. Ein Beispiel hierfür wäre, dass ein Benutzer einen Identitäts Anbieter (z. b. Google, Microsoft, Facebook oder Twitter) Benachrichtigen muss, dass eine Berechtigung einer Anwendung erteilt werden muss, um auf Ihre Daten zuzugreifen, ohne die Identität des Benutzers zu teilen. Sie wird häufig als Ansatz verwendet, mit dem sich Benutzer bei Websites und Anwendungen mithilfe eines Identitäts Anbieters anmelden können, ohne Ihr Kennwort für die Website oder Anwendung verfügbar zu machen.

Eine allgemeine Übersicht über den Authentifizierungs Ablauf bei der Verwendung eines OAuth-Identitäts Anbieters lautet wie folgt:

1. Die Anwendung navigiert einen Browser zu einer Identitäts Anbieter-URL.
1. Der Identitäts Anbieter verarbeitet die Benutzerauthentifizierung und gibt einen Autorisierungs Code an die Anwendung zurück.
1. Die Anwendung tauscht den Autorisierungs Code für ein Zugriffs Token vom Identitäts Anbieter aus.
1. Die Anwendung verwendet das Zugriffs Token für den Zugriff auf APIs des Identitäts Anbieters, z. b. eine API zum Anfordern grundlegender Benutzerdaten.

Die Beispielanwendung veranschaulicht die Verwendung von xamarin. auth zum Implementieren eines nativen Authentifizierungs Flows für Google. Obwohl Google als Identitäts Anbieter in diesem Thema verwendet wird, ist der Ansatz gleichermaßen auf andere Identitäts Anbieter anwendbar. Weitere Informationen zur Authentifizierung mithilfe des OAuth 2,0-Endpunkts von Google finden [Sie unter Verwenden von OAuth 2.0](https://developers.google.com/identity/protocols/OAuth2) für den Zugriff auf Google-APIs auf der Google-Website.

> [!NOTE]
> In ios 9 und höher erzwingt App-Transport Sicherheit (app Transport Security, ATS) sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und der APP, wodurch eine versehentliche Offenlegung vertraulicher Informationen verhindert wird. Da ATS in apps, die für IOS 9 erstellt wurden, standardmäßig aktiviert ist, unterliegen alle Verbindungen den Sicherheitsanforderungen. Wenn Verbindungen diese Anforderungen nicht erfüllen, können Sie mit einer Ausnahme fehlschlagen.
> ATS kann deaktiviert werden, wenn es nicht möglich ist, das `HTTPS` Protokoll und die sichere Kommunikation für Internetressourcen zu verwenden. Dies kann durch Aktualisieren der Datei " **Info. plist** " der APP erreicht werden. Weitere Informationen finden Sie unter [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Verwenden von xamarin. auth zum Authentifizieren von Benutzern

Xamarin. auth unterstützt zwei Ansätze, mit denen Anwendungen mit dem Autorisierungs Endpunkt eines Identitäts Anbieters interagieren können:

1. Verwenden einer eingebetteten Webansicht. Obwohl dies eine gängige Vorgehensweise ist, wird Sie aus den folgenden Gründen nicht mehr empfohlen:

    - Die Anwendung, die die Webansicht hostet, kann auf die vollständigen Authentifizierungs Anmelde Informationen des Benutzers zugreifen, nicht nur auf die OAuth-Autorisierungs Gewährung, die für die Anwendung vorgesehen war. Dies verstößt gegen das Prinzip der geringsten Rechte, da die Anwendung Zugriff auf leistungsfähigere Anmelde Informationen hat, als Sie benötigt, und die Angriffsfläche der Anwendung potenziell erhöhen kann.
    - Die Host Anwendung kann Benutzernamen und Kenn Wörter erfassen, Formulare automatisch übermitteln und die Benutzer Zustimmung übermitteln und Sitzungs Cookies kopieren und verwenden, um authentifizierte Aktionen als Benutzer auszuführen.
    - Eingebettete Webansichten verwenden den Authentifizierungs Status nicht für andere Anwendungen oder den Webbrowser des Geräts. Dies erfordert, dass sich der Benutzer für jede Autorisierungs Anforderung anmeldet, die als eine geringere Benutzer Darstellung angesehen wird.
    - Einige Autorisierungs Endpunkte führen Maßnahmen zum erkennen und Blockieren von Autorisierungs Anforderungen aus, die aus Webansichten stammen.

1. Der empfohlene Ansatz ist die Verwendung des Webbrowsers des Geräts. Die Verwendung des Geräte Browsers für OAuth-Anforderungen verbessert die Benutzerfreundlichkeit einer Anwendung, da Benutzer sich nur einmal pro Gerät beim Identitäts Anbieter anmelden müssen, um die Konvertierungsraten für Anmelde-und Autorisierungs Abläufe in der Anwendung zu verbessern. Der Geräte Browser bietet auch eine verbesserte Sicherheit, da Anwendungen Inhalte in einer Webansicht überprüfen und ändern können, während der Inhalt im Browser nicht angezeigt wird. Dies ist der Ansatz, der in diesem Artikel und in der Beispielanwendung verwendet wird.

Eine allgemeine Übersicht über die Verwendung von xamarin. auth durch die Beispielanwendung zum Authentifizieren von Benutzern und zum Abrufen ihrer grundlegenden Daten finden Sie in der folgenden Abbildung:

![](oauth-images/google-auth.png "Using Xamarin.Auth to Authenticate with Google")

Die Anwendung sendet mithilfe der `OAuth2Authenticator`-Klasse eine Authentifizierungsanforderung an Google. Eine Authentifizierungs Antwort wird zurückgegeben, sobald sich der Benutzer über die Anmeldeseite, die ein Zugriffs Token enthält, erfolgreich bei Google authentifiziert hat. Die Anwendung sendet dann eine Anforderung an Google für grundlegende Benutzerdaten, wobei die `OAuth2Request`-Klasse verwendet wird, wobei das Zugriffs Token in der Anforderung enthalten ist.

### <a name="setup"></a>Setup

Ein Google API-Konsolen Projekt muss erstellt werden, um die Google-Anmeldung in eine xamarin. Forms-Anwendung zu integrieren. Dies kann folgendermaßen erfüllt werden:

1. Wechseln Sie zur [Google API Console](https://console.developers.google.com) -Website, und melden Sie sich mit den Anmelde Informationen für Google-Konto an.
1. Wählen Sie in der Dropdown-Dropdown-Option ein vorhandenes Projekt aus, oder erstellen Sie ein neues Projekt.
1. Klicken Sie in der Rand Leiste unter "API-Manager" auf **Anmelde**Informationen, und wählen Sie dann die **Registerkarte OAuth-Zustimmungs Bildschirm** Wählen Sie eine **e-Mail-Adresse**aus, geben Sie einen **Produktnamen an, der Benutzern angezeigt**wird
1. Wählen Sie auf der Registerkarte **Anmelde** Informationen die Dropdown Liste **Anmelde Informationen erstellen** aus, und wählen Sie **OAuth-Client-ID**aus.
1. Wählen Sie unter **Anwendungstyp**die Plattform aus, auf der die Mobile Anwendung ausgeführt werden soll (**IOS** oder **Android**).
1. Geben Sie die erforderlichen Details ein, und klicken Sie auf die Schaltfläche **Erstellen** .

> [!NOTE]
> Eine Client-ID ermöglicht einem Anwendungs Zugriff das Aktivieren von Google-APIs, und für mobile Anwendungen ist eine einzigartige Plattform. Daher sollte eine **OAuth-Client-ID** für jede Plattform erstellt werden, die die Google-Anmeldung verwendet.

Nachdem Sie diese Schritte ausgeführt haben, kann xamarin. auth verwendet werden, um einen OAuth2-Authentifizierungs Fluss mit Google zu initiieren.

### <a name="creating-and-configuring-an-authenticator"></a>Erstellen und Konfigurieren eines Authentifikators

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
- **Bereich** – Hiermit wird der API-Zugriff identifiziert, der von der Anwendung angefordert wird, und der Zustimmungs Bildschirm, der dem Benutzer angezeigt wird, wird durch den Wert informiert. Weitere Informationen zu Bereichen finden Sie unter [Autorisierungs-API-Anforderung](https://developers.google.com/+/web/api/rest/oauth) auf der Google-Website.
- **Autorisierungs-URL** – Hiermit wird die URL identifiziert, von der der Autorisierungs Code abgerufen wird.
- **Umleitungs-URL** – Hiermit wird die URL identifiziert, an die die Antwort gesendet wird. Der Wert dieses Parameters muss mit einem der Werte, die auf der Registerkarte **Anmelde** Informationen für das Projekt in der [Google Developers Console](https://console.developers.google.com/)angezeigt werden, identisch sein.
- **Access** Token-URL – Hiermit wird die URL identifiziert, die zum Anfordern von Zugriffs Token verwendet wird, nachdem ein Autorisierungs Code abgerufen wurde.
- **Getusernameasync Func** – ein optionales `Func`, das zum asynchronen Abrufen des Benutzernamens des Kontos verwendet wird, nachdem es erfolgreich authentifiziert wurde.
- **Native Benutzeroberfläche verwenden** – ein `boolean` Wert, der angibt, ob der Webbrowser des Geräts verwendet werden soll, um die Authentifizierungsanforderung auszuführen.

### <a name="setup-authentication-event-handlers"></a>Einrichten von Authentifizierungs Ereignis Handlern

Vor der Darstellung der Benutzeroberfläche muss ein Ereignishandler für das `OAuth2Authenticator.Completed`-Ereignis registriert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
authenticator.Completed += OnAuthCompleted;
```

Dieses Ereignis wird ausgelöst, wenn der Benutzer die Anmeldung erfolgreich authentifiziert oder abgebrochen hat.

Optional kann auch ein Ereignishandler für das `OAuth2Authenticator.Error` Ereignis registriert werden.

### <a name="presenting-the-sign-in-user-interface"></a>Darstellung der Anmelde Benutzeroberfläche

Die Anmelde Benutzeroberfläche kann dem Benutzer mithilfe einer xamarin. auth Login Presenter angezeigt werden, die in jedem Platt Form Projekt initialisiert werden muss. Im folgenden Codebeispiel wird gezeigt, wie ein Login Presenter in der `AppDelegate`-Klasse im IOS-Projekt initialisiert wird:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Im folgenden Codebeispiel wird gezeigt, wie ein Login Presenter in der `MainActivity`-Klasse im Android-Projekt initialisiert wird:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Das .NET Standard Bibliotheksprojekt kann dann den Login Presenter wie folgt aufrufen:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Beachten Sie, dass das Argument für die `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login`-Methode die `OAuth2Authenticator` Instanz ist. Wenn die `Login`-Methode aufgerufen wird, wird der Benutzer die Anmelde Benutzeroberfläche auf einer Registerkarte im Webbrowser des Geräts angezeigt. Dies wird in den folgenden Screenshots veranschaulicht:

![](oauth-images/login.png "Google Sign-In")

### <a name="processing-the-redirect-url"></a>Verarbeiten der Umleitungs-URL

Nachdem der Benutzer den Authentifizierungsprozess abgeschlossen hat, wird die Steuerung von der Registerkarte des Webbrowsers an die Anwendung zurückgegeben. Dies wird erreicht, indem ein benutzerdefiniertes URL-Schema für die Umleitungs-URL registriert wird, die vom Authentifizierungsprozess zurückgegeben wird, und dann die benutzerdefinierte URL nach dem Senden ermittelt und verarbeitet wird.

Wenn Sie ein benutzerdefiniertes URL-Schema auswählen, das einer Anwendung zugeordnet werden soll, müssen Anwendungen ein Schema verwenden, das auf einem Namen unter dem Steuerelement basiert. Dies kann erreicht werden, indem der paketbezeichnername unter IOS und der Paketname unter Android verwendet und diese dann umgekehrt werden, um das URL-Schema zu erstellen. Einige Identitäts Anbieter, wie z. b. Google, weisen jedoch Client Bezeichner auf der Grundlage von Domänen Namen zu, die dann umgekehrt und als URL-Schema verwendet werden. Wenn Google z. b. eine Client-ID `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`erstellt, wird das URL-Schema `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Beachten Sie, dass nach der Schema Komponente nur ein einziger `/` angezeigt werden kann. Daher ist ein umfassendes Beispiel für eine Umleitungs-URL, die ein benutzerdefiniertes URL-Schema nutzt, `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Wenn der Webbrowser eine Antwort vom Identitäts Anbieter empfängt, der ein benutzerdefiniertes URL-Schema enthält, wird versucht, die URL zu laden. Dies schlägt fehl. Stattdessen wird das benutzerdefinierte URL-Schema an das Betriebssystem gemeldet, indem ein Ereignis ausgegeben wird. Das Betriebssystem prüft dann, ob registrierte Schemas vorhanden sind. Wenn eine solche gefunden wird, startet das Betriebssystem die Anwendung, die das Schema registriert hat, und sendet Sie die Umleitungs-URL.

Der Mechanismus, mit dem ein benutzerdefiniertes URL-Schema beim Betriebssystem registriert und das Schema behandelt wird, ist spezifisch für jede Plattform.

#### <a name="ios"></a>iOS

Unter IOS wird ein benutzerdefiniertes URL-Schema in " **Info. plist**" registriert, wie im folgenden Screenshot zu sehen:

![](oauth-images/info-plist.png "URL Scheme Registration")

Der **Bezeichnerwert** kann beliebiger Wert sein, und der **Rollen** Wert muss auf **Viewer**festgelegt sein. Der Wert der **URL-Schemas** , der mit `com.googleusercontent.apps`beginnt, kann von der IOS-Client-ID für das Projekt in der [Google API-Konsole](https://console.developers.google.com)abgerufen werden.

Wenn der Identitäts Anbieter die Autorisierungs Anforderung abschließt, wird er an die Umleitungs-URL der Anwendung umgeleitet. Da die URL ein benutzerdefiniertes Schema verwendet, führt dies dazu, dass IOS die Anwendung startet und die URL als Startparameter übergibt, wo Sie von der `OpenUrl` Überschreibung der `AppDelegate` Klasse der Anwendung verarbeitet wird, die im folgenden Codebeispiel gezeigt wird. :

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

Die `OpenUrl`-Methode konvertiert die empfangene URL von einer `NSUrl` in eine .net-`Uri`, bevor die Umleitungs-URL mit der `OnPageLoading`-Methode eines öffentlichen `OAuth2Authenticator` Objekts verarbeitet wird. Dies bewirkt, dass xamarin. auth die Registerkarte Webbrowser schließt und die empfangenen OAuth-Daten analysiert.

#### <a name="android"></a>Android

Unter Android wird ein benutzerdefiniertes URL-Schema durch Angeben eines [`IntentFilter`](xref:Android.App.IntentFilterAttribute) -Attributs auf dem `Activity` registriert, das das Schema behandelt. Wenn der Identitäts Anbieter die Autorisierungs Anforderung abschließt, wird er an die Umleitungs-URL der Anwendung umgeleitet. Wenn die URL ein benutzerdefiniertes Schema verwendet, führt dies dazu, dass Android die Anwendung startet und die URL als Startparameter übergibt, wo Sie von der `OnCreate`-Methode des `Activity` verarbeitet wird, das für die Handhabung des benutzerdefinierten URL-Schemas registriert ist. Das folgende Codebeispiel zeigt die-Klasse aus der Beispielanwendung, die das benutzerdefinierte URL-Schema behandelt:

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

Die `OnCreate`-Methode konvertiert die empfangene URL von einer `Android.Net.Url` in eine .net-`Uri`, bevor die Umleitungs-URL mit der `OnPageLoading`-Methode eines öffentlichen `OAuth2Authenticator` Objekts verarbeitet wird. Dies bewirkt, dass xamarin. auth die Registerkarte Webbrowser schließt und die empfangenen OAuth-Daten analysiert.

> [!IMPORTANT]
> Unter Android verwendet xamarin. auth die `CustomTabs`-API für die Kommunikation mit dem Webbrowser und dem Betriebssystem. Es ist jedoch nicht sichergestellt, dass ein `CustomTabs` kompatibler Browser auf dem Gerät des Benutzers installiert wird.

### <a name="examining-the-oauth-response"></a>Untersuchen der OAuth-Antwort

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

Die Daten, die von einer erfolgreichen Authentifizierung gesammelt werden, sind in der `AuthenticatorCompletedEventArgs.Account`-Eigenschaft verfügbar. Dies schließt ein Zugriffs Token ein, das zum Signieren von Datenanforderungen an eine vom Identitäts Anbieter bereitgestellte API verwendet werden kann.

### <a name="making-requests-for-data"></a>Anfordern von Datenanforderungen

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

Ebenso wie die HTTP-Methode und die API-URL gibt die `OAuth2Request` Instanz auch eine `Account` Instanz an, die das Zugriffs Token enthält, mit dem die Anforderung an die von der `Constants.UserInfoUrl`-Eigenschaft angegebene URL signiert wird. Der Identitäts Anbieter gibt dann grundlegende Benutzerdaten als JSON-Antwort, einschließlich des Namens und der e-Mail-Adresse des Benutzers, zurück, vorausgesetzt, dass das Zugriffs Token als gültig erkannt wird. Die JSON-Antwort wird dann gelesen und in die `user` Variable deserialisiert.

Weitere Informationen finden Sie unter [Aufrufen einer Google-API](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) im Google-Entwickler Portal.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Speichern und Abrufen von Kontoinformationen auf Geräten

Xamarin. auth speichert `Account` Objekte sicher in einem Konto Speicher, damit Anwendungen nicht immer die Benutzer erneut authentifizieren müssen. Die `AccountStore`-Klasse ist für die Speicherung von Kontoinformationen zuständig und wird durch Keychain-Dienste in IOS und die `KeyStore`-Klasse in Android unterstützt.

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

In diesem Artikel wurde erläutert, wie Sie xamarin. auth verwenden, um den Authentifizierungsprozess in einer xamarin. Forms-Anwendung zu verwalten. Xamarin. auth stellt die `OAuth2Authenticator`-und `OAuth2Request` Klassen bereit, die von xamarin. Forms-Anwendungen zum Nutzen von Identitäts Anbietern wie Google, Microsoft, Facebook und Twitter verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [Oauthnativeflow (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-oauthnativeflow)
- [OAuth 2,0 für Native apps](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Verwenden von OAuth 2.0 für den Zugriff auf Google-APIs](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin. auth (nuget)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin. auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
