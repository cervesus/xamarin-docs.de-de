---
title: Die Integration von Azure Active Directory B2C mit Azure Mobile Apps
description: Azure Active Directory B2C ist eine Cloudlösung für die Verwaltung von Identitäten für kundenorientierte Web- und mobile Anwendungen. In diesem Artikel wird veranschaulicht, wie Sie Azure Active Directory B2C zum Bereitstellen von Authentifizierung und Autorisierung mit einer Instanz von Azure Mobile Apps mit Xamarin.Forms zu verwenden.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2019
ms.openlocfilehash: 4c3cc489f7c65636ad9f2b5541e552665b10980e
ms.sourcegitcommit: 0cb62b02a7efb5426f2356d7dbdfd9afd85f2f4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2019
ms.locfileid: "65557263"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Die Integration von Azure Active Directory B2C mit Azure Mobile Apps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)

_Azure Active Directory B2C bietet cloudidentitätsverwaltung für kundenorientierte Anwendungen. In diesem Artikel wird erläutert, wie Sie Azure Active Directory B2C, um die identitätsverwaltung in eine Azure Mobile Apps-Instanz mit Xamarin.Forms zu integrieren._

## <a name="overview"></a>Übersicht

Azure Mobile Apps können Sie ein skalierbares Back-End in Azure App Service gehosteten Anwendungen entwickeln. Mobile Authentifizierung, offlinesynchronisierung und Pushbenachrichtigungen unterstützt Benachrichtigungen. Weitere Informationen zu Azure Mobile Apps, finden Sie unter [Nutzung von Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md), und [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure Active Directory B2C ist ein identitätsverwaltungsdienst für kundenorientierte Anwendungen, die Benutzern ermöglicht wird:

- Melden Sie sich mit ihren vorhandenen Konten sozialer Netzwerke (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Erstellen Sie neue Anmeldeinformationen (e-Mail-Adresse und Kennwort oder Benutzername und Kennwort). Diese Anmeldeinformationen werden als bezeichnet *lokalen* Konten.

Weitere Informationen zu Azure Active Directory B2C, finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure Active Directory B2C kann verwendet werden, um den Workflow der Authentifizierung für Azure Mobile Apps zu verwalten. Dieser Ansatz identitätsverwaltung in der Cloud konfiguriert und kann Änderungen ohne Änderungen am Anwendungscode.


Azure Mobile Apps mit Azure Active Directory B2C ermöglicht zwei Authentifizierungsmethoden:

- [Clientgesteuerte](#client-managed-authentication) – die mobilen Xamarin.Forms-Anwendung startet den Authentifizierungsprozess mit dem Azure Active Directory B2C-Mandanten, und übergibt das empfangene Authentifizierungstoken an die Azure Mobile Apps-Instanz.
- [Vom Server verwaltete](#server-managed-authentication) – die Azure Mobile Apps-Instanz verwendet den Azure Active Directory B2C-Mandanten des Authentifizierungsprozesses durch einen Web-basierten Workflow zu starten.

In beiden Fällen wird der Authentifizierungsvorgang durch den Azure Active Directory B2C-Mandanten bereitgestellt. In der beispielanwendung sollte der Anmeldebildschirm in den folgenden Screenshots ähneln:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Anmeldeseite")

In diesem Beispiel wird gezeigt, melden Sie sich mit einem lokalen Konto, aber Sie können auch aktivieren, Microsoft, Google, Facebook und anderer Identitätsanbieter entgegen.

## <a name="setup"></a>Setup

In beiden Workflows lautet der erste Prozess für die Integration in eines Azure Active Directory B2C-Mandantenverwaltungs mit Azure Mobile Apps:

1. Erstellen Sie eine Azure Mobile Apps-Instanz im Azure-Portal an. Dadurch wird eine ähnliche zum Beispielprojekt Projekt beginnen. Weitere Informationen finden Sie unter [Nutzung von Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Erstellen eines Azure Active Directory B2C-Mandanten. Weitere Informationen finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).
1. Aktivieren der Authentifizierung in der Azure Mobile Apps-Instanz und die Xamarin.Forms-Anwendung. Weitere Informationen finden Sie unter [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

> [!NOTE]
> Der Microsoft Authentication Library (MSAL) ist erforderlich, wenn Sie einen Workflow Client verwaltete Authentifizierung verwenden. MSAL verwendet den Webbrowser des Geräts zum Authentifizieren. Nach Abschluss der Authentifizierung im Browser wird der Benutzer eine benutzerdefinierte URL-Schema umgeleitet. Die Anwendung registriert die benutzerdefinierte URL-Schema, sodass sie wieder die Kontrolle über die Benutzeroberfläche können von und reagieren auf die Authentifizierung. Weitere Informationen zur Verwendung von MSAL für die Kommunikation mit einem Azure Active Directory B2C-Mandanten finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

## <a name="client-managed-authentication"></a>Vom Client verwaltete Authentifizierung

Bei der vom Client verwaltete Authentifizierung kontaktiert eine Xamarin.Forms-Anwendung ein Azure Active Directory B2C-Mandanten zum Ablauf der Authentifizierung zu starten. Nach dem Anmelden gibt der Azure Active Directory B2C-Mandanten ein Identitätstoken, die mit der Azure Mobile Apps-Instanz bereitgestellt wird. Dadurch kann es sich um die Xamarin.Forms-Anwendung für Aktionen, die Berechtigungen eines authentifizierten Benutzers erfordern.

### <a name="azure-active-directory-b2c-client-managed-tenant-configuration"></a>Azure Active Directory B2C-Mandanten vom Client verwaltete-Konfiguration

Der Azure Active Directory B2C-Mandant sollte für einen Workflow Client verwaltete Authentifizierung wie folgt konfiguriert werden:

- Legen Sie **nativen Client einschließen** auf "Ja".
- Legen Sie den Umleitungs-URI. Die MSAL-Dokumentation empfiehlt, mithilfe von "Msal" zusammen mit der App-ID und gefolgt von ":/ / Auth /". Weitere Informationen finden Sie unter [MSAL-Clientanwendungen umleitungs-URI](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Client-Applications#redirect-uri).

Der folgende Screenshot veranschaulicht diese Konfiguration:

[!["Azure Active Directory B2C-Konfiguration"](azure-ad-b2c-mobile-app-images/azure-redirect-uri-cropped.png)](azure-ad-b2c-mobile-app-images/azure-redirect-uri.png#lightbox "Azure Active Directory B2C-Konfiguration")

Die Richtlinie, die in den Azure Active Directory B2C-Mandanten verwendet sollte auch konfiguriert werden, damit die Antwort-URL auf die gleiche benutzerdefinierte URL-Schema festgelegt ist. Dies kann erreicht werden, dazu **ausführen benutzerflow** im Azure-Portal die Einstellungen für den Zugriff auf. Speichern Sie die Metadaten-URL, die auf diesem Bildschirm gefunden werden, da Sie ihn für die Mobile App-Konfiguration benötigen. Der folgende Screenshot zeigt diese Konfiguration und die URL, die Sie kopieren soll:

[!["Azure Active Directory B2C-Richtlinien"](azure-ad-b2c-mobile-app-images/client-flow-policies-cropped.png)](azure-ad-b2c-mobile-app-images/client-flow-policies.png#lightbox "Konfiguration der Azure Active Directory B2C-Richtlinien")

### <a name="azure-mobile-app-configuration"></a>Azure Mobile App-Konfiguration

Konfigurieren Sie die Azure Mobile Apps-Instanz für einen Workflow Client verwaltete Authentifizierung wie folgt:

- App Service-Authentifizierung sollte aktiviert sein.
- Die Aktion an, wenn eine Anforderung nicht authentifiziert ist sollte festgelegt werden, um **melden Sie sich mit Azure Active Directory**.

Der folgende Screenshot veranschaulicht diese Konfiguration:

[!["Azure Mobile Apps-Authentifizierungskonfiguration"](azure-ad-b2c-mobile-app-images/ama-config-cropped.png)](azure-ad-b2c-mobile-app-images/ama-config.png#lightbox "Authentifizierungskonfiguration für Azure Mobile Apps")

Konfigurieren Sie die Azure Mobile Apps-Instanz für die Kommunikation mit dem Azure Active Directory B2C-Mandanten:

- Klicken Sie auf der Azure Active Directory-Konfiguration, und aktivieren Sie **erweitert** Modus für die Azure Active Directory-Authentifizierung.
- Legen Sie **Client-ID** auf die **Anwendungs-ID** des Azure Active Directory B2C-Mandanten.
- Legen Sie die **Aussteller-Url** zur Metadaten-URL von der Richtlinie, die Sie zuvor bei der Mandantenkonfiguration kopiert.

Der folgende Screenshot veranschaulicht diese Konfiguration:

!["Azure Mobile Apps erweiterte Konfiguration"](azure-ad-b2c-mobile-app-images/ama-advanced-config.png)

### <a name="signing-in"></a>Anmelden

Das folgende Codebeispiel zeigt wichtige Methodenaufrufen, zum Starten eines Workflows vom Client verwaltete Authentifizierung:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...

    authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        string.Empty,
        UIBehavior.SelectAccount,
        string.Empty,
        App.UiParent);

    ...

    var payload = new JObject();
    if (authenticationResult != null && !string.IsNullOrWhiteSpace(authenticationResult.IdToken))
    {
        payload["access_token"] = authenticationResult.IdToken;
    }

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    success = true;

    ...
}
```

Der Microsoft Authentication Library (MSAL) wird verwendet, um einen authentifizierungsworkflow mit dem Azure Active Directory B2C-Mandanten zu starten. Die `AcquireTokenAsync` Methode startet Webbrowser des Geräts, und zeigt Sie Authentifizierungsoptionen, die definiert, in der Azure Active Directory B2C-Richtlinie, die von der Richtlinie, die auf die verwiesen wird durch angegeben wird die `Constants.AuthoritySignin` Konstanten. Diese Richtlinie wird definiert, die Erfahrung des Benutzers anmelden und registrieren Sie sich, und die Ansprüche, die die Anwendung nach erfolgreicher Authentifizierung erhält.

Das Ergebnis der `AcquireTokenAsync` Methodenaufruf ist ein `AuthenticationResult` Instanz. Wenn die Authentifizierung erfolgreich ist, ist die `AuthenticationResult` Instanz enthält ein Identitätstoken, die lokal zwischengespeichert wird. Wenn die Authentifizierung nicht erfolgreich ist, ist die `AuthenticationResult` Instanz enthält Daten, der angibt, warum die Authentifizierung fehlgeschlagen ist. Weitere Informationen zur Verwendung von MSAL für die Kommunikation mit einem Azure Active Directory B2C-Mandanten finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Wenn die `CurrentClient.LoginAsync` -Methode wird aufgerufen, die Azure Mobile Apps-Instanz empfängt das Identitätstoken umschlossen eine `JObject`. Das Vorhandensein einer gültigen token bedeutet, dass die Azure Mobile Apps-Instanz, die eine eigene OAuth 2.0-authentifizierungsfluss starten müssen nicht. Stattdessen die `CurrentClient.LoginAsync` Methode gibt eine `MobileServiceUser` -Instanz, die in gespeichert werden, die `MobileServiceClient.CurrentUser` Eigenschaft. Diese Eigenschaft bietet `UserId` und `MobileServiceAuthenticationToken` Eigenschaften. Diese stehen für authentifizierte Benutzer und Token, das verwendet werden kann, bis es abläuft. In allen Anforderungen, die versucht, die Azure Mobile Apps-Instanz, wodurch die Xamarin.Forms-Anwendung für Aktionen, die Berechtigungen eines authentifizierten Benutzers zu erfordern, wird das Authentifizierungstoken enthalten sein.

### <a name="signing-out"></a>Abmelden

Im folgenden Codebeispiel wird veranschaulicht, wie die clientverwaltete Abmeldevorgang aufgerufen wird:

```csharp
public async Task<bool> LogoutAsync()
{
    ...

    IEnumerable<IAccount> accounts = await ADB2CClient.GetAccountsAsync();
    while (accounts.Any())
    {
        await ADB2CClient.RemoveAsync(accounts.First());
        accounts = await ADB2CClient.GetAccountsAsync();
    }
    User = null;

    ...
}
```

Die `CurrentClient.LogoutAsync` Methode aufgehoben authentifiziert den Benutzer mit der Azure Mobile Apps-Instanz, und klicken Sie dann alle Authentifizierungstoken aus dem lokalen Cache erstellt werden, indem Sie MSAL deaktiviert sind.

## <a name="server-managed-authentication"></a>Vom Server verwaltete Authentifizierung

Bei der vom Server verwaltete Authentifizierung kontaktiert eine Xamarin.Forms-Anwendung für eine Azure Mobile Apps-Instanz, die den Azure Active Directory B2C-Mandanten verwendet wird, um den OAuth 2.0-authentifizierungsfluss zu verwalten, indem Sie eine Anmeldeseite angezeigt, wie in der B2C-Richtlinie definiert. Nach dem erfolgreichen Anmelden gibt die Azure Mobile Apps-Instanz ein Token, das können die Xamarin.Forms-Anwendung für Aktionen, die Berechtigungen eines authentifizierten Benutzers erfordern.

### <a name="azure-active-directory-b2c-server-managed-tenant-configuration"></a>Azure Active Directory B2C-Mandanten vom Server verwaltete-Konfiguration

Der Azure Active Directory B2C-Mandant sollte für einen Workflow vom Server verwaltete Authentifizierung wie folgt konfiguriert werden:

- Umfassen Sie eine Web-app/Web-API und zulassen Sie den impliziten Fluss.
- Legen die Antwort-URL auf die Adresse der Azure Mobile App, gefolgt von `/.auth/login/aad/callback`.

Der folgende Screenshot veranschaulicht diese Konfiguration:

[!["Azure Active Directory B2C-Konfiguration"](azure-ad-b2c-mobile-app-images/server-flow-config-cropped.png)](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C-Konfiguration")

Die Richtlinie, die verwendet werden, in der Azure Active Directory B2C-Mandanten muss die Antwort-URL konfiguriert haben. Dies wird erreicht, indem die Antwort-URL an die Adresse des Azure Mobile App, gefolgt von `/.auth/login/aad/callback`. Sie sollten auch speichern, die Metadaten-URL, die am oberen Rand dieses Bildschirms gefunden werden, da Sie ihn für die Mobile App-Konfiguration benötigen. Der folgende Screenshot zeigt diese Konfiguration und die Metadaten-URL, die Sie speichern sollten:

!["Azure Active Directory B2C-Richtlinie-Konfiguration"](azure-ad-b2c-mobile-app-images/server-flow-policies.png)

### <a name="azure-mobile-apps-instance-configuration"></a>Konfiguration von Azure Mobile Apps-Instanz

Für einen Workflow vom Server verwaltete Authentifizierung sollte die Azure Mobile Apps-Instanz wie folgt konfiguriert werden:

- App Service-Authentifizierung sollte aktiviert sein.
- Die Aktion an, wenn eine Anforderung nicht authentifiziert ist sollte festgelegt werden, um **melden Sie sich mit Azure Active Directory**.

Der folgende Screenshot veranschaulicht diese Konfiguration:

[!["Azure Mobile Apps-Authentifizierungskonfiguration"](azure-ad-b2c-mobile-app-images/ama-config-cropped.png)](azure-ad-b2c-mobile-app-images/ama-config.png#lightbox "Authentifizierungskonfiguration für Azure Mobile Apps")

Die Azure Mobile Apps-Instanz sollte auch für die Kommunikation mit dem Azure Active Directory B2C-Mandanten konfiguriert werden:

- Klicken Sie auf der Azure Active Directory-Konfiguration, und aktivieren Sie **erweitert** Modus für die Azure Active Directory-Authentifizierung.
- Legen Sie **Client-ID** auf die **Anwendungs-ID** des Azure Active Directory B2C-Mandanten.
- Die **Aussteller-Url** Metadaten-URL von der Richtlinie, die Sie zuvor bei der Mandantenkonfiguration kopiert wird.

Der folgende Screenshot veranschaulicht diese Konfiguration:

!["Azure Mobile Apps erweiterte Konfiguration"](azure-ad-b2c-mobile-app-images/ama-advanced-config.png)

### <a name="signing-in"></a>Anmelden

Im folgenden Codebeispiel wird veranschaulicht, wie zum Starten eines Workflows vom Server verwaltete Authentifizierung wird:

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...

    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);

    ...
}
```

Wenn die `CurrentClient.LoginAsync` -Methode wird aufgerufen, die Azure Mobile Apps-Instanz ausgeführt wird, die verknüpfte Azure Active Directory B2C-Richtlinie, die startet den OAuth 2.0-authentifizierungsfluss. Jede `AuthenticateAsync` Methode ist plattformspezifisch. Jedoch jede `AuthenticateAsync` Methodenaufrufe der `CurrentClient.LoginAsync` Methode und gibt an, dass es sich bei Azure Active Directory-Mandanten bei der Authentifizierung verwendet wird. Weitere Informationen finden Sie unter [Anmeldung von Benutzern](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

Die `CurrentClient.LoginAsync` Methode gibt eine `MobileServiceUser` -Instanz, die in gespeichert werden, die `CurrentClient.CurrentUser` Eigenschaft. Diese Eigenschaft bietet `UserId` und `MobileServiceAuthenticationToken` Eigenschaften. Diese repräsentieren den authentifizierten Benutzer und ein Authentifizierungstoken für den Benutzer, die verwendet werden kann, bis es abläuft. In allen Anforderungen, die versucht, die Azure Mobile Apps-Instanz, wodurch die Xamarin.Forms-Anwendung zum Ausführen von Aktionen für die Azure Mobile Apps-Instanz, die Berechtigungen eines authentifizierten Benutzers zu erfordern, wird das Authentifizierungstoken enthalten sein.

### <a name="signing-out"></a>Abmelden

Im folgenden Codebeispiel wird veranschaulicht, wie die vom Server verwaltete Abmeldevorgang aufgerufen wird:

```csharp
public async Task<bool> LogoutAsync()
{
    ...

    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();

    ...
}
```

Die `MobileServiceClient.LogoutAsync` Methode authentifiziert den Benutzer mit der Azure Mobile Apps-Instanz, aufheben. Weitere Informationen finden Sie unter [sich Benutzer anmelden](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).


## <a name="related-links"></a>Verwandte Links

- [TodoAzureAuth ClientFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [TodoAzureAuth ServerFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [Verwenden einer Azure-Mobile-App](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft Authentication Library Nuget-Paket](https://www.nuget.org/packages/Microsoft.Identity.Client)
- [Microsoft Authentication Library-Dokumentation](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)