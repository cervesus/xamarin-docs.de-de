---
title: Authentifizieren von Benutzern mit Azure Active Directory B2C
description: Azure Active Directory B2C bietet cloudidentitätsverwaltung für kundenorientierte Web- und mobile Anwendungen. In diesem Artikel wird gezeigt, wie Sie Azure Active Directory B2C, um die identitätsverwaltung in eine mobile Anwendung mit der Microsoft-Authentifizierungsbibliothek zu integrieren.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2019
ms.openlocfilehash: db44e09e9caa5c35a5e107cfed80d1d30fd7eb7d
ms.sourcegitcommit: 864f47c4f79fa588b65ff7f721367311ff2e8f8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "64347127"
---
# <a name="authenticate-users-with-azure-active-directory-b2c"></a>Authentifizieren von Benutzern mit Azure Active Directory B2C

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)

_Azure Active Directory B2C bietet cloudidentitätsverwaltung für kundenorientierte Web- und mobile Anwendungen. In diesem Artikel wird gezeigt, wie Sie Azure Active Directory B2C, um die identitätsverwaltung in eine mobile Anwendung mit der Microsoft-Authentifizierungsbibliothek zu integrieren._

## <a name="overview"></a>Übersicht

Azure Active Directory B2C (ADB2C) ist ein identitätsverwaltungsdienst für kundenorientierte Anwendungen. Damit können Benutzer Ihre Anwendung mit ihren vorhandenen Konten sozialer Netzwerke oder benutzerdefinierten Anmeldeinformationen, z. B. e-Mail-Adresse oder Benutzername und Kennwort anmelden. Benutzerdefinierte Anmeldeinformationen Konten werden als bezeichnet _lokalen_ Konten.

Der Prozess für die Integration der Azure Active Directory B2C-Identity-Management-Dienst in einer mobilen Anwendung lautet wie folgt aus:

1. Erstellen eines Azure Active Directory B2C-Mandanten
1. Registrieren Sie Ihre mobile Anwendung mit dem Azure Active Directory B2C-Mandanten
1. Richtlinien für die Registrierung und Anmeldung zu erstellen und benutzerabläufe Kennwort vergessen
1. Verwenden Sie die Microsoft Authentication Library (MSAL), um einen authentifizierungsworkflow bei Ihrem Azure Active Directory B2C-Mandanten zu starten.

> [!NOTE]
> Azure Active Directory B2C unterstützt mehrere Identitätsanbieter, einschließlich Microsoft, GitHub, Facebook, Twitter und mehr. Weitere Informationen zu Azure Active Directory B2C-Funktionen, finden Sie unter [Dokumentation zu Azure Active Directory B2C](/azure/active-directory-b2c/).
>
> Microsoft Authentication Library unterstützt mehrere Architekturen und Plattformen. Informationen zu den MSAL-Funktionen finden Sie unter [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) auf GitHub.

## <a name="configure-an-azure-active-directory-b2c-tenant"></a>Konfigurieren Sie ein Azure Active Directory B2C-Mandanten

Um das Beispielprojekt ausführen zu können, müssen Sie einen Azure Active Directory B2C-Mandanten erstellen. Weitere Informationen finden Sie unter [Erstellen eines Azure Active Directory B2C-Mandanten im Azure-Portal](/azure/active-directory-b2c/active-directory-b2c-get-started/).

Nachdem Sie einen Mandanten erstellen, müssen Sie die **Mandantenname** und **Mandanten-ID** zum Konfigurieren der mobilen Anwendung. Die Mandanten-ID und Name werden definiert, von der Domäne generiert, wenn Sie Ihre Mandanten-URL erstellt haben. Wenn Ihre generierten Mandanten-URL ist `https://contoso20190410tenant.onmicrosoft.com/` der **Mandanten-ID** ist `contoso20190410tenant.onmicrosoft.com` und **Mandantenname** ist `contoso20190410tenant`. Die mandantendomäne im Azure-Portal zu finden, indem Sie auf die **Verzeichnis- und Abonnementfilter** in der oberen Menüleiste. Der folgende Screenshot zeigt das Azure-Verzeichnis und die Schaltfläche "Filter" Abonnement und die mandantendomäne:

[![Mandantennamen in der Azure Directory und Abonnements filtern](azure-ad-b2c-images/azure-tenant-name-cropped.png)](azure-ad-b2c-images/azure-tenant-name.png#lightbox)

Bearbeiten Sie in das Beispielprojekt, das **"Constants.cs"** hinzu, um einzurichten der `tenantName` und `tenantId` Felder. Der folgende Code zeigt wie die folgenden Werte festgelegt werden soll, ist Ihre mandantendomäne `https://contoso20190410tenant.onmicrosoft.com/`, ersetzen Sie diese Werte mit Werten aus dem Portal:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    ...
}
```

## <a name="register-your-mobile-application-with-azure-active-directory-b2c"></a>Registrieren Sie Ihre mobile Anwendung mit Azure Active Directory B2C

Eine mobile Anwendung muss mit dem Mandanten registriert werden, bevor er eine Verbindung herstellen und Benutzer authentifizieren kann. Während der Registrierung weist eine eindeutige **Anwendungs-ID** an die Anwendung, und ein **Umleitungs-URL** , die Antworten zurück an die Anwendung nach der Authentifizierung weiterleitet. Weitere Informationen finden Sie unter [Azure Active Directory B2C: Registrieren Sie Ihre Anwendung](/azure/active-directory-b2c/active-directory-b2c-app-registration/). Sie müssen wissen, die **Anwendungs-ID** zugewiesen, die für Ihre Anwendung, die nach dem Anwendungsnamen in der Eigenschaftenansicht angezeigt wird. Der folgende Screenshot zeigt, wo Sie die Anwendungs-ID finden:

[![Anwendungs-ID in der Eigenschaftenansicht Azure-Anwendung](azure-ad-b2c-images/azure-application-id-cropped.png)](azure-ad-b2c-images/azure-application-id.png#lightbox)

Microsoft Authentication Library erwartet, dass die **Umleitungs-URL** für Ihre Anwendung Ihre **Anwendungs-ID** mit dem Text "Msal" das Präfix und gefolgt von einem Endpunkt namens "Auth". Wenn Ihre Anwendungs-ID "1234abcd" ist, muss die vollständige URL `msal1234abcd://auth`. Stellen Sie sicher, dass Ihre Anwendung aktiviert hat die **Native Client** festlegen, und erstellen Sie eine **benutzerdefinierte Umleitungs-URI** Ihre Anwendungs-ID verwenden, wie im folgenden Screenshot gezeigt:

![Benutzerdefinierte Umleitungs-URI in der Eigenschaftenansicht Azure-Anwendung](azure-ad-b2c-images/azure-redirect-uri.png)

Die URL wird weiter unten in der sowohl die Android **"ApplicationManifest.xml"** und den iOS- **"Info.plist"**.

Bearbeiten Sie in das Beispielprojekt der **"Constants.cs"** Datei Festlegen der `clientId` Feld Ihre **Anwendungs-ID**. Der folgende Code zeigt wie dieser Wert festgelegt werden soll, wenn Ihre Anwendungs-ID ist `1234abcd`:

```csharp
public static class Constants
{
    static readonly string tenantName = "contoso20190410tenant";
    static readonly string tenantId = "contoso20190410tenant.onmicrosoft.com";
    static readonly string clientId = "1234abcd";
    ...
}
```

## <a name="create-sign-up-and-sign-in-policies-and-forgot-password-policies"></a>Registrierung und Anmeldung Richtlinien erstellen und vergessen Sie Kennwortrichtlinien

Eine Richtlinie ist eine Benutzeroberfläche, die Benutzer zum Abschluss einer Aufgabe z. B. Erstellen eines Kontos oder Zurücksetzen eines Kennworts durchlaufen. Eine Richtlinie gibt auch den Inhalt der Token, die die Anwendung empfängt, wenn der Benutzer aus dieser Erfahrung zurückkehrt. Sie müssen Richtlinien für beide Konto einrichten, Registrierung und Anmeldung und kennwortzurücksetzung. Azure verfügt über integrierte Richtlinien, die die Erstellung von allgemeinen Richtlinien zu vereinfachen. Weitere Informationen finden Sie unter [Azure Active Directory B2C: Integrierte Richtlinien](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

Wenn Sie die Einrichtung abgeschlossen haben, sollten Sie zwei Richtlinien verfügen, in der **benutzerabläufe (Richtlinien)** Ansicht im Azure-Portal. Der folgende Screenshot zeigt zwei konfigurierte Richtlinien im Azure-Portal:

![Anzeigen von zwei konfigurierten Richtlinien in der Azure-benutzerflows (Richtlinien)](azure-ad-b2c-images/azure-application-policies.png)

Bearbeiten Sie in das Beispielprojekt der **"Constants.cs"** Datei Festlegen der `policySignin` und `policyPassword` Felder entsprechend die Namen, Sie während der Einrichtung ausgewählt haben:

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

## <a name="use-the-microsoft-authentication-library-msal-for-authentication"></a>Verwenden Sie die Microsoft Authentication Library (MSAL) für die Authentifizierung

Das Microsoft Authentication Library (MSAL) NuGet-Paket muss die freigegebene .NET Standard-Projekt und die Plattformprojekte in einer Xamarin.Forms-Projektmappe hinzugefügt werden. MSAL bietet eine `PublicClientApplication` zum Vereinfachen der Authentifizierung mit Azure Active Directory B2C. In den Code für das Beispielprojekt **"App.xaml"** definiert statische Eigenschaften für ein `AuthenticationClient` und `UiParent` und instanziiert die `AuthenticationClient` im Konstruktor. Der zweite Parameter bereitgestellt, um die `PublicClientApplication` ist die Standardeinstellung **Autorität**, oder der Richtlinie, die zum Authentifizieren von Benutzern verwendet werden. Im folgende Beispiel wird veranschaulicht, wie zum Instanziieren der `PublicClientApplication`:

```csharp
public partial class App : Application
{
    public static PublicClientApplication AuthenticationClient { get; private set; }

    public static UIParent UiParent { get; set; } = null;

    public App()
    {
        InitializeComponent();
        AuthenticationClient = new PublicClientApplication(Constants.ClientId, Constants.AuthoritySignin);
        MainPage = new NavigationPage(new LoginPage());
    }

    ...
```

Die `OnAppearing` -Ereignishandler in der **LoginPage.xaml.cs** CodeBehind-Aufrufe `AcquireTokenSilentAsync` das Authentifizierungstoken für Benutzer zu aktualisieren, die vor dem angemeldet haben. Der Authentifizierungsprozess leitet an die `LogoutPage` im Erfolgsfall und führt keine Aktion bei einem Fehler. Das folgende Beispiel zeigt den Prozess der automatischen erneuten Authentifizierung in `OnAppearing`:

```csharp
public partial class LoginPage : ContentPage
{
    ...

    protected override async void OnAppearing()
    {
        try
        {
            IEnumerable<IAccount> accounts = await App.AuthenticationClient.GetAccountsAsync();

            AuthenticationResult result = await App.AuthenticationClient.AcquireTokenSilentAsync(
                Constants.Scopes,
                accounts.FirstOrDefault());
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

Die `OnLoginButtonClicked` -Ereignishandler (wird ausgelöst, wenn auf die Schaltfläche "Anmelden" geklickt wird) Aufrufe `AcquireTokenAsync`. Die MSAL-Bibliothek öffnet der Browser für mobile Geräte automatisch und navigiert zur Anmeldeseite. Die Anmelde-URL, dem Namen einer **Autorität**, ist eine Kombination aus den Namen des Mandanten und Richtlinien definiert, der **"Constants.cs"** Datei. Bei Einmaliger Betätigung der vergessen Password-Option, diese zurückgegeben werden, für die app mit einer Ausnahme aus, die startet die Benutzeroberfläche für das Kennwort vergessen. Das folgende Beispiel zeigt den Authentifizierungsprozess:

```csharp
public partial class LoginPage : ContentPage
{
    ...

    async void OnLoginButtonClicked(object sender, EventArgs e)
    {
        AuthenticationResult result;
        try
        {
            result = await App.AuthenticationClient.AcquireTokenAsync(
                Constants.Scopes,
                string.Empty,
                UIBehavior.SelectAccount,
                string.Empty,
                App.UiParent);
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

Die `OnForgotPassword` Methode ist vergleichbar mit der Anmeldevorgang aber eine benutzerdefinierte Richtlinie implementiert. `OnForgotPassword` verwendet eine andere Überladung der `AcquireTokenAsync`, sodass Sie eine bestimmte **Autorität**. Das folgende Beispiel zeigt, wie Sie eine benutzerdefinierte angeben **Autorität** , wenn ein Token anfordern:

```csharp
public partial class LoginPage : ContentPage
{
    ...
    async Task<AuthenticationResult> OnForgotPassword()
    {
        try
        {
            return await App.AuthenticationClient.AcquireTokenAsync(
                Constants.Scopes,
                string.Empty,
                UIBehavior.SelectAccount,
                string.Empty,
                null,
                Constants.AuthorityPasswordReset,
                App.UiParent
                );
        }
        catch (MsalException)
        {
            // Do nothing - ErrorCode will be displayed in OnLoginButtonClicked
            return null;
        }
    }
}
```

Das letzte Teil der Authentifizierung ist die Abmelde-Prozess. Die `OnLogoutButtonClicked` Methode wird aufgerufen, wenn der Benutzer die Abmeldeschaltfläche drückt. Es durchläuft alle Konten und stellt sicher, dass deren Token ungültig. Das folgende Beispiel veranschaulicht die Abmelde-Implementierung:

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

Unter iOS, muss die benutzerdefinierte URL-Schema, die mit Azure Active Directory B2C registriert wurde im registriert werden **"Info.plist"**. MSAL erwartet, dass das URL-Schema entsprechen einem bestimmten Muster, die im zuvor beschriebenen [registrieren Sie Ihre mobile Anwendung mit Azure Active Directory B2C](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c). Der folgende Screenshot zeigt die benutzerdefinierte URL-Schema in **"Info.plist"**.

!["Registrieren eines benutzerdefinierten URL-Schemas unter iOS"](azure-ad-b2c-images/customurl-ios.png)

MSAL erfordert auch Schlüsselbundberechtigungen die unter iOS, die registriert wird, der **Entitilements.plist**, wie im folgenden Screenshot gezeigt:

!["Festlegen von Anwendungsberechtigungen unter iOS"](azure-ad-b2c-images/entitlements-ios.png)

Wenn Azure Active Directory B2C die autorisierungsanforderung abgeschlossen ist, erfolgt eine Umleitung an die registrierte umleitungs-URL. Die benutzerdefinierte URL-Schema führt zu iOS die mobile Anwendung zu starten, und übergeben in der URL als Parameter starten, in dem sie vom verarbeitet wird die `OpenUrl` außer Kraft setzen, von der Anwendung `AppDelegate` -Klasse, und gibt die Steuerung der msal-Umgebung. Die `OpenUrl` -Implementierung wird im folgenden Codebeispiel dargestellt:

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

Unter Android, muss die benutzerdefinierte URL-Schema, die mit Azure Active Directory B2C registriert wurde in registriert werden die **"androidmanifest.xml"**. MSAL erwartet, dass das URL-Schema entsprechen einem bestimmten Muster, die im zuvor beschriebenen [registrieren Sie Ihre mobile Anwendung mit Azure Active Directory B2C](/docs/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md#register-your-mobile-application-with-azure-active-directory-b2c). Das folgende Beispiel zeigt die benutzerdefinierte URL-Schema in der **"androidmanifest.xml"**.

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

Die `MainActivity` Klasse muss geändert werden, um zu ermöglichen die `UiParent` an die Anwendung während der `OnCreate` aufrufen. Wenn Azure Active Directory B2C die autorisierungsanforderung abgeschlossen ist, leitet Sie der registrierten URL-Schema aus der **"androidmanifest.xml"**. Das registrierte URI-Schema führt Android Aufrufen `OnActivityResult` mit der URL als Parameter starten, in dem vom verarbeitet die `SetAuthenticationContinuationEventArgs`.

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
        App.UiParent = new UIParent(this);
    }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {
        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
    }
}
```

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Ohne zusätzliche Einrichtung ist erforderlich, um MSAL verwenden, für die universelle Windows-Plattform

## <a name="run-the-project"></a>Ausführen des Projekts

Führen Sie die Anwendung auf einem virtuellen oder physischen Gerät. Durch Tippen auf die **Anmeldung** Schaltfläche Öffnen Sie den Browser und navigieren Sie zu einer Seite, in dem Sie anmelden oder ein Konto erstellen können, sollte. Nach Abschluss des Anmeldeprozesses, sollte Sie Abmeldeseite der Anwendung zurückgegeben werden. Der folgende Screenshot zeigt die Benutzeranmeldung im Bildschirm auf Android- und iOS ausgeführt wird:

!["Azure ADB2C Anmeldebildschirm unter Android und iOS"](azure-ad-b2c-images/login.png)

## <a name="related-links"></a>Verwandte Links

- [AzureADB2CAuth (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Microsoft-Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Microsoft Authentication Library-Dokumentation](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)
