---
title: Integrieren von Azure Mobile Apps in Azure Active Directory B2C
description: "Azure Active Directory B2C ist eine Cloud-identitätsverwaltungslösung für kundenorientierten Web- und mobilen Anwendungen. Dieser Artikel veranschaulicht, wie Azure Active Directory B2C-Authentifizierung und Autorisierung mit einer Instanz von Azure Mobile Apps mit Xamarin.Forms bereitstellen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: c28ddc09b07066de67f5c974cf5c2128726c6932
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Integrieren von Azure Mobile Apps in Azure Active Directory B2C

_Azure Active Directory B2C ist eine Cloud-identitätsverwaltungslösung für kundenorientierten Web- und mobilen Anwendungen. Dieser Artikel veranschaulicht, wie Azure Active Directory B2C-Authentifizierung und Autorisierung mit einer Instanz von Azure Mobile Apps mit Xamarin.Forms bereitstellen._

![](~/media/shared/preview.png "Diese API ist derzeit Vorabversion")

> [!NOTE]
> Die [Microsoft-Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Identity.Client) noch in der Vorschau ist, aber für die Verwendung in einer produktiven Umgebung geeignet ist. Allerdings kann es aktuelle werden Änderungen an der API, internen Cache-Format und andere Mechanismen der Bibliothek, die sich auf Ihrer Anwendung auswirken kann.

## <a name="overview"></a>Übersicht

