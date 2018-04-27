---
title: Authentifizieren von Benutzern mit Azure Active Directory B2C
description: Azure Active Directory B2C ist eine Cloud-identitätsverwaltungslösung für kundenorientierten Web- und mobilen Anwendungen. Dieser Artikel veranschaulicht, wie Microsoft-Authentifizierungsbibliothek und Azure Active Directory B2C Consumer identitätsverwaltung in einer mobilen Anwendung integrieren.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 627c6773c099c9cf45f871a9bb73a201bf98271a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2018
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Authentifizieren von Benutzern mit Azure Active Directory B2C

_Azure Active Directory B2C ist eine Cloud-identitätsverwaltungslösung für kundenorientierten Web- und mobilen Anwendungen. Dieser Artikel veranschaulicht, wie Microsoft-Authentifizierungsbibliothek und Azure Active Directory B2C Consumer identitätsverwaltung in einer mobilen Anwendung integrieren._

![](~/media/shared/preview.png "Diese API ist derzeit Vorabversion")

> [!NOTE]
> Die [Microsoft-Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Identity.Client) noch in der Vorschau ist, aber für die Verwendung in einer produktiven Umgebung geeignet ist. Allerdings kann es aktuelle werden Änderungen an der API, internen Cache-Format und andere Mechanismen der Bibliothek, die sich auf Ihrer Anwendung auswirken kann.

## <a name="overview"></a>Übersicht

Azure Active Directory B2C ist ein identitätsverwaltungsdienst bei kundenorientierten-Anwendungen, die Consumern, die Anwendung unter anmelden kann:

- Verwenden ihre vorhandenen sozialen Konten (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Erstellen neue Anmeldeinformationen (e-Mail-Adresse und Kennwort oder Benutzernamen und Kennwort). Diese Anmeldeinformationen werden als bezeichnet *lokale* Konten.

Der Prozess für die Integration von Azure Active Directory B2C Identity-Management-Dienst in einer mobilen Anwendung lautet wie folgt:

1. Erstellen eines Azure Active Directory B2C-Mandanten. Weitere Informationen finden Sie unter [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Registrieren Sie Ihre mobile Anwendung mit dem Azure Active Directory B2C-Mandanten ein. Während der Registrierung weist eine **Anwendungs-ID** Identifizierung Ihrer Anwendung und einer **Umleitungs-URL** , weiterleiten, Antworten zurück an Ihre Anwendung verwendet werden kann. Weitere Informationen finden Sie unter [Azure Active Directory B2C: Registrieren Ihrer Anwendung](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Erstellen Sie eine Anmelde und melden Sie sich die Richtlinie. Diese Richtlinie definiert die Erfahrungen, die während der Registrierung und melden Sie sich der Consumer durchlaufen werden, und gibt auch den Inhalt der Token die Anwendung erhält erfolgreich ausgeführt registrieren oder anmelden. Weitere Informationen finden Sie unter [Azure Active Directory B2C: integrierter Richtlinien](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Verwenden der [Microsoft-Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) in der mobilen Anwendung um einen authentifizierungsworkflow mit Ihrem Azure Active Directory B2C-Mandanten zu initiieren.

> [!NOTE]
> Sowie die Integration von Azure Active Directory B2C-identitätsverwaltung in mobilen Anwendungen, werden MSAL auch zum Integrieren von Azure Active Directory-identitätsverwaltung in mobilen Anwendungen verwendet. Dies kann durch eine mobile Anwendung mit Azure Active Directory zur Registrierung der [App-Registrierungsportal](https://apps.dev.microsoft.com/). Während der Registrierung weist eine **Anwendungs-ID** Identifizierung Ihrer Anwendung, die bei Verwendung von MSAL angegeben werden soll. Weitere Informationen finden Sie unter [zum Registrieren einer app mit dem Endpunkt v2. 0](/azure/active-directory/develop/active-directory-v2-app-registration/), und [authentifizieren Ihrer mobiler Apps mithilfe von Microsoft-Authentifizierungsbibliothek](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) im Xamarin-Blog.

MSAL verwendet Webbrowser des Geräts zum Durchführen der Authentifizierung. Dies verbessert die Verwendbarkeit einer Anwendung, wie Benutzer müssen sich nur anmelden, nachdem jedes Gerät, in der Anwendung verbessern Wechselkurse für die Anmeldung und Autorisierung fließt. Die Gerätebrowser bietet auch eine verbesserte Sicherheit. Nachdem der Benutzer den Authentifizierungsprozess abgeschlossen ist, gibt Steuerelement aus der Registerkarte des Webbrowsers für die Anwendung zurück. Dies erfolgt durch registrieren ein benutzerdefinierte URL-Schema für die umleitungs-URL, die den Authentifizierungsprozess, und klicken Sie dann erkennen und behandeln die benutzerdefinierte URL nach dem Senden der es zurückgegeben wird. Weitere Informationen zum Auswählen eines benutzerdefinierten URL-Schemas finden Sie unter [wählen einen umleitungs-URI für das systemeigene app](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> Der Mechanismus zum Registrieren eines benutzerdefinierten URL-Schemas mit dem Betriebssystem und behandeln das Schema ist für jede Plattform spezifisch.

Gibt an, jede Anforderung, die auf einen Azure Active Directory B2C-Mandanten gesendet wird eine *Richtlinie*. Richtlinien beschreiben Consumer Identität Erfahrungen z. B. registrieren oder anmelden. So kann z. B. eine Richtlinie registrieren Sie sich das Verhalten des Azure Active Directory B2C-Mandanten über die folgenden Einstellungen konfiguriert werden:

- Kontotypen, die Consumer zur Anmeldung an die Anwendung verwenden können.
- Daten, die bei der Registrierung von der Consumer gesammelt werden sollen.
- Multi-Factor Authentication.
- Registrierungsseite Inhalt.
- Token Ansprüche, die die mobile Anwendung erhält, wenn die Richtlinie ausgeführt wurde.

Azure Active Directory-Mandanten kann mehrere Richtlinien verschiedener Typen enthalten, die in der Anwendung nach Bedarf verwendet werden können. Darüber hinaus können Richtlinien für Anwendungen, sodass Sie definieren und Ändern von Consumer Identität Erfahrungen ohne Änderung des Codes wiederverwendet werden. Weitere Informationen zu Richtlinien finden Sie unter [Azure Active Directory B2C: integrierter Richtlinien](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Setup

Die Microsoft Authentication Library (MSAL) NuGet-Bibliothek muss auf die Portable Klassenbibliothek (PCL)-Projekt und die Plattformprojekte in einer Xamarin.Forms-Projektmappe hinzugefügt werden. Die folgenden Abschnitte enthalten weitere installationsanweisungen für die Verwendung von MSAL für die Kommunikation mit einem Azure Active Directory B2C-Mandanten aus einer mobilen Anwendung.

### <a name="portable-class-library"></a>Portable Klassenbibliothek

PCLs, die MSAL nutzen müssen, um Profile7 verwenden umgeleitet zu werden. Weitere Informationen zu PCLs finden Sie unter [Introduction to Portable Class Libraries (Einführung in portable Klassenbibliotheken)](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

IOS, muss die benutzerdefinierte URL-Schema, die mit Azure Active Directory B2C registriert war in registriert werden **"Info.plist"**, wie im folgenden Screenshot gezeigt:

![](azure-ad-b2c-images/customurl-ios.png "Registrieren eine benutzerdefinierte URL-Schema für iOS")

Wenn Azure Active Directory B2C die autorisierungsanforderung abgeschlossen ist, wird er an der registrierte umleitungs-URL umgeleitet. Da die URL ein benutzerdefiniertes Schema verwendet wird, führt dies zu iOS, die die mobile Anwendung gestartet, in der URL als Parameter starten, in denen Verarbeitung erfolgt durch Übergeben der `OpenUrl` außer Kraft setzen, der der Anwendungsverzeichnis `AppDelegate` -Klasse, die in den folgenden Code gezeigt wird Beispiel:

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

Der Code in die `OpenURL` Methode wird sichergestellt, dass die Steuerung an MSAL zurückgegeben, sobald der interaktiven Teil der authentifizierungsworkflow beendet wurde.

### <a name="android"></a>Android

Auf Android-Geräten muss die benutzerdefinierte URL-Schema, die mit Azure Active Directory B2C registriert war in registriert werden **AndroidManifest.xml**, durch Hinzufügen einer `<activity>` Element innerhalb der vorhandenen `<application>` Element. Die `<activity>` -Element gibt die `IntentFilter` auf die `Activity` , behandelt das Schema, und wird im folgenden Beispiel gezeigt:

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

Wenn Azure Active Directory B2C die autorisierungsanforderung abgeschlossen ist, wird er an der registrierte umleitungs-URL umgeleitet. Da die URL ein benutzerdefiniertes Schema verwendet wird, führt dies zu Android, die die mobile Anwendung gestartet, in der URL als Parameter starten, in denen Verarbeitung erfolgt durch Übergeben der `microsoft.identity.client.BrowserTabActivity`. Beachten Sie, dass die `data android:scheme` Eigenschaft muss festgelegt werden, um die benutzerdefinierte URL-Schema, die mit der Azure Active Directory B2C-Anwendung registriert ist.

Darüber hinaus die `MainActivity` Klasse muss geändert werden, wie im folgenden Codebeispiel gezeigt:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
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

Die `OnCreate` Methode geändert wird, durch Zuweisen einer `UIParent` -Instanz, auf die `App.UiParent` Eigenschaft. Dadurch wird sichergestellt, dass der Authentifizierungsablauf im Kontext der aktuellen Aktivität erfolgt.

Der Code in die `OnActivityResult` Methode wird sichergestellt, dass die Steuerung an MSAL zurückgegeben, sobald der interaktiven Teil der authentifizierungsworkflow beendet wurde.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Klicken Sie auf die universelle Windows-Plattform muss ohne zusätzliche Einrichtung MSAL verwenden.

## <a name="initialization"></a>Initialisierung

Die Microsoft-Authentifizierungsbibliothek liegt bei Mitgliedern der die `PublicClientApplication` Klasse, um einen authentifizierungsworkflow zu initiieren. Die beispielanwendung deklariert und initialisiert ein `public` -Eigenschaft dieses Typs, mit dem Namen `ADB2CClient`in die `AuthenticationProvider` Klasse. Im folgenden Codebeispiel wird veranschaulicht, wie diese Eigenschaft initialisiert wird:

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

Wenn die mobile Anwendung mit dem Azure Active Directory B2C-Mandanten registriert wurde, während der Registrierung zugewiesen ein **Anwendungs-ID**. Diese ID muss angegeben werden, der `PublicClientApplication` Konstruktor, zusammen mit einer `Authority` Konstante, die besteht aus einer Basis-URL und die Azure Active Directory B2C-Richtlinie ausgeführt werden.

## <a name="signing-in"></a>Anmelden

Die im Anmeldebildschirm ein, in der beispielanwendung wird in den folgenden Screenshots veranschaulicht:

![](azure-ad-b2c-images/login.png "Anmeldeseite")

Anmelden mit Identitätsanbieter sozialer Netzwerke oder mit einem lokalen Konto, sind zulässig. Während Microsoft und Google, Facebook, wie oben gezeigt als Identitätsanbieter sozialer Netzwerke, verwendet werden können auch andere Identitätsanbieter verwendet werden.

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

Die `AcquireTokenAsync` Methode startet der Webbrowser des Geräts und zeigt die Authentifizierungsoptionen definiert, in der Azure Active Directory B2C-Richtlinie, die von der Richtlinie verwiesen wird, über angegeben wird die `Constants.Authority` konstant. Diese Richtlinie definiert die Features, mit denen Kunden durchlaufen werden bei der Registrierung, und melden Sie sich, und die Ansprüche an, dass die Anwendung erfolgreich empfangen wird, registrieren oder anmelden.

Das Ergebnis der `AcquireTokenAsync` Methodenaufruf ist ein `AuthenticationResult` Instanz. Wenn die Authentifizierung erfolgreich ist, ist die `AuthenticationResult` Instanz enthält ein Identitätstoken an, die lokal zwischengespeichert wird. Wenn die Authentifizierung nicht erfolgreich ist, ist die `AuthenticationResult` Instanz enthält Daten, der angibt, warum die Authentifizierung fehlgeschlagen ist.

In der beispielanwendung, wenn die Authentifizierung erfolgreich ist, ist die `TodoList` Seite navigiert wird.

## <a name="silent-re-authentication"></a>Automatische erneute Authentifizierung

Wenn die `LoginPage` im Beispiel für die Anwendung angezeigt wird, es wird versucht, ein Benutzertoken abzurufen, ohne Anzeige einer Benutzeroberfläche für die Authentifizierung. Dies wird erreicht, mit der `AcquireTokenSilentAsync` Methode, wie im folgenden Codebeispiel gezeigt:

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

Die `AcquireTokenSilentAsync` Methode versucht, ein Benutzertoken aus dem Cache abgerufen werden, ohne dass der Benutzer zur Anmeldung. Verarbeitet das Szenario, in dem eine geeignete Token bereits im Cache aus früheren Sitzungen möglicherweise vorhanden. Wenn der Versuch zum Abrufen eines Token erfolgreich ist, ist die `TodoList` Seite navigiert wird. Wenn der Versuch zum Abrufen eines Token nicht erfolgreich ist, geschieht nichts, und der Benutzer muss die Wahl ein neuen Workflows für die Authentifizierung initiiert.

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

Dadurch werden alle Authentifizierungstoken vom lokalen Cache gelöscht.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie Microsoft Authentication Library (MSAL) und Azure Active Directory B2C verwenden, um die identitätsverwaltung Consumer in eine mobile Anwendung integriert wird. Azure Active Directory B2C ist eine Cloud-identitätsverwaltungslösung für kundenorientierten Web- und mobilen Anwendungen.


## <a name="related-links"></a>Verwandte Links

- [AzureADB2CAuth (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft-Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Identity.Client)
