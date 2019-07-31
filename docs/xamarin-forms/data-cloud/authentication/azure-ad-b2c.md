---
title: Authentifizieren von Benutzern mit Azure Active Directory B2C
description: Azure Active Directory B2C bietet cloudbasierte Identitätsverwaltung für kundenorientierte Web-und Mobile Anwendungen. In diesem Artikel wird beschrieben, wie Sie mithilfe von Azure Active Directory B2C die Identitätsverwaltung mithilfe der Microsoft-Authentifizierungs Bibliothek in eine mobile Anwendung integrieren.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2019
ms.openlocfilehash: 765f34af3b3c43531857b705bb4a39ea56e32f61
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656133"
---
# <a name="authenticate-users-with-azure-active-directory-b2c"></a>Authentifizieren von Benutzern mit Azure Active Directory B2C

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azureadb2cauth)

_Azure Active Directory B2C bietet cloudbasierte Identitätsverwaltung für kundenorientierte Web-und Mobile Anwendungen. In diesem Artikel wird beschrieben, wie Sie mithilfe von Azure Active Directory B2C die Identitätsverwaltung mithilfe der Microsoft-Authentifizierungs Bibliothek in eine mobile Anwendung integrieren._

## <a name="overview"></a>Übersicht

Azure Active Directory B2C (ADB2C) ist ein Identitäts Verwaltungsdienst für kundenorientierte Anwendungen. Sie ermöglicht Benutzern die Anmeldung bei Ihrer Anwendung mithilfe Ihrer vorhandenen Konten für soziale Netzwerke oder benutzerdefinierter Anmelde Informationen, wie z. b. e-Mail oder Benutzername und Kennwort. Benutzerdefinierte Anmelde Informations Konten werden als _lokale_ Konten bezeichnet.

Der Prozess für die Integration der Azure Active Directory B2C-Identity-Management-Dienst in einer mobilen Anwendung lautet wie folgt aus:

1. Erstellen eines Azure Active Directory B2C Mandanten
1. Registrieren Ihrer mobilen Anwendung beim Azure Active Directory B2C-Mandanten
1. Erstellen von Richtlinien für Registrierung und Anmeldung und Kenn Wort benutzerflows
1. Verwenden Sie die Microsoft Authentication Library (msal), um einen Authentifizierungs Workflow mit Ihrem Azure Active Directory B2C Mandanten zu starten.

> [!NOTE]
> Azure Active Directory B2C unterstützt verschiedene Identitäts Anbieter, einschließlich Microsoft, GitHub, Facebook, Twitter und mehr. Weitere Informationen zu Azure Active Directory B2C Funktionen finden Sie in der [Azure Active Directory B2C-Dokumentation](/azure/active-directory-b2c/).
>
> Die Microsoft-Authentifizierungs Bibliothek unterstützt mehrere Anwendungsarchitekturen und-Plattformen. Weitere Informationen zu msal-Funktionen finden Sie in der [Microsoft-Authentifizierungs Bibliothek](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) auf GitHub.

## <a name="configure-an-azure-active-directory-b2c-tenant"></a>Konfigurieren eines Azure Active Directory B2C Mandanten

Wenn Sie das Beispiel Projekt ausführen möchten, müssen Sie einen Azure Active Directory B2C-Mandanten erstellen. Weitere Informationen finden Sie unter [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](/azure/active-directory-b2c/active-directory-b2c-get-started/).

Nachdem Sie einen Mandanten erstellt haben, benötigen Sie den Mandanten **Namen** und die Mandanten- **ID** , um die Mobile Anwendung zu konfigurieren. Die Mandanten-ID und der Name werden von der Domäne definiert, die beim Erstellen der Mandanten-URL generiert wurde. Wenn `https://contoso20190410tenant.onmicrosoft.com/` die generierte Mandanten `contoso20190410tenant.onmicrosoft.com` -URL die Mandanten- **ID** ist und der Mandanten **Name** `contoso20190410tenant`lautet. Suchen Sie die Mandanten Domäne im Azure-Portal, indem Sie im oberen Menü auf den **Filter Verzeichnis und Abonnement** klicken. Der folgende Screenshot zeigt die Filter Schaltfläche für Azure-Verzeichnisse und-Abonnements und die Mandanten Domäne:

[![Mandanten Name in der Azure-Verzeichnis-und Abonnement Filter Ansicht](azure-ad-b2c-images/azure-tenant-name-cropped.png)](azure-ad-b2c-images/azure-tenant-name.png#lightbox)

Bearbeiten Sie im Beispiel Projekt die Datei **Constants.cs** , und legen Sie `tenantName` die `tenantId` Felder und fest. Der folgende Code zeigt, wie diese Werte festgelegt werden sollten, wenn Ihre `https://contoso20190410tenant.onmicrosoft.com/`Mandanten Domäne ist. ersetzen Sie diese Werte durch Werte aus dem Portal:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    ...
}
```

## <a name="register-your-mobile-application-with-azure-active-directory-b2c"></a>Registrieren Ihrer mobilen Anwendung bei Azure Active Directory B2C

Eine mobile Anwendung muss beim Mandanten registriert werden, bevor Sie eine Verbindung herstellen und Benutzer authentifizieren kann. Der Registrierungsprozess weist der Anwendung eine eindeutige **Anwendungs-ID** und eine **Umleitungs-URL** zu, die Antworten nach der Authentifizierung an die Anwendung zurück leitet. Weitere Informationen finden [Sie unter Azure Active Directory B2C: Registrieren Sie Ihre](/azure/active-directory-b2c/active-directory-b2c-app-registration/)Anwendung. Sie müssen die **Anwendungs-ID** kennen, die Ihrer Anwendung zugewiesen ist, die nach dem Anwendungsnamen in der Eigenschaften Ansicht aufgeführt wird. Der folgende Screenshot zeigt, wo Sie die Anwendungs-ID finden:

[![Anwendungs-ID in der Azure-Anwendungseigenschaften Ansicht](azure-ad-b2c-images/azure-application-id-cropped.png)](azure-ad-b2c-images/azure-application-id.png#lightbox)

Die Microsoft-Authentifizierungs Bibliothek erwartet, dass die **Umleitungs-URL** für Ihre Anwendung ihrer **Anwendungs-ID** als Präfix mit dem Text "msal" vorangestellt ist, gefolgt von einem Endpunkt namens "auth". Wenn Ihre Anwendungs-ID "1234ABCD" lautet, sollte die vollständige `msal1234abcd://auth`URL lauten. Stellen Sie sicher, dass Ihre Anwendung die **Native Client** -Einstellung aktiviert hat, und erstellen Sie mithilfe ihrer Anwendungs-ID wie im folgenden Screenshot gezeigt einen **benutzerdefinierten Umleitungs-URI** :

![Benutzerdefinierter Umleitungs-URI in der Azure-Anwendungseigenschaften Ansicht](azure-ad-b2c-images/azure-redirect-uri.png)

Die URL wird später in der Datei " **ApplicationManifest. XML** " und in der Datei "IOS **Info. plist**" verwendet.

Bearbeiten Sie im Beispiel Projekt die Datei **Constants.cs** , und legen Sie `clientId` das Feld auf Ihre **Anwendungs-ID**fest. Der folgende Code zeigt, wie dieser Wert festgelegt werden sollte, wenn Ihre `1234abcd`Anwendungs-ID lautet:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    ...
}
```

## <a name="create-sign-up-and-sign-in-policies-and-forgot-password-policies"></a>Erstellen von Registrierungs-und Anmelde Richtlinien und vergessen von Kenn Wort Richtlinien

Eine Richtlinie ist eine Benutzerumgebung, die Benutzer durchlaufen, um eine Aufgabe wie das Erstellen eines Kontos oder das Zurücksetzen eines Kennworts abzuschließen. Eine Richtlinie gibt auch den Inhalt von Token an, die die Anwendung empfängt, wenn der Benutzer aus der Benutzerfunktion zurückkehrt. Sie müssen Richtlinien für die Registrierung und Anmeldung von Konten einrichten und das Kennwort zurücksetzen. Azure verfügt über integrierte Richtlinien, die die Erstellung von allgemeinen Richtlinien vereinfachen. Weitere Informationen finden [Sie unter Azure Active Directory B2C: Integrierte Richtlinien](/azure/active-directory-b2c/active-directory-b2c-reference-policies/)

Wenn Sie das Einrichten der Richtlinie abgeschlossen haben, sollten Sie über zwei Richtlinien in der Ansicht **benutzerflows (Richtlinien)** im Azure-Portal verfügen. Der folgende Screenshot zeigt zwei konfigurierte Richtlinien in der Azure-Portal:

![Zwei konfigurierte Richtlinien in der Ansicht "Azure-Benutzer Abläufe" (Richtlinien)](azure-ad-b2c-images/azure-application-policies.png)

Bearbeiten Sie im Beispiel Projekt die Datei **Constants.cs** , `policySignin` und legen Sie die `policyPassword` Felder und so fest, dass Sie die beim Einrichten der Richtlinie ausgewählten Namen widerspiegeln:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    static readonly string policySignin = "B2C_1_signupsignin1";
    static readonly string policyPassword = "B2C_1_passwordreset";
    ...
}
```

## <a name="use-the-microsoft-authentication-library-msal-for-authentication"></a>Verwenden der Microsoft Authentication Library (msal) für die Authentifizierung

Das nuget-Paket der Microsoft Authentication Library (msal) muss dem freigegebenen Projekt, .NET Standard Projekt und den Platt Form Projekten in einer xamarin. Forms-Projekt Mappe hinzugefügt werden. Msal enthält eine `PublicClientApplicationBuilder` -Klasse, die ein Objekt erstellt, `IPublicClientApplication` das der-Schnittstelle entspricht. Msal verwendet `With` Klauseln, um zusätzliche Parameter für den Konstruktor und die Authentifizierungsmethoden bereitzustellen.

Im Beispiel Projekt definiert der Code Behind für **app. XAML** statische Eigenschaften mit dem Namen `AuthenticationClient` und `UIParent`und instanziiert das `AuthenticationClient` Objekt im Konstruktor. Die `WithIosKeychainSecurityGroup` -Klausel stellt einen Sicherheitsgruppen Namen für IOS-Anwendungen bereit. Die `WithB2CAuthority` -Klausel stellt die Standard **Autorität**oder-Richtlinie bereit, die zum Authentifizieren von Benutzern verwendet wird. Im folgenden Beispiel wird veranschaulicht, wie das `PublicClientApplication`instanziiert wird:

```csharp
public partial class App : Application
{
    public static IPublicClientApplication AuthenticationClient { get; private set; }

    public static object UIParent { get; set; } = null;

    public App()
    {
        InitializeComponent();

        AuthenticationClient = PublicClientApplicationBuilder.Create(Constants.ClientId)
            .WithIosKeychainSecurityGroup(Constants.IosKeychainSecurityGroups)
            .WithB2CAuthority(Constants.AuthoritySignin)
            .Build();

        MainPage = new NavigationPage(new LoginPage());
    }

    ...
```

Der `OnAppearing` -Ereignishandler im **LoginPage.XAML.cs** -Code Behind `AcquireTokenSilentAsync` Ruft auf, um das Authentifizierungs Token für Benutzer zu aktualisieren, die sich zuvor angemeldet haben. Der Authentifizierungsprozess wird bei erfolgreicher Umleitung `LogoutPage` an den umgeleitet und hat bei einem Fehler keinen Erfolg. Das folgende Beispiel zeigt den automatischen erneuten Authentifizierungsprozess in `OnAppearing`:

```csharp
public partial class LoginPage : ContentPage
{
    ...

    protected override async void OnAppearing()
    {
        try
        {
            // Look for existing account
            IEnumerable<IAccount> accounts = await App.AuthenticationClient.GetAccountsAsync();

            AuthenticationResult result = await App.AuthenticationClient
                .AcquireTokenSilent(Constants.Scopes, accounts.FirstOrDefault())
                .ExecuteAsync();

            await Navigation.PushAsync(new LogoutPage(result));
        }
        catch
        {
            // Do nothing - the user isn't logged in
        }
        base.OnAppearing();
    }

    ...
}
```

Der `OnLoginButtonClicked` Ereignishandler (wird beim Klicken auf die Anmelde Schaltfläche `AcquireTokenAsync`ausgelöst) aufruft. Die msal-Bibliothek öffnet automatisch den Browser für mobile Geräte und navigiert zur Anmeldeseite. Die Anmelde-URL, die als **Autorität**bezeichnet wird, ist eine Kombination aus dem Mandanten Namen und den Richtlinien, die in der Datei **Constants.cs** definiert sind. Wenn der Benutzer die Option "Kennwort vergessen" auswählt, wird er mit einer Ausnahme an die APP zurückgegeben. Dadurch wird die Benutzer Kennwort vergessen. Das folgende Beispiel zeigt den Authentifizierungsprozess:

```csharp
public partial class LoginPage : ContentPage
{
    ...

    async void OnLoginButtonClicked(object sender, EventArgs e)
    {
        AuthenticationResult result;
        try
        {
            result = await App.AuthenticationClient
                .AcquireTokenInteractive(Constants.Scopes)
                .WithPrompt(Prompt.SelectAccount)
                .WithParentActivityOrWindow(App.UIParent)
                .ExecuteAsync();
    
            await Navigation.PushAsync(new LogoutPage(result));
        }
        catch (MsalException ex)
        {
            if (ex.Message != null && ex.Message.Contains("AADB2C90118"))
            {
                result = await OnForgotPassword();
                await Navigation.PushAsync(new LogoutPage(result));
            }
            else if (ex.ErrorCode != "authentication_canceled")
            {
                await DisplayAlert("An error has occurred", "Exception message: " + ex.Message, "Dismiss");
            }
        }
    }

    ...
}
```

Die `OnForgotPassword` -Methode ähnelt dem Anmeldevorgang, implementiert aber eine benutzerdefinierte Richtlinie. `OnForgotPassword`verwendet eine andere Überladung `AcquireTokenAsync`von, die es Ihnen ermöglicht, eine bestimmte **Autorität**bereitzustellen. Im folgenden Beispiel wird gezeigt, wie eine benutzerdefinierte **Autorität** beim Abrufen eines Tokens bereitgestellt wird:

```csharp
public partial class LoginPage : ContentPage
{
    ...
    async Task<AuthenticationResult> OnForgotPassword()
    {
        try
        {
            return await App.AuthenticationClient
                .AcquireTokenInteractive(Constants.Scopes)
                .WithPrompt(Prompt.SelectAccount)
                .WithParentActivityOrWindow(App.UIParent)
                .WithB2CAuthority(Constants.AuthorityPasswordReset)
                .ExecuteAsync();
        }
        catch (MsalException)
        {
            // Do nothing - ErrorCode will be displayed in OnLoginButtonClicked
            return null;
        }
    }
}
```

Der letzte Teil der Authentifizierung ist der Abmeldevorgang. Die `OnLogoutButtonClicked` -Methode wird aufgerufen, wenn der Benutzer die Schaltfläche Abmelden drückt. Sie durchläuft alle Konten und stellt sicher, dass Ihre Token für ungültig erklärt wurden. Das folgende Beispiel veranschaulicht die Abmelde Implementierung:

```csharp
public partial class LogoutPage : ContentPage
{
    ...
    async void OnLogoutButtonClicked(object sender, EventArgs e)
    {
        IEnumerable<IAccount> accounts = await App.AuthenticationClient.GetAccountsAsync();

        while (accounts.Any())
        {
            await App.AuthenticationClient.RemoveAsync(accounts.First());
            accounts = await App.AuthenticationClient.GetAccountsAsync();
        }

        await Navigation.PopAsync();
    }
}
```

### <a name="ios"></a>iOS

