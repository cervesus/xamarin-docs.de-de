---
title: Authentifizieren von Benutzern mit einem Identitätsanbieter
description: Xamarin.Auth ist eine plattformübergreifende-SDK für die Authentifizierung von Benutzern und speichern ihre Konten. Es umfasst OAuth-Authentifikatoren, die Unterstützung für die Nutzung der Identitätsanbieter z. B. Google, Microsoft, Facebook und Twitter. In diesem Artikel erläutert die Xamarin.Auth verwenden, um den Authentifizierungsvorgang in einer Xamarin.Forms-Anwendung zu verwalten.
ms.prod: xamarin
ms.assetid: D44745D5-77BB-4596-9B8C-EC75C259157C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/19/2017
ms.openlocfilehash: 26e85a37cfd36b5d4f045273548efafccca79e1a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="authenticating-users-with-an-identity-provider"></a>Authentifizieren von Benutzern mit einem Identitätsanbieter

_Xamarin.Auth ist eine plattformübergreifende-SDK für die Authentifizierung von Benutzern und speichern ihre Konten. Es umfasst OAuth-Authentifikatoren, die Unterstützung für die Nutzung der Identitätsanbieter z. B. Google, Microsoft, Facebook und Twitter. In diesem Artikel erläutert die Xamarin.Auth verwenden, um den Authentifizierungsvorgang in einer Xamarin.Forms-Anwendung zu verwalten._

OAuth ist ein offener Standard für die Authentifizierung und ermöglicht einen Besitzer der Ressource um einem Ressourcenanbieter zu benachrichtigen, dass die Berechtigung erteilt werden sollte, um eine dritte Partei, um ihre Informationen zugreifen, ohne die Identität der Besitzer der Ressource freigeben. Ein Beispiel hierfür wäre zur Aktivierung eines Benutzers einem Identitätsanbieter (z. B. Google, Microsoft, Facebook oder Twitter) benachrichtigt, dass eine Anwendung auf ihre Daten zugreifen, ohne Freigabe der Identität des Benutzers Berechtigung gewährt werden soll. Es wird häufig als ein Ansatz für Benutzer verwendet, Websites und Anwendungen, die mit einem Identitätsanbieter, ohne dass ihr Kennwort auf der Website oder Anwendung anmelden.

Eine allgemeine Übersicht über den Authentifizierungsablauf beim Verarbeiten eines OAuth-Identitätsanbieter lautet wie folgt:

1. Die Anwendung navigiert, einen Browser mit einem Identitätsanbieter-URL.
1. Der Identitätsanbieter Benutzerauthentifizierung behandelt und gibt einen Autorisierungscode an die Anwendung zurück.
1. Die Anwendung tauscht den Autorisierungscode gegen ein Zugriffstoken vom Identitätsanbieter an.
1. Die Anwendung verwendet das Zugriffstoken zum Zugriff auf APIs in den Identitätsanbieter, z. B. eine API für grundlegende Daten angefordert werden.

Die beispielanwendung veranschaulicht, wie Xamarin.Auth verwenden, um einen einheitlichen Authentifizierungsablauf für Google zu implementieren. Während Sie Google als Identitätsanbieter in diesem Thema verwendet wird, ist der Ansatz gleichermaßen anwendbar für anderer Identitätsanbieter entgegen. Weitere Informationen zur Authentifizierung mithilfe von Google OAuth 2.0-Endpunkts finden Sie unter [OAuth2.0 verwenden, um Google-APIs zugreifen](https://developers.google.com/identity/protocols/OAuth2) Google-Website.

> [!NOTE]
> In iOS 9 und höher erzwingt App-Transport-Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern. Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.
> ATS können von hinzugewählt werden, ist es nicht möglich ist, verwenden Sie die `HTTPS` -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).

## <a name="using-xamarinauth-to-authenticate-users"></a>Verwenden von Xamarin.Auth zum Authentifizieren von Benutzern