Azure Mobile Apps können Sie Anwendungen mit Back-Ends skalierbare in Azure App Service, gehostet wird, mit Unterstützung für mobile-Authentifizierung, offline-Synchronisierung und Push-Benachrichtigungen zu entwickeln. Weitere Informationen zu Azure-Mobile-Apps, finden Sie unter [einer Azure-Mobile-App nutzen](~/xamarin-forms/data-cloud/consuming/azure.md), und [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure Active Directory B2C ist ein identitätsverwaltungsdienst bei kundenorientierten-Anwendungen, die Consumern, die Anwendung unter anmelden kann:

- Verwenden ihre vorhandenen sozialen Konten (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Erstellen neue Anmeldeinformationen (e-Mail-Adresse und Kennwort oder Benutzernamen und Kennwort). Diese Anmeldeinformationen werden als bezeichnet *lokale* Konten.

Weitere Informationen zu Azure Active Directory B2C finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure Active Directory B2C kann zum Verwalten der Authentifizierung für ein Azure-Mobile-Anwendung verwendet werden. Bei diesem Ansatz die Verwaltungsoberfläche für die Identität ist vollständig in der Cloud definiert und kann ohne Änderung des Codes für die mobile Anwendung geändert werden.

Es gibt zwei authentifizierungsworkflows, die übernommen werden können, wenn Sie einen Azure Active Directory B2C-Mandanten mit einer Instanz von Azure Mobile Apps integrieren:

- [Client verwalteten](#client_managed) – in diesem Ansatz die Xamarin.Forms mobile Anwendung initiiert die Authentifizierung mit dem Azure Active Directory B2C-Mandanten zu verarbeiten, und übergibt die empfangenen Authentifizierungstokens an die Azure Mobile Apps-Instanz.
- [Serververwaltetem](#server_managed) – bei diesem Ansatz die Azure-Mobile-Apps verwendet Instanz des Azure Active Directory B2C-Mandanten, um die Authentifizierung über einen Web-basierten Workflow initiieren.

In beiden Fällen wird der Authentifizierungsvorgang von Azure Active Directory B2C-Mandanten bereitgestellt. In der beispielanwendung führt dies in der im Anmeldebildschirm ein, in den folgenden Screenshots dargestellt:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Anmeldeseite")

Anmelden mit Identitätsanbieter sozialer Netzwerke oder mit einem lokalen Konto, sind zulässig. Während Microsoft und Google, Facebook als Identitätsanbieter sozialer Netzwerke in diesem Beispiel verwendet werden, können auch andere Identitätsanbieter verwendet werden.

## <a name="setup"></a>Setup

Unabhängig von der authentifizierungsworkflow verwendet wird wird wie folgt des ersten Prozesses für die Integration von Azure Active Directory B2C-Mandanten mit einer Instanz von Azure-Mobile-Apps:

1. Erstellen Sie eine Azure Mobile Apps-Instanz. Weitere Informationen finden Sie unter [einer Azure-Mobile-App nutzen](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Aktivieren der Authentifizierung in der Azure-Mobile Apps-Instanz und die Xamarin.Forms-Anwendung. Weitere Informationen finden Sie unter [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Erstellen eines Azure Active Directory B2C-Mandanten. Weitere Informationen finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Beachten Sie, dass die Microsoft Authentication Library (MSAL) bei Verwendung ein Workflows für verwaltete Client-Authentifizierung erforderlich ist. MSAL verwendet Webbrowser des Geräts zum Durchführen der Authentifizierung. Dies verbessert die Verwendbarkeit einer Anwendung, wie Benutzer müssen sich nur anmelden, nachdem jedes Gerät, in der Anwendung verbessern Wechselkurse für die Anmeldung und Autorisierung fließt. Die Gerätebrowser bietet auch eine verbesserte Sicherheit. Nachdem der Benutzer den Authentifizierungsprozess abgeschlossen ist, gibt Steuerelement aus der Registerkarte des Webbrowsers für die Anwendung zurück. Dies erfolgt durch registrieren ein benutzerdefinierte URL-Schema für die umleitungs-URL, die den Authentifizierungsprozess, und klicken Sie dann erkennen und behandeln die benutzerdefinierte URL nach dem Senden der es zurückgegeben wird. Weitere Informationen zur Verwendung von MSAL für die Kommunikation mit einem Azure Active Directory B2C-Mandanten finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Verwaltete Client-Authentifizierung

Bei der Authentifizierung Client verwaltet kontaktiert eine Xamarin.Forms-mobile-Anwendung ein Azure Active Directory B2C-Mandanten, um ein Authentifizierungsablauf zu initiieren. Nach der erfolgreichen Anmeldung der Azure Active Directory B2C zurück Mandanten ein Identitätstoken an das dann während der Anmeldung mit der Azure-Mobile Apps-Instanz bereitgestellt wird. Dadurch kann es sich um die Xamarin.Forms-Anwendung auszuführenden Aktionen auf der Azure-Mobile Apps-Instanz, die authentifizierten Benutzerberechtigungen erfordert.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Die Konfiguration von Azure Active Directory B2C-Mandanten

Für einen authentifizierungsworkflow verwalteten Client-sollte wie folgt die Azure Active Directory B2C-Mandanten konfiguriert werden:

- Einen systemeigenen Client einschließen
- Legen Sie benutzerdefinierte Umleitungs-URI auf eine URL-Schema, die eindeutig von der mobilen Anwendung ein, gefolgt von `://auth/`. Weitere Informationen zum Auswählen eines benutzerdefinierten URL-Schemas finden Sie unter [wählen einen umleitungs-URI für das systemeigene app](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

Der folgende Screenshot zeigt diese Konfiguration:

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Die Konfiguration von Azure Active Directory B2C")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "Azure Active Directory B2C-Konfiguration")

Die Richtlinie verwendet wird, in der Azure Active Directory B2C Mandanten sollte auch konfiguriert werden, sodass die Antwort-URL auf den gleichen benutzerdefinierten URL-Schema festgelegt ist, gefolgt von `://auth/`. Der folgende Screenshot zeigt diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Azure Active Directory B2C-Richtlinien")

### <a name="azure-mobile-app-configuration"></a>Azure Mobile App-Konfiguration

Für einen authentifizierungsworkflow Client verwalteten sollten das Azure Mobile Apps-Instanz wie folgt konfiguriert werden:

- App Service-Authentifizierung sollte aktiviert sein.
- Die Aktion an, wenn eine Anforderung nicht authentifiziert ist sollte festgelegt werden, um **melden Sie sich mit Azure Active Directory**.

Der folgende Screenshot zeigt diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Azure Mobile Apps-Konfiguration")

Die Instanz von Azure Mobile Apps sollten auch für die Kommunikation mit dem Azure Active Directory B2C-Mandanten konfiguriert werden. Dies geschieht durch Aktivieren der **erweitert** Modus für die Azure Active Directory-Authentifizierungsanbieter, mit der **Client-ID** wird die **Anwendungs-ID** von Azure Active Directory B2C-Mandanten und die **Aussteller-Url** wird der Metadaten-Endpunkt für die Azure Active Directory B2C-Richtlinie. Der folgende Screenshot zeigt diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Erweiterte Konfiguration von Azure-Mobile Apps")

### <a name="signing-in"></a>Anmelden

Im folgenden Codebeispiel wird gezeigt, wie einen authentifizierungsworkflow Client verwalteten initiiert wird:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);

    ...
    var payload = new JObject();
    payload["access_token"] = authenticationResult.IdToken;

    User = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        payload);
    ...
}
```

Die Microsoft Authentication Library (MSAL) wird verwendet, um einen authentifizierungsworkflow mit dem Azure Active Directory B2C-Mandanten zu initiieren. Die `AcquireTokenAsync` -Methode wird der Webbrowser des Geräts gestartet und zeigt Sie Authentifizierungsoptionen definiert, in der Azure Active Directory B2C-Richtlinie, die von der Richtlinie verwiesen wird, über angegeben wird die `Constants.Authority` konstant. Diese Richtlinie definiert die Features, mit denen Kunden durchlaufen werden bei der Registrierung, und melden Sie sich, und die Ansprüche an, dass die Anwendung erfolgreich empfangen wird, registrieren oder anmelden.

Das Ergebnis der `AcquireTokenAsync` Methodenaufruf ist ein `AuthenticationResult` Instanz. Wenn die Authentifizierung erfolgreich ist, ist die `AuthenticationResult` Instanz enthält ein Identitätstoken an, die lokal zwischengespeichert wird. Wenn die Authentifizierung nicht erfolgreich ist, ist die `AuthenticationResult` Instanz enthält Daten, der angibt, warum die Authentifizierung fehlgeschlagen ist. Informationen zur Verwendung von MSAL für die Kommunikation mit einem Azure Active Directory B2C-Mandanten finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Wenn die `MobileServiceClient.LoginAsync` Methode wird aufgerufen, die Azure-Mobile Apps-Instanz empfängt das Identitätstoken eingebunden in eine `JObject`. Das Vorhandensein einer gültigen token bedeutet, dass die die Azure-Mobile Apps-Instanz nicht benötigt, um eine eigene OAuth 2.0-authentifizierungsfluss zu initiieren. Stattdessen die `MobileServiceClient.LoginAsync` Methode gibt ein `MobileServiceUser` -Instanz, die in gespeichert werden, die `MobileServiceClient.CurrentUser` Eigenschaft. Diese Eigenschaft ermöglicht `UserId` und `MobileServiceAuthenticationToken` Eigenschaften. Diese repräsentieren die authentifizierten Benutzer und ein Authentifizierungstoken für den Benutzer, die verwendet werden kann, bis sie abläuft. In aller erfolgten Anforderungen an die Azure Mobile Apps-Instanz, die Xamarin.Forms Anwendung zum Ausführen von Aktionen auf der Azure-Mobile Apps-Instanz, die authentifizierten Benutzerberechtigungen erfordern, wird das Authentifizierungstoken enthalten sein.

### <a name="signing-out"></a>Abmelden

Im folgenden Codebeispiel wird veranschaulicht, wie der Client verwaltet Abmeldevorgang aufgerufen wird:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();

    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

Die `MobileServiceClient.LogoutAsync` Methode Deduplizierung authentifiziert den Benutzer mit der Azure-Mobile Apps-Instanz, und klicken Sie dann alle Authentifizierungstoken vom lokalen Cache erstellt, indem MSAL deaktiviert sind.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Verwaltete Server-Authentifizierung

Bei der Authentifizierung serververwaltetem kontaktiert eine Xamarin.Forms-Anwendung eine Azure Mobile Apps-Instanz, die den Azure Active Directory B2C-Mandanten verwendet wird, um die OAuth 2.0-authentifizierungsfluss zu verwalten, indem Sie eine Anmeldeseite angezeigt, wie in der B2C-Richtlinie definiert. Nach der erfolgreichen Anmeldung gibt die Azure-Mobile Apps-Instanz ein Token, erlaubt die Xamarin.Forms-Anwendung zum Ausführen von Aktionen auf der Azure-Mobile Apps-Instanz, die Berechtigungen authentifizierter Benutzer erfordern.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Die Konfiguration von Azure Active Directory B2C-Mandanten

Für einen authentifizierungsworkflow verwalteten Server-sollte wie folgt die Azure Active Directory B2C-Mandanten konfiguriert werden:

- Schließen Sie eine Web-app/Web-API und ermöglichen Sie impliziten Datenfluss zu.
- Legen Sie die Antwort-URL auf die Adresse des Azure-Mobile-Anwendung, gefolgt von `/.auth/login/aad/callback`.

Der folgende Screenshot zeigt diese Konfiguration:

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Die Konfiguration von Azure Active Directory B2C")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory B2C-Konfiguration")

Die Richtlinie in der Azure Active Directory B2C Mandanten auch konfiguriert werden sollte, damit die Antwort-URL festgelegt ist, an die Adresse des Azure-Mobile-App verwendet, gefolgt von `/.auth/login/aad/callback`. Der folgende Screenshot zeigt diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Azure Active Directory B2C-Richtlinien")

### <a name="azure-mobile-apps-instance-configuration"></a>Konfiguration der Azure-Mobile Apps-Instanz

Für einen authentifizierungsworkflow serververwaltetem sollten das Azure Mobile Apps-Instanz wie folgt konfiguriert werden:

- App Service-Authentifizierung sollte aktiviert sein.
- Die Aktion an, wenn eine Anforderung nicht authentifiziert ist sollte festgelegt werden, um **melden Sie sich mit Azure Active Directory**.

Der folgende Screenshot zeigt diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Azure Mobile Apps-Konfiguration")

Die Instanz von Azure Mobile Apps sollten auch für die Kommunikation mit dem Azure Active Directory B2C-Mandanten konfiguriert werden. Dies geschieht durch Aktivieren der **erweitert** Modus für die Azure Active Directory-Authentifizierungsanbieter, mit der **Client-ID** wird die **Anwendungs-ID** von Azure Active Directory B2C-Mandanten und die **Aussteller-Url** wird der Metadaten-Endpunkt für die Azure Active Directory B2C-Richtlinie. Der folgende Screenshot zeigt diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Erweiterte Konfiguration von Azure-Mobile Apps")

### <a name="signing-in"></a>Anmelden

Im folgenden Codebeispiel wird gezeigt, wie einen authentifizierungsworkflow serververwaltetem initiiert wird:

```csharp
public async Task<bool> AuthenticateAsync()
{
    ...
    MobileServiceUser user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
        UIApplication.SharedApplication.KeyWindow.RootViewController,
        MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory,
        Constants.URLScheme);
    ...
}
```

Wenn die `MobileServiceClient.LoginAsync` Methode wird aufgerufen, die Azure-Mobile Apps-Instanz ausgeführt wird, die verknüpften Azure Active Directory B2C-Richtlinie, die OAuth 2.0-authentifizierungsfluss initiiert. Beachten Sie, dass jedes `AuthenticateAsync` Methode ist plattformspezifisch. Allerdings jedes `AuthenticateAsync` -Methode verwendet die `MobileServiceClient.LoginAsync` Methode und gibt an, dass bei der Authentifizierung ein Azure Active Directory-Mandanten verwendet wird. Weitere Informationen finden Sie unter [Anmeldung von Benutzern](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

Die `MobileServiceClient.LoginAsync` Methode gibt ein `MobileServiceUser` -Instanz, die in gespeichert werden, die `MobileServiceClient.CurrentUser` Eigenschaft. Diese Eigenschaft ermöglicht `UserId` und `MobileServiceAuthenticationToken` Eigenschaften. Diese repräsentieren die authentifizierten Benutzer und ein Authentifizierungstoken für den Benutzer, die verwendet werden kann, bis sie abläuft. In aller erfolgten Anforderungen an die Azure Mobile Apps-Instanz, die Xamarin.Forms Anwendung zum Ausführen von Aktionen auf der Azure-Mobile Apps-Instanz, die authentifizierten Benutzerberechtigungen erfordern, wird das Authentifizierungstoken enthalten sein.

### <a name="signing-out"></a>Abmelden

Im folgenden Codebeispiel wird veranschaulicht, wie der Server verwaltet Abmeldevorgang aufgerufen wird:

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
    ...
}
```

Die `MobileServiceClient.LogoutAsync` Methode Deduplizierung authentifiziert den Benutzer mit der Azure-Mobile Apps-Instanz. Weitere Informationen finden Sie unter [Protokollierung Out Benutzer](~/xamarin-forms/data-cloud/authentication/azure.md#logging-out).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie Azure Active Directory B2C zu verwenden, um Authentifizierung und Autorisierung mit einer Instanz von Azure Mobile Apps mit Xamarin.Forms bereitzustellen. Azure Active Directory B2C ist eine Cloud-identitätsverwaltungslösung für kundenorientierten Web- und mobilen Anwendungen.


## <a name="related-links"></a>Verwandte Links

- [TodoAzureAuth ServerFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Verwenden einer mobilen Anwendung für Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft-Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Identity.Client)