Unter IOS muss das benutzerdefinierte URL-Schema, das bei Azure Active Directory B2C registriert wurde, in " **Info. plist**" registriert werden. Msal erwartet, dass das URL-Schema einem bestimmten Muster entspricht, das weiter oben unter [Registrieren der mobilen Anwendung bei Azure Active Directory B2C](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)beschrieben wird. Der folgende Screenshot zeigt das benutzerdefinierte URL-Schema in " **Info. plist**".

!["Registrieren eines benutzerdefinierten URL-Schemas unter IOS"](azure-ad-b2c-images/customurl-ios.png)

Msal erfordert auch Keychain-Berechtigungen für IOS, die in der **entitilements. plist**registriert sind, wie im folgenden Screenshot zu sehen:

!["Festlegen von Anwendungs Berechtigungen für IOS"](azure-ad-b2c-images/entitlements-ios.png)

Wenn Azure Active Directory B2C die autorisierungsanforderung abgeschlossen ist, erfolgt eine Umleitung an die registrierte umleitungs-URL. Das benutzerdefinierte URL-Schema führt dazu, dass IOS die Mobile Anwendung startet und die URL als Startparameter übergibt, wo Sie von der `OpenUrl` Überschreibung der `AppDelegate` Anwendungsklasse verarbeitet wird, und gibt die Steuerung der Benutzeroberflächen Funktion an msal zurück. Die `OpenUrl` -Implementierung wird im folgenden Codebeispiel gezeigt:

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
            return base.OpenUrl(app, url, options);
        }
    }
}
```

### <a name="android"></a>Android

Unter Android muss das benutzerdefinierte URL-Schema, das bei Azure Active Directory B2C registriert wurde, in der Datei " **androidmanifest. XML**" registriert werden. Msal erwartet, dass das URL-Schema einem bestimmten Muster entspricht, das weiter oben unter [Registrieren der mobilen Anwendung bei Azure Active Directory B2C](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c)beschrieben wird. Das folgende Beispiel zeigt das benutzerdefinierte URL-Schema in der Datei " **androidmanifest. XML**".

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="com.xamarin.adb2cauthorization">
  <uses-sdk android:minSdkVersion="15" />
  <application android:label="ADB2CAuthorization">
    <activity android:name="microsoft.identity.client.BrowserTabActivity">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="INSERT_URI_SCHEME_HERE" android:host="auth" />"
      </intent-filter>
    </activity>"
  </application>
</manifest>
```

Die `MainActivity` -Klasse muss geändert werden, damit `UIParent` das-Objekt während des `OnCreate` Aufrufes der Anwendung bereitgestellt wird. Wenn Azure Active Directory B2C die Autorisierungs Anforderung abschließt, wird Sie von der Datei " **androidmanifest. XML**" an das registrierte URL-Schema umgeleitet. Das registrierte URI-Schema führt dazu, dass `OnActivityResult` Android die-Methode mit der URL als Startparameter aufruft, wo Sie `SetAuthenticationContinuationEventArgs` von der-Methode verarbeitet wird.

```csharp
public class MainActivity : FormsAppCompatActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        LoadApplication(new App());
        App.UIParent = this;
    }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
    }
}
```

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Für die Verwendung von msal auf dem universelle Windows-Plattform ist keine weitere Installation erforderlich.

## <a name="run-the-project"></a>Ausführen des Projekts

Führen Sie die Anwendung auf einem virtuellen oder physischen Gerät aus. Wenn Sie auf die **Anmelde** Schaltfläche tippen, sollten Sie den Browser öffnen und zu einer Seite navigieren, auf der Sie sich anmelden oder ein Konto erstellen können. Nachdem Sie den Anmeldevorgang abgeschlossen haben, sollten Sie zur Abmelde Seite der Anwendung zurückkehren. Der folgende Screenshot zeigt den Benutzer Anmeldebildschirm, der unter Android und IOS ausgeführt wird:

!["Azure ADB2C Sign in Screen on Android and IOS"](azure-ad-b2c-images/login.png)

## <a name="related-links"></a>Verwandte Links

- [AzureADB2CAuth (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azureadb2cauth)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft-Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Dokumentation der Microsoft-Authentifizierungs Bibliothek](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