Xamarin.Auth unterstützt zwei Ansätze für Anwendungen für die Interaktion mit einem Identitätsanbieter-autorisierungsendpunkt an:

1. Verwenden eine eingebettete Webansicht. Während dies üblich wurde, wird nicht mehr aus den folgenden Gründen empfohlen:

    - Die Anwendung, die der Webseitenansicht hostet, kann Anmeldeinformationen für den vollständigen Authentifizierung des Benutzers, nicht nur das OAuth Authorization Grant zugreifen, die für die Anwendung bestimmt wurde. Dies verstößt gegen das Prinzip der geringsten Rechte, wie die Anwendung Zugriff auf leistungsfähigeren Anmeldeinformationen hat als potenziell erhöht die Angriffsfläche der Anwendung erfordert.
    - Die hostanwendung konnte erfassen, Benutzernamen und Kennwörter, automatisch Formulare und benutzerzustimmung, umgehen und Sitzungscookies kopieren und verwenden, um authentifizierte Aktionen als Benutzer ausführen.
    - Eingebettete Webansichten freigeben nicht des Authentifizierungsstatus mit anderen Anwendungen oder Webbrowser des Geräts, da der Benutzer für jede autorisierungsanforderung anmelden, die ein kleiner als benutzererfahrung betrachtet wird.
    - Einige Autorisierungs-Endpunkten Schritte zu erkennen und zu blockieren Autorisierung-Anforderungen, die von Webansichten stammen.

1. Das Gerät Webbrowser verwenden, der die empfohlene Vorgehensweise ist. Die Verwendbarkeit einer Anwendung mit dem Gerätebrowser für OAuth-Anforderungen verbessert werden, da die Benutzer müssen nur zur Anmeldung an den Identitätsanbieter einmal pro Gerät Wechselkurse der Anmeldung und Autorisierung Datenflüsse in der Anwendung zu verbessern. Die Gerätebrowser bietet verbesserte Sicherheit auch, wie Anwendungen, um zu überprüfen und Ändern von Inhalt in einer Webansicht, aber nicht die im Browser angezeigte Inhalt können. Dies ist der Ansatz in diesem Artikel und Beispiel-Anwendung.

Ein allgemeinen Überblick darüber, wie die beispielanwendung Xamarin.Auth verwendet, um Benutzer zu authentifizieren und ihre grundlegenden Daten abzurufen, ist in der folgenden Abbildung dargestellt:

![](oauth-images/google-auth.png "Verwenden für die Authentifizierung bei Google Xamarin.Auth")

Die Anwendung fordert eine Authentifizierung mit Google die `OAuth2Authenticator` Klasse. Eine Authentifizierungsantwort wird zurückgegeben, nachdem der Benutzer erfolgreich mit Google über ihre Anmeldeseite authentifiziert hat ein Zugriffstoken enthält. Klicken Sie dann die Anwendung sendet eine Anfrage an Google für grundlegende Daten mithilfe der `OAuth2Request` -Klasse, mit dem Zugriffstoken wird in der Anforderung enthalten.

### <a name="setup"></a>Setup

Ein Projekt für die Google-API-Konsole muss erstellt werden, um Google-Anmeldung mit einer Xamarin.Forms-Anwendung zu integrieren. Dies kann folgendermaßen erfüllt werden:

1. Wechseln Sie zu der [Google-API-Konsole](http://console.developers.google.com) -Website, und melden Sie sich mit Google-Kontoanmeldeinformationen.
1. Wählen Sie ein vorhandenes Projekt, oder erstellen Sie eine neue, aus der Dropdownliste Projekt.
1. Wählen Sie in der Randleiste unter "API-Manager" **Anmeldeinformationen**, und wählen Sie dann die **OAuth Consent Bildschirmregisterkarte**. Wählen Sie eine **e-Mail-Adresse**, geben Sie eine **Produktname, die Benutzern angezeigt wird,**, und drücken Sie die **speichern**.
1. In der **Anmeldeinformationen** Registerkarte die **Anmeldeinformationen erstellen** Dropdown-Liste, und wählen **OAuth-Client-ID**.
1. Klicken Sie unter **Anwendungstyp**, wählen Sie die Plattform, die die mobile Anwendung ausgeführt wird (**iOS** oder **Android**).
1. Füllen Sie die erforderlichen Details ein, und wählen Sie die **erstellen** Schaltfläche.

> [!NOTE]
> Eine Client-ID können eine Anwendung aktivierten Google-APIs zuzugreifen, und für mobile Anwendungen für eine einzelne Plattform eindeutig ist. Aus diesem Grund eine **OAuth-Client-ID** erstellt werden soll, für jede Plattform, die Google-Anmeldung verwenden.

Nach Ausführung dieser Schritte können Xamarin.Auth So initiieren Sie ein "oauth2" Authentifizierungsablauf mit Google verwendet werden.

### <a name="creating-and-configuring-an-authenticator"></a>Erstellen und konfigurieren ein Authentifikator

Der Xamarin.Auth `OAuth2Authenticator` -Klasse ist verantwortlich für die Behandlung der OAuth-authentifizierungsdatenfluss. Das folgende Codebeispiel zeigt die Instanziierung der `OAuth2Authenticator` Klasse bei der Authentifizierung mithilfe der Webbrowser des Geräts ausführen:

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

Die `OAuth2Authenticator` Klasse erfordert eine Anzahl von Parametern, die Folgendes gilt:

- **Client-ID** – Hierdurch wird den Client, der die Anforderung, abgerufen werden kann, aus dem Projekt in der [Google-API-Konsole](http://console.developers.google.com).
- **Geheimer Clientschlüssel** – Dies dürfte `null` oder `string.Empty`.
- **Bereich** – dies identifiziert die von der Anwendung angeforderten API-Zugriff und der Wert informiert die Zustimmung-Bildschirm, der dem Benutzer angezeigt wird. Weitere Informationen zu Bereichen finden Sie unter [Autorisieren von API-Anforderung](https://developers.google.com/+/web/api/rest/oauth) Google-Website.
- **Autorisieren Sie URL** – Hierdurch wird die URL, in dem der Autorisierungscode aus abgerufen werden wird.
- **Umleitungs-URL** – Hierdurch wird die URL, an die Antwort gesendet werden wird. Der Wert dieses Parameters muss übereinstimmen, einen der Werte, die in angezeigt wird der **Anmeldeinformationen** Registerkarte für das Projekt in der [Google-Entwicklerkonsole](https://console.developers.google.com/).
- **AccessToken Url** – Hierdurch wird die URL zum Anfordern von Zugriffstoken nach dem Abrufen eines Autorisierungscodes verwendet.
- **GetUserNameAsync Func** – ein Optionaler `Func` , die verwendet werden wird, den Benutzernamen des Kontos asynchron abgerufen werden, nachdem er erfolgreich authentifiziert wird.
- **Verwenden Sie einheitliche Benutzeroberfläche** – `boolean` Wert, der angibt, ob der Webbrowser des Geräts zu verwenden, um die Authentifizierungsanforderung auszuführen.

### <a name="setup-authentication-event-handlers"></a>Setup-Ereignishandler für die Authentifizierung

Vor dem Bereitstellen der Benutzeroberfläche, einen Ereignishandler für das `OAuth2Authenticator.Completed` Ereignis muss registriert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
authenticator.Completed += OnAuthCompleted;
```

Dieses Ereignis wird ausgelöst, wenn der Benutzer erfolgreich authentifiziert oder bricht der Anmeldeseite ab.

Optional, einen Ereignishandler für das `OAuth2Authenticator.Error` Ereignis kann auch registriert werden.

### <a name="presenting-the-sign-in-user-interface"></a>Präsentieren die Anmeldebenutzeroberfläche-Schnittstelle

Die Anmeldebenutzeroberfläche-Schnittstelle kann mit Xamarin.Auth Anmeldung Referent, die in jede plattformprojekt initialisiert werden muss, die dem Benutzer angezeigt. Im folgenden Codebeispiel wird veranschaulicht, wie eine Anmeldung Vortragende in zu initialisieren der `AppDelegate` Klasse im iOS-Projekt:

```csharp
global::Xamarin.Auth.Presenters.XamarinIOS.AuthenticationConfiguration.Init();
```

Im folgenden Codebeispiel wird veranschaulicht, wie eine Anmeldung Vortragende in zu initialisieren der `MainActivity` Klasse im Android-Projekt:

```csharp
global::Xamarin.Auth.Presenters.XamarinAndroid.AuthenticationConfiguration.Init(this, bundle);
```

Das Projekt der portablen Klassenbibliothek (PCL) kann Anmeldung Referenten. Klicken Sie dann wie folgt aufrufen:

```csharp
var presenter = new Xamarin.Auth.Presenters.OAuthLoginPresenter();
presenter.Login(authenticator);
```

Beachten Sie, dass das Argument für die `Xamarin.Auth.Presenters.OAuthLoginPresenter.Login` Methode ist die `OAuth2Authenticator` Instanz. Wenn die `Login` Methode wird aufgerufen, die Anmeldebenutzeroberfläche-Schnittstelle wird für den Benutzer auf einer Registerkarte angezeigt, über Webbrowser des Geräts, die in den folgenden Screenshots angezeigt wird:

![](oauth-images/login.png "Google-Anmeldung")

### <a name="processing-the-redirect-url"></a>Verarbeiten der Umleitungs-URL

Nachdem der Benutzer den Authentifizierungsprozess abgeschlossen ist, gibt Steuerelement aus der Registerkarte des Webbrowsers für die Anwendung zurück. Dies erfolgt durch registrieren ein benutzerdefinierte URL-Schema für die umleitungs-URL, die den Authentifizierungsprozess, und klicken Sie dann erkennen und behandeln die benutzerdefinierte URL nach dem Senden der es zurückgegeben wird.

Wenn Sie ein benutzerdefinierte URL-Schema zum Zuordnen einer Anwendung auswählen, müssen Anwendungen ein Schema basierend auf einem Namen unter ihrer Kontrolle verwenden. Dies kann erreicht werden, indem Sie den Bezeichnernamen Paket für iOS und dem Paketnamen unter Android verwenden und dann umkehren, um die URL-Schema stellen. Allerdings weisen einige Identitätsanbieter, z. B. Google, Client-IDs, die basierend auf Domänennamen, die dann rückgängig gemacht und als URL-Schema verwendet. Wenn Google Client-Id erstellt z. B. `902730282010-ks3kd03ksoasioda93jldas93jjj22kr.apps.googleusercontent.com`, URL-Schema werden `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr`. Beachten Sie, dass nur ein einzelnes `/` können nach dem Schema-Komponente angezeigt werden. Ein vollständiges Beispiel für eine umleitungs-URL unter Verwendung eines benutzerdefinierten URL-Schemas also `com.googleusercontent.apps.902730282010-ks3kd03ksoasioda93jldas93jjj22kr:/oauth2redirect`.

Wenn der Webbrowser eine Antwort vom Identitätsanbieter, die ein benutzerdefinierte URL-Schema enthält erhält, wird versucht, die URL zu laden, was die fehl. Stattdessen wird die benutzerdefinierte URL-Schema für das Betriebssystem gemeldet, durch das Auslösen eines Ereignisses. Das Betriebssystem wird dann für registrierte Schemas überprüft, und wenn wird eins gefunden, das Betriebssystem wird starten Sie die Anwendung, die das Schema registriert, und die umleitungs-URL zu senden.

Der Mechanismus zum Registrieren eines benutzerdefinierten URL-Schemas mit dem Betriebssystem und behandeln das Schema ist für jede Plattform spezifisch.

#### <a name="ios"></a>iOS

Bei iOS kann ein benutzerdefinierte URL-Schema im registriert **"Info.plist"**, wie im folgenden Screenshot gezeigt:

![](oauth-images/info-plist.png "URL-Schema-Registrierung")

Die **Bezeichner** Werte ist möglich, und die **Rolle** Wert festgelegt werden muss, um **Viewer**. Die **Url-Schemas** -Wert, der mit beginnt `com.googleusercontent.apps`, aus der iOS-Client-Id für das Projekt abgerufen werden kann, um auf [Google-API-Konsole](http://console.developers.google.com).

Wenn der Identitätsanbieter die autorisierungsanforderung abgeschlossen ist, wird er an die Anwendung umleitungs-URL umgeleitet. Da die URL ein benutzerdefiniertes Schema verwendet wird, führt dies zu iOS, die die Anwendung gestartet, in der URL als Parameter starten, in denen Verarbeitung erfolgt durch Übergeben der `OpenUrl` außer Kraft setzen, der der Anwendungsverzeichnis `AppDelegate` -Klasse, die im folgenden Codebeispiel gezeigt wird:

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

Die `OpenUrl` -Methode konvertiert die empfangene URL aus einer `NSUrl` in ein .NET `Uri`, vor der Verarbeitung der umleitungs-URL mit der `OnPageLoading` einer öffentlichen Methode `OAuth2Authenticator` Objekt. Dies bewirkt, dass Xamarin.Auth, um die Registerkarte des Webbrowsers zu schließen und die empfangenen OAuth-Daten zu analysieren.

#### <a name="android"></a>Android

Auf Android-Geräten wird ein benutzerdefinierte URL-Schema durch Angabe registriert eine [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) -Attribut auf die `Activity` behandelt, die das Schema. Wenn der Identitätsanbieter die autorisierungsanforderung abgeschlossen ist, wird er an die Anwendung umleitungs-URL umgeleitet. Wie die URL ein benutzerdefiniertes Schema verwendet, führt dies zu Android die Anwendung gestartet, in der URL als Parameter starten, in denen Verarbeitung erfolgt durch Übergeben der `OnCreate` Methode der `Activity` registriert, um die benutzerdefinierte URL-Schema zu behandeln. Das folgende Codebeispiel zeigt die Klasse aus der beispielanwendung, die die benutzerdefinierte URL-Schema behandelt:

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

Die `DataSchemes` Eigenschaft von der [ `IntentFilter` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) muss festgelegt werden, um die umgekehrten Clientbezeichner, die aus der Android-Client-Id für das Projekt auf abgerufen werden [Google-API-Konsole](http://console.developers.google.com).

Die `OnCreate` -Methode konvertiert die empfangene URL aus einer `Android.Net.Url` in ein .NET `Uri`, vor der Verarbeitung der umleitungs-URL mit der `OnPageLoading` einer öffentlichen Methode `OAuth2Authenticator` Objekt. Dies bewirkt, dass Xamarin.Auth, schließen Sie die Registerkarte des Webbrowsers, und die empfangenen OAuth-Daten zu analysieren.

> [!IMPORTANT]
> Auf Android-Geräten Xamarin.Auth verwendet die `CustomTabs` API für die Kommunikation mit dem Webbrowser und Betriebssystem. Allerdings ist nicht gewährleistet, die eine `CustomTabs` kompatiblen Browser auf dem Gerät des Benutzers installiert werden soll.

### <a name="examining-the-oauth-response"></a>Untersuchen die OAuth-Antwort

Nach der Analyse der empfangenen OAuth Daten Xamarin.Auth löst die `OAuth2Authenticator.Completed` Ereignis. Im Ereignishandler für dieses Ereignis besitzt die `AuthenticatorCompletedEventArgs.IsAuthenticated` Eigenschaft kann verwendet werden, um zu identifizieren, ob die Authentifizierung erfolgreich war, wie im folgenden Codebeispiel wird dargestellt:

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

Die von einer erfolgreichen Authentifizierung erfassten Daten finden Sie in der `AuthenticatorCompletedEventArgs.Account` Eigenschaft. Dies schließt ein Zugriffstoken, das zum Signieren von Anforderungen für Daten an eine API, die vom Identitätsanbieter verwendet werden kann.

### <a name="making-requests-for-data"></a>Datenanforderungen vorgenommen.

Nach die Anwendung ein Zugriffstoken abruft, müssen sie dient zum Senden von Anforderungen an die `https://www.googleapis.com/oauth2/v2/userinfo` -API, um grundlegende Daten vom Identitätsanbieter. Wird diese Anforderung mit der Xamarin.Auth `OAuth2Request` Klasse, die eine Anforderung darstellt, der mit einem Konto aus abgerufenen authentifiziert wird ein `OAuth2Authenticator` Instanz, wie im folgenden Codebeispiel gezeigt:

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

Sowie die HTTP-Methode und die URL-API der `OAuth2Request` Instanz gibt auch an ein `Account` Instanz, die das Zugriffstoken enthält, die die Anforderung an die vom angegebenen URL signiert die `Constants.UserInfoUrl` Eigenschaft. Der Identitätsanbieter zurückgegeben grundlegende Benutzerdaten als eine JSON-Antwort, einschließlich der Benutzer-Name und e-Mail-Adresse, vorausgesetzt, dass das Zugriffstoken als gültig erkennt. Die JSON-Antwort dann gelesen und in deserialisiert die `user` Variable.

Weitere Informationen finden Sie unter [eine Google-API Aufrufen](https://developers.google.com/identity/protocols/OAuth2InstalledApp#callinganapi) Google-Entwickler-Portal.

### <a name="storing-and-retrieving-account-information-on-devices"></a>Speichern und Abrufen von Kontoinformationen auf Geräten

Xamarin.Auth sicher gespeichert `Account` Speichern von Objekten in einem Konto, damit Anwendungen immer keine Benutzer erneut zu authentifizieren. Die `AccountStore` Klasse ist verantwortlich für das Speichern von Kontoinformationen und von Keychain-Diensten in iOS, gesichert ist und die `KeyStore` Klasse in Android.

Das folgende Codebeispiel zeigt, wie ein `Account` -Objekts sicher gespeichert wird:

```csharp
AccountStore.Create ().Save (e.Account, Constants.AppName);
```

Gespeicherte Konten sind eindeutig identifiziert mithilfe eines Schlüssels besteht aus des Kontos `Username` Eigenschaft und eine Dienst-ID, d. h. eine Zeichenfolge, die beim Abrufen von Konten aus dem Store Konto verwendet wird. Wenn ein `Account` zuvor gespeichert wurde, wird Aufrufen der `Save` Methode erneut überschreibt es.

`Account` Objekte für ein Dienst kann, durch Aufrufen abgerufen werden der `FindAccountsForService` Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
var account = AccountStore.Create ().FindAccountsForService (Constants.AppName).FirstOrDefault();
```

Die `FindAccountsForService` Methode gibt ein `IEnumerable` Auflistung von `Account` Objekte, mit dem ersten Element in der Auflistung, der als das übereinstimmende Konto festgelegt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Xamarin.Auth verwenden, um den Authentifizierungsvorgang in einer Xamarin.Forms-Anwendung zu verwalten. Xamarin.Auth bietet die `OAuth2Authenticator` und `OAuth2Request` Klassen, die von Anwendungen mit Xamarin.Forms verwendet werden, z. B. Google, Microsoft, Facebook und Twitter-Identitätsanbieter zu nutzen.


## <a name="related-links"></a>Verwandte Links

- [OAuthNativeFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/OAuthNativeFlow/)
- [OAuth 2.0 für systemeigene Apps](https://tools.ietf.org/html/draft-ietf-oauth-native-apps-12)
- [Mithilfe von Google-APIs den Zugriff auf OAuth2.0](https://developers.google.com/identity/protocols/OAuth2)
- [Xamarin.Auth (NuGet)](https://www.nuget.org/packages/xamarin.auth/)
- [Xamarin.Auth (GitHub)](https://github.com/xamarin/Xamarin.Auth)
