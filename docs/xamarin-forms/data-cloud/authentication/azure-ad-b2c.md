---
title: Authentifizieren von Benutzern mit Azure Active Directory B2C
description: Azure Active Directory B2C ist eine Cloudlösung für die Verwaltung von Identitäten für kundenorientierte Web- und mobile Anwendungen. In diesem Artikel wird veranschaulicht, wie Microsoft-Authentifizierungsbibliothek und Azure Active Directory B2C verwenden, um die Verwaltung von Kundenidentitäten in eine mobile Anwendung integrieren.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: b6989782c438ec41911cc9317d9f911d6518132d
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38872721"
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Authentifizieren von Benutzern mit Azure Active Directory B2C

_Azure Active Directory B2C ist eine Cloudlösung für die Verwaltung von Identitäten für kundenorientierte Web- und mobile Anwendungen. In diesem Artikel wird veranschaulicht, wie Microsoft-Authentifizierungsbibliothek und Azure Active Directory B2C verwenden, um die Verwaltung von Kundenidentitäten in eine mobile Anwendung integrieren._

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

> [!NOTE]
> Die [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) ist weiterhin als Vorschau verfügbar, aber für die Verwendung in einer produktionsumgebung geeignet ist. Allerdings kann es wichtige werden Änderungen an der API, internen und Mechanismen für die Bibliothek, die Ihre Anwendung auswirken können.

## <a name="overview"></a>Übersicht

Azure Active Directory B2C ist ein identitätsverwaltungsdienst für kundenorientierte Anwendungen, die Kunden bei Ihrer Anwendung anmelden kann:

- Verwenden ihren vorhandenen Konten sozialer Netzwerke (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Erstellen neue Anmeldeinformationen (e-Mail-Adresse und Kennwort oder Benutzername und Kennwort). Diese Anmeldeinformationen werden als bezeichnet *lokalen* Konten.

Der Prozess für die Integration der Azure Active Directory B2C-Identity-Management-Dienst in einer mobilen Anwendung lautet wie folgt aus:

1. Erstellen eines Azure Active Directory B2C-Mandanten. Weitere Informationen finden Sie unter [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Registrieren Sie Ihre mobile Anwendung mit dem Azure Active Directory B2C-Mandanten an. Während der Registrierung weist eine **Anwendungs-ID** , die Ihre Anwendung eindeutig identifiziert und **Umleitungs-URL** , der zum Umleiten von Antworten zurück an Ihre Anwendung verwendet werden kann. Weitere Informationen finden Sie unter [Azure Active Directory B2C: Registrieren Ihrer Anwendung](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Erstellen einer Richtlinie für Registrierung und Anmeldung. Diese Richtlinie wird definiert, die Benutzeroberflächen, die Kunden während der Registrierung und Anmeldung durchlaufen, und gibt auch den Inhalt der Token die Anwendung erhält erfolgreich Registrierungs- oder Anmelderichtlinie. Weitere Informationen finden Sie unter [Azure Active Directory B2C: integrierte Richtlinien](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Verwenden der [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) in Ihre mobile Anwendung um einen authentifizierungsworkflow bei Ihrem Azure Active Directory B2C-Mandanten zu initiieren.

> [!NOTE]
> Sowie das Integrieren der identitätsverwaltung für Azure Active Directory B2C in mobile Anwendungen, kann auch MSAL verwendet werden, Azure Active Directory-identitätsverwaltung in mobilen Anwendungen integrieren. Dies geschieht durch Registrieren einer mobilen Anwendung in Azure Active Directory auf die [App-Registrierungsportal](https://apps.dev.microsoft.com/). Während der Registrierung weist eine **Anwendungs-ID** Kennzeichnung Ihrer Anwendung, die bei Verwendung der MSAL angegeben werden. Weitere Informationen finden Sie unter [zum Registrieren einer app mit dem v2. 0-Endpunkt](/azure/active-directory/develop/active-directory-v2-app-registration/), und [authentifizieren Ihrer Mobile Apps mithilfe von Microsoft-Authentifizierungsbibliothek](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) im Xamarin-Blog.

MSAL verwendet Webbrowser des Geräts, um die Authentifizierung durchzuführen. Dies verbessert die Verwendbarkeit einer Anwendung, die als Benutzer müssen sich nur anmelden nach pro-Gerät in der Anwendung verbessern konvertierungsraten-Anmeldung und Autorisierung übertragen. Der Browser für Gerät bietet auch eine verbesserte Sicherheit. Nachdem der Benutzer den Authentifizierungsprozess abgeschlossen ist, gibt die Kontrolle an die Anwendung aus der Registerkarte des Webbrowsers zurück. Dies erfolgt durch die Registrierung eines benutzerdefinierten URL-Schemas für die umleitungs-URL, die zurückgegeben wird, aus der Authentifizierungsprozess, und klicken Sie dann erkennen und behandeln die benutzerdefinierte URL ein, nach der sie gesendet wird. Weitere Informationen zum Auswählen eines benutzerdefinierten URL-Schemas finden Sie unter [Auswählen eines umleitungs-URI für das systemeigene app](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> Der Mechanismus zum Registrieren eines benutzerdefinierten URL-Schemas mit dem Betriebssystem, und behandeln das Schema ist für jede Plattform spezifisch.

Gibt an, jede Anforderung, die mit einem Azure Active Directory B2C-Mandanten gesendet wird eine *Richtlinie*. Richtlinien beschreiben Benutzeroberflächen im kundenidentität, z. B. Registrierung oder Anmeldung. Eine Registrierungsrichtlinie ermöglicht beispielsweise das Verhalten des Azure Active Directory B2C-Mandanten über die folgenden Einstellungen konfiguriert werden:

- Arten von Konten, mit denen Kunden bei der Anwendung anmelden.
- Daten, die der Kunde bei der Registrierung werden.
- Multi-Factor Authentication.
- Inhalt der Seite für die Registrierung.
- Tokenansprüche, die die mobile Anwendung empfängt, wenn die Richtlinie ausgeführt wurde.

Azure Active Directory-Mandanten kann mehrere Richtlinien verschiedener Typen enthalten, die in Ihrer Anwendung nach Bedarf verwendet werden können. Darüber hinaus können Richtlinien für Anwendungen, sodass Sie definieren und Ändern der kundenidentität ohne Änderung des Codes, wiederverwendet werden. Weitere Informationen zu Richtlinien finden Sie unter [Azure Active Directory B2C: integrierte Richtlinien](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Setup

Das Projekt der portablen Klassenbibliothek (PCL) und Platform-Projekten in einer Xamarin.Forms-Projektmappe muss die Microsoft Authentication Library (MSAL) NuGet-Bibliothek hinzugefügt werden. Die folgenden Abschnitte enthalten weitere installationsanweisungen für die Verwendung von MSAL für die Kommunikation mit einem Azure Active Directory B2C-Mandanten in einer mobilen Anwendung.

### <a name="portable-class-library"></a>Portable Klassenbibliothek

Portable Klassenbibliotheken, die MSAL verwenden, müssen neu zugewiesen werden, um Profile7 zu verwenden. Weitere Informationen zu PCLs finden Sie unter [Introduction to Portable Class Libraries (Einführung in portable Klassenbibliotheken)](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

Unter iOS, muss die benutzerdefinierte URL-Schema, die mit Azure Active Directory B2C registriert wurde im registriert werden **"Info.plist"**, wie im folgenden Screenshot gezeigt:

![](azure-ad-b2c-images/customurl-ios.png "Registrieren einer benutzerdefinierten URL-Schemas unter iOS")

Wenn Azure Active Directory B2C die autorisierungsanforderung abgeschlossen ist, erfolgt eine Umleitung an die registrierte umleitungs-URL. Da die URL ein benutzerdefiniertes Schema verwendet, dies zu iOS führt, starten die mobile Anwendung, in der URL übergeben, als Parameter starten, in dem vom verarbeitet die `OpenUrl` außer Kraft setzen, von der Anwendung `AppDelegate` -Klasse, die in den folgenden Code gezeigt wird Beispiel:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return true;
        }
    }
}
```

Der Code in die `OpenURL` Methode wird sichergestellt, die Steuerung an MSAL zurückgegeben wird, wenn der interaktive Teil des Workflows für die Authentifizierung beendet wurde.

### <a name="android"></a>Android

Unter Android, muss die benutzerdefinierte URL-Schema, die mit Azure Active Directory B2C registriert wurde in registriert werden **"androidmanifest.xml"**, durch das Hinzufügen einer `<activity>` Element innerhalb der vorhandenen `<application>` Element. Die `<activity>` Element gibt die `IntentFilter` auf die `Activity` , behandelt das Schema und ist im folgenden Beispiel gezeigt:

```xml
<application ...>
  <activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <data android:scheme="INSERT_URL_SCHEME_HERE" android:host="auth" />
    </intent-filter>
  </activity>
</application>
```

Wenn Azure Active Directory B2C die autorisierungsanforderung abgeschlossen ist, erfolgt eine Umleitung an die registrierte umleitungs-URL. Da die URL ein benutzerdefiniertes Schema verwendet, Android, die die mobile Anwendung gestartet wird, übergeben Sie in der URL als Parameter starten, in dem vom verarbeitet die `microsoft.identity.client.BrowserTabActivity`. Beachten Sie, dass die `data android:scheme` Eigenschaft muss festgelegt werden, um die benutzerdefinierte URL-Schema, das mit der Azure Active Directory B2C-Anwendung registriert ist.

Darüber hinaus die `MainActivity` Klasse muss geändert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);
            Microsoft.WindowsAzure.MobileServices.CurrentPlatform.Init();
            LoadApplication(new App());
            App.UiParent = new UIParent(this);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
        }
    }
}

```

Die `OnCreate` -Methode geändert, durch Zuweisen einer `UIParent` -Instanz, auf die `App.UiParent` Eigenschaft. Dadurch wird sichergestellt, dass der Ablauf der Authentifizierung im Kontext der aktuellen Aktivität auftritt.

Der Code in die `OnActivityResult` Methode wird sichergestellt, die Steuerung an MSAL zurückgegeben wird, wenn der interaktive Teil des Workflows für die Authentifizierung beendet wurde.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Klicken Sie auf der universellen Windows-Plattform ist ohne zusätzliche Einrichtung erforderlich, um MSAL verwenden.

## <a name="initialization"></a>Initialisierung

Der Microsoft-Authentifizierungsbibliothek liegt bei Mitgliedern der der `PublicClientApplication` Klasse, um einen authentifizierungsworkflow zu initiieren. Die beispielanwendung deklariert und initialisiert ein `public` Eigenschaft dieses Typs, mit dem Namen `ADB2CClient`in die `AuthenticationProvider` Klasse. Im folgenden Codebeispiel wird veranschaulicht, wie diese Eigenschaft initialisiert wird:

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

Wenn die mobile Anwendung mit dem Azure Active Directory B2C-Mandanten registriert wurde, während der Registrierung zugewiesen ein **Anwendungs-ID**. Diese ID muss angegeben werden, der `PublicClientApplication` Konstruktor zusammen mit einem `Authority` -Konstante, die eine Basis-URL und die Azure Active Directory B2C-Richtlinie ausgeführt werden, besteht aus.

## <a name="signing-in"></a>Anmeldung

Der Anmeldebildschirm in der beispielanwendung wird in den folgenden Screenshots dargestellt:

![](azure-ad-b2c-images/login.png "Anmeldeseite")

Anmeldung mit Identitätsanbietern aus sozialen Netzwerken oder mit einem lokalen Konto, sind zulässig. Während Microsoft, Google und Facebook, wie oben gezeigt als Identitätsanbietern aus sozialen Netzwerken verwendet werden können auch andere Identitätsanbieter verwendet werden.

Im folgenden Codebeispiel wird veranschaulicht, wie der Anmeldevorgang aufgerufen wird:

```csharp
using Microsoft.Identity.Client;

public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);
    ...
}

```

Die `AcquireTokenAsync` Methode Webbrowser des Geräts startet und zeigt die Authentifizierungsoptionen, die definiert, in der Azure Active Directory B2C-Richtlinie, die von der Richtlinie, die auf die verwiesen wird durch angegeben wird die `Constants.Authority` Konstanten. Diese Richtlinie definiert die Benutzeroberflächen, die Kunden bei der Registrierung und Anmeldung durchlaufen, und die Ansprüche an, dass die Anwendung beim erfolgreichen Registrierungs- oder Anmelderichtlinie erhält.

Das Ergebnis der `AcquireTokenAsync` Methodenaufruf ist ein `AuthenticationResult` Instanz. Wenn die Authentifizierung erfolgreich ist, ist die `AuthenticationResult` Instanz enthält ein Identitätstoken, die lokal zwischengespeichert wird. Wenn die Authentifizierung nicht erfolgreich ist, ist die `AuthenticationResult` Instanz enthält Daten, der angibt, warum die Authentifizierung fehlgeschlagen ist.

In diesem Beispiel, wenn die Authentifizierung erfolgreich ist, ist die `TodoList` Seite navigiert wird.

## <a name="silent-re-authentication"></a>Automatische erneute Authentifizierung

Wenn die `LoginPage` im Beispiel Anwendung angezeigt wird, es wird versucht, ein Benutzertoken abgerufen werden, ohne dass eine Benutzeroberfläche für die Authentifizierung. Dies wird erreicht, mit der `AcquireTokenSilentAsync` -Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult;

    if (useSilent)
    {
        authenticationResult = await ADB2CClient.AcquireTokenSilentAsync(
            Constants.Scopes,
            GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
            Constants.Authority,
            false);
    }
    ...
}
```

Die `AcquireTokenSilentAsync` Methode versucht, ein Benutzertoken aus dem Cache abrufen, ohne dass der Benutzer sich anmelden. Diese behandelt das Szenario, in denen möglicherweise ein geeignetes Token bereits aus früheren Sitzungen im Cache vorhanden sein. Wenn der Versuch zum Abrufen eines Tokens erfolgreich ist, ist die `TodoList` Seite navigiert wird. Wenn der Versuch zum Abrufen eines Tokens nicht erfolgreich ist, geschieht nichts, und der Benutzer muss die Möglichkeit, einen neuer authentifizierungsworkflow für die zu initiieren.

## <a name="signing-out"></a>Abmelden

Im folgenden Codebeispiel wird veranschaulicht, wie der Abmeldevorgang aufgerufen wird:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

Dadurch werden die Authentifizierungstoken aus dem lokalen Cache gelöscht.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Microsoft Authentication Library (MSAL) und Azure Active Directory B2C verwenden, um die Verwaltung von Kundenidentitäten in eine mobile Anwendung integrieren. Azure Active Directory B2C ist eine Cloudlösung für die Verwaltung von Identitäten für kundenorientierte Web- und mobile Anwendungen.


## <a name="related-links"></a>Verwandte Links

- [AzureADB2CAuth (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft-Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Identity.Client)
