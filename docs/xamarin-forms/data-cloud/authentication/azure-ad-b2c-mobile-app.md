---
title: Die Integration von Azure Active Directory B2C mit Azure Mobile Apps
description: Azure Active Directory B2C ist eine Cloudlösung für die Verwaltung von Identitäten für kundenorientierte Web- und mobile Anwendungen. In diesem Artikel wird veranschaulicht, wie Sie Azure Active Directory B2C zum Bereitstellen von Authentifizierung und Autorisierung mit einer Instanz von Azure Mobile Apps mit Xamarin.Forms zu verwenden.
ms.prod: xamarin
ms.assetid: 53F52036-A997-4D0F-86B4-4302C6913136
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 135977329e2a190dd4c611937f6b8a664f135f5c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61332194"
---
# <a name="integrating-azure-active-directory-b2c-with-azure-mobile-apps"></a>Die Integration von Azure Active Directory B2C mit Azure Mobile Apps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)

_Azure Active Directory B2C ist eine Cloudlösung für die Verwaltung von Identitäten für kundenorientierte Web- und mobile Anwendungen. In diesem Artikel wird veranschaulicht, wie Sie Azure Active Directory B2C zum Bereitstellen von Authentifizierung und Autorisierung mit einer Instanz von Azure Mobile Apps mit Xamarin.Forms zu verwenden._

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

> [!NOTE]
> Die [Microsoft Authentication Library](https://www.nuget.org/packages/Microsoft.Identity.Client) ist weiterhin als Vorschau verfügbar, aber für die Verwendung in einer produktionsumgebung geeignet ist. Allerdings kann es wichtige werden Änderungen an der API, internen und Mechanismen für die Bibliothek, die Ihre Anwendung auswirken können.

## <a name="overview"></a>Übersicht

Azure Mobile Apps können Sie Anwendungen mit skalierbaren Back-Ends in Azure App Service gehostet wird, mit Unterstützung für mobile Authentifizierung, offlinesynchronisierung und Pushbenachrichtigungen zu entwickeln. Weitere Informationen zu Azure Mobile Apps, finden Sie unter [Nutzung von Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md), und [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

Azure Active Directory B2C ist ein identitätsverwaltungsdienst für kundenorientierte Anwendungen, die Kunden bei Ihrer Anwendung anmelden kann:

- Verwenden ihren vorhandenen Konten sozialer Netzwerke (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Erstellen neue Anmeldeinformationen (e-Mail-Adresse und Kennwort oder Benutzername und Kennwort). Diese Anmeldeinformationen werden als bezeichnet *lokalen* Konten.

Weitere Informationen zu Azure Active Directory B2C, finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Azure Active Directory B2C kann verwendet werden, um den Workflow der Authentifizierung für Azure Mobile Apps zu verwalten. Bei diesem Ansatz die Verwaltungsoberfläche für die Identität ist in der Cloud vollständig definiert und geändert werden kann, ohne Ihren mobilen Anwendungscode ändern zu müssen.

Es gibt zwei authentifizierungsworkflows, die übernommen werden können, wenn Sie einen Azure Active Directory B2C-Mandanten mit einer Azure Mobile Apps-Instanz integrieren:

- [Clientgesteuerte](#client_managed) – in diesem Ansatz die Xamarin.Forms-Verwaltungsrichtlinie für mobile Anwendungen initiiert die Authentifizierung mit dem Azure Active Directory B2C-Mandanten zu verarbeiten, und übergibt das empfangene Authentifizierungstoken an die Azure Mobile Apps-Instanz.
- [Vom Server verwaltete](#server_managed) – bei diesem Ansatz den Azure Mobile Apps-Instanz, die Azure Active Directory B2C-Mandanten zum Initiieren des Authentifizierungsprozesses durch einen Web-basierten Workflow verwendet.

In beiden Fällen wird der Authentifizierungsvorgang durch den Azure Active Directory B2C-Mandanten bereitgestellt. In der beispielanwendung führt dies in der Anmeldeseite angezeigt, die in den folgenden Screenshots gezeigt:

![](azure-ad-b2c-mobile-app-images/screenshots.png "Anmeldeseite")

Anmeldung mit Identitätsanbietern aus sozialen Netzwerken oder mit einem lokalen Konto, sind zulässig. Während es sich bei Microsoft, Google und Facebook als Identitätsanbieter in diesem Beispiel verwendet werden, können auch andere Identitätsanbieter verwendet werden.

## <a name="setup"></a>Setup

Ist unabhängig von den Workflow der Authentifizierung verwendet der erste Prozess für die Integration in eines Azure Active Directory B2C-Mandantenverwaltungs mit einer Azure Mobile Apps-Instanz wie folgt ein:

1. Erstellen Sie eine Azure Mobile Apps-Instanz. Weitere Informationen finden Sie unter [Nutzung von Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Aktivieren der Authentifizierung in der Azure Mobile Apps-Instanz und die Xamarin.Forms-Anwendung. Weitere Informationen finden Sie unter [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).
1. Erstellen eines Azure Active Directory B2C-Mandanten. Weitere Informationen finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Beachten Sie, dass der Microsoft Authentication Library (MSAL) erforderlich ist, wenn Sie einen Workflow Client verwaltete Authentifizierung verwenden. MSAL verwendet Webbrowser des Geräts, um die Authentifizierung durchzuführen. Dies verbessert die Verwendbarkeit einer Anwendung, die als Benutzer müssen sich nur anmelden nach pro-Gerät in der Anwendung verbessern konvertierungsraten-Anmeldung und Autorisierung übertragen. Der Browser für Gerät bietet auch eine verbesserte Sicherheit. Nachdem der Benutzer den Authentifizierungsprozess abgeschlossen ist, gibt die Kontrolle an die Anwendung aus der Registerkarte des Webbrowsers zurück. Dies erfolgt durch die Registrierung eines benutzerdefinierten URL-Schemas für die umleitungs-URL, die zurückgegeben wird, aus der Authentifizierungsprozess, und klicken Sie dann erkennen und behandeln die benutzerdefinierte URL ein, nach der sie gesendet wird. Weitere Informationen zur Verwendung von MSAL für die Kommunikation mit einem Azure Active Directory B2C-Mandanten finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

<a name="client_managed" />

## <a name="client-managed-authentication"></a>Vom Client verwaltete Authentifizierung

Bei der vom Client verwaltete Authentifizierung kontaktiert eine mobilen Xamarin.Forms-Anwendung ein Azure Active Directory B2C-Mandanten um einen authentifizierungsdatenfluss zu initiieren. Nach der erfolgreichen Anmeldung der Azure Active Directory B2C gibt Mandanten ein Identitätstoken, die dann bei der Anmeldung mit der Azure Mobile Apps-Instanz bereitgestellt wird. Dadurch kann es sich um die Xamarin.Forms-Anwendung zum Ausführen von Aktionen auf der Azure Mobile Apps-Instanz, die Berechtigungen eines authentifizierten Benutzers erfordert.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Konfiguration der Azure Active Directory B2C-Mandanten

Der Azure Active Directory B2C-Mandant sollte für einen Workflow Client verwaltete Authentifizierung wie folgt konfiguriert werden:

- Einen nativen Client einschließen.
- Legen Sie die benutzerdefinierte Umleitungs-URI eine URL-Schema, die eindeutig von der mobilen Anwendung ein, gefolgt von `://auth/`. Weitere Informationen zum Auswählen eines benutzerdefinierten URL-Schemas finden Sie unter [Auswählen eines umleitungs-URI für das systemeigene app](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri).

Der folgende Screenshot veranschaulicht diese Konfiguration:

[![](azure-ad-b2c-mobile-app-images/client-flow-config-sml.png "Azure Active Directory-B2C-Konfiguration")](azure-ad-b2c-mobile-app-images/client-flow-config.png#lightbox "Azure Active Directory-B2C-Konfiguration")

Die Richtlinie, die verwendet werden, in der Azure Active Directory B2C Mandanten sollte auch konfiguriert werden, sodass die Antwort-URL auf die gleiche benutzerdefinierte URL-Schema festgelegt ist, gefolgt von `://auth/`. Der folgende Screenshot veranschaulicht diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/client-flow-policies.png "Azure Active Directory-B2C-Richtlinien")

### <a name="azure-mobile-app-configuration"></a>Azure Mobile App-Konfiguration

Für einen Workflow Client verwaltete Authentifizierung sollte die Azure Mobile Apps-Instanz wie folgt konfiguriert werden:

- App Service-Authentifizierung sollte aktiviert sein.
- Die Aktion an, wenn eine Anforderung nicht authentifiziert ist sollte festgelegt werden, um **melden Sie sich mit Azure Active Directory**.

Der folgende Screenshot veranschaulicht diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-config.png "Azure Mobile Apps-Konfiguration")

Die Azure Mobile Apps-Instanz sollte auch für die Kommunikation mit dem Azure Active Directory B2C-Mandanten konfiguriert werden. Dies kann erreicht werden, durch Aktivieren der **erweitert** Modus für die Azure Active Directory-Authentifizierung, mit der **Client-ID** wird die **Anwendungs-ID** von Azure Active Directory B2C-Mandanten, und die **Aussteller-Url** wird von der Metadaten-Endpunkt für die Azure Active Directory B2C-Richtlinie. Der folgende Screenshot veranschaulicht diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/client-flow-ama-advanced-config.png "Azure Mobile Apps, die erweiterte Konfiguration")

### <a name="signing-in"></a>Anmeldung

Im folgenden Codebeispiel wird veranschaulicht, wie einen Workflow Client verwaltete Authentifizierung initiiert wird:

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

Der Microsoft Authentication Library (MSAL) wird verwendet, um einen authentifizierungsworkflow mit dem Azure Active Directory B2C-Mandanten zu initiieren. Die `AcquireTokenAsync` Methode startet Webbrowser des Geräts, und zeigt Sie Authentifizierungsoptionen, die definiert, in der Azure Active Directory B2C-Richtlinie, die von der Richtlinie, die auf die verwiesen wird durch angegeben wird die `Constants.Authority` Konstanten. Diese Richtlinie definiert die Benutzeroberflächen, die Kunden bei der Registrierung und Anmeldung durchlaufen, und die Ansprüche an, dass die Anwendung beim erfolgreichen Registrierungs- oder Anmelderichtlinie erhält.

Das Ergebnis der `AcquireTokenAsync` Methodenaufruf ist ein `AuthenticationResult` Instanz. Wenn die Authentifizierung erfolgreich ist, ist die `AuthenticationResult` Instanz enthält ein Identitätstoken, die lokal zwischengespeichert wird. Wenn die Authentifizierung nicht erfolgreich ist, ist die `AuthenticationResult` Instanz enthält Daten, der angibt, warum die Authentifizierung fehlgeschlagen ist. Weitere Informationen zur Verwendung von MSAL für die Kommunikation mit einem Azure Active Directory B2C-Mandanten finden Sie unter [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md).

Wenn die `MobileServiceClient.LoginAsync` -Methode wird aufgerufen, die Azure Mobile Apps-Instanz empfängt das Identitätstoken umschlossen eine `JObject`. Das Vorhandensein einer gültigen token bedeutet, dass die Azure Mobile Apps-Instanz, die eine eigene OAuth 2.0-authentifizierungsfluss initiieren müssen nicht. Stattdessen die `MobileServiceClient.LoginAsync` Methode gibt eine `MobileServiceUser` -Instanz, die in gespeichert werden, die `MobileServiceClient.CurrentUser` Eigenschaft. Diese Eigenschaft bietet `UserId` und `MobileServiceAuthenticationToken` Eigenschaften. Diese repräsentieren den authentifizierten Benutzer und ein Authentifizierungstoken für den Benutzer, die verwendet werden kann, bis es abläuft. In allen Anforderungen, die versucht, die Azure Mobile Apps-Instanz, wodurch die Xamarin.Forms-Anwendung zum Ausführen von Aktionen für die Azure Mobile Apps-Instanz, die Berechtigungen eines authentifizierten Benutzers zu erfordern, wird das Authentifizierungstoken enthalten sein.

### <a name="signing-out"></a>Abmelden

Im folgenden Codebeispiel wird veranschaulicht, wie die clientverwaltete Abmeldevorgang aufgerufen wird:

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

Die `MobileServiceClient.LogoutAsync` Methode aufgehoben authentifiziert den Benutzer mit der Azure Mobile Apps-Instanz, und klicken Sie dann alle Authentifizierungstoken aus dem lokalen Cache erstellt werden, indem Sie MSAL deaktiviert sind.

<a name="server_managed" />

## <a name="server-managed-authentication"></a>Vom Server verwaltete Authentifizierung

Bei der vom Server verwaltete Authentifizierung kontaktiert eine Xamarin.Forms-Anwendung für eine Azure Mobile Apps-Instanz, die den Azure Active Directory B2C-Mandanten verwendet wird, um den OAuth 2.0-authentifizierungsfluss zu verwalten, indem Sie eine Anmeldeseite angezeigt, wie in der B2C-Richtlinie definiert. Nach dem erfolgreichen Anmelden gibt die Azure Mobile Apps-Instanz ein Token, das können die Xamarin.Forms-Anwendung zum Ausführen von Aktionen für die Azure Mobile Apps-Instanz, die Berechtigungen eines authentifizierten Benutzers erfordern.

### <a name="azure-active-directory-b2c-tenant-configuration"></a>Konfiguration der Azure Active Directory B2C-Mandanten

Der Azure Active Directory B2C-Mandant sollte für einen Workflow vom Server verwaltete Authentifizierung wie folgt konfiguriert werden:

- Umfassen Sie eine Web-app/Web-API und zulassen Sie den impliziten Fluss.
- Legen die Antwort-URL auf die Adresse der Azure Mobile App, gefolgt von `/.auth/login/aad/callback`.

Der folgende Screenshot veranschaulicht diese Konfiguration:

[![](azure-ad-b2c-mobile-app-images/server-flow-config-sml.png "Azure Active Directory-B2C-Konfiguration")](azure-ad-b2c-mobile-app-images/server-flow-config.png#lightbox "Azure Active Directory-B2C-Konfiguration")

Die Richtlinie, die verwendet werden, in der Azure Active Directory B2C Mandanten sollte auch konfiguriert werden, sodass die Antwort-URL an die Adresse der Azure-Mobile-App festgelegt wird, gefolgt von `/.auth/login/aad/callback`. Der folgende Screenshot veranschaulicht diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/server-flow-policies.png "Azure Active Directory-B2C-Richtlinien")

### <a name="azure-mobile-apps-instance-configuration"></a>Konfiguration von Azure Mobile Apps-Instanzen

Für einen Workflow vom Server verwaltete Authentifizierung sollte die Azure Mobile Apps-Instanz wie folgt konfiguriert werden:

- App Service-Authentifizierung sollte aktiviert sein.
- Die Aktion an, wenn eine Anforderung nicht authentifiziert ist sollte festgelegt werden, um **melden Sie sich mit Azure Active Directory**.

Der folgende Screenshot veranschaulicht diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-config.png "Azure Mobile Apps-Konfiguration")

Die Azure Mobile Apps-Instanz sollte auch für die Kommunikation mit dem Azure Active Directory B2C-Mandanten konfiguriert werden. Dies kann erreicht werden, durch Aktivieren der **erweitert** Modus für die Azure Active Directory-Authentifizierung, mit der **Client-ID** wird die **Anwendungs-ID** von Azure Active Directory B2C-Mandanten, und die **Aussteller-Url** wird von der Metadaten-Endpunkt für die Azure Active Directory B2C-Richtlinie. Der folgende Screenshot veranschaulicht diese Konfiguration:

![](azure-ad-b2c-mobile-app-images/server-flow-ama-advanced-config.png "Azure Mobile Apps, die erweiterte Konfiguration")

### <a name="signing-in"></a>Anmeldung

Im folgenden Codebeispiel wird veranschaulicht, wie einen Workflow vom Server verwaltete Authentifizierung initiiert wird:

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

Wenn die `MobileServiceClient.LoginAsync` -Methode wird aufgerufen, die Azure Mobile Apps-Instanz ausgeführt wird, die verknüpfte Azure Active Directory B2C-Richtlinie, die initiiert den OAuth 2.0-authentifizierungsfluss. Beachten Sie, dass jedes `AuthenticateAsync` Methode ist plattformspezifisch. Jedoch jede `AuthenticateAsync` -Methode verwendet die `MobileServiceClient.LoginAsync` Methode und gibt an, dass es sich bei Azure Active Directory-Mandanten bei der Authentifizierung verwendet wird. Weitere Informationen finden Sie unter [Anmeldung von Benutzern](~/xamarin-forms/data-cloud/authentication/azure.md#logging-in).

Die `MobileServiceClient.LoginAsync` Methode gibt eine `MobileServiceUser` -Instanz, die in gespeichert werden, die `MobileServiceClient.CurrentUser` Eigenschaft. Diese Eigenschaft bietet `UserId` und `MobileServiceAuthenticationToken` Eigenschaften. Diese repräsentieren den authentifizierten Benutzer und ein Authentifizierungstoken für den Benutzer, die verwendet werden kann, bis es abläuft. In allen Anforderungen, die versucht, die Azure Mobile Apps-Instanz, wodurch die Xamarin.Forms-Anwendung zum Ausführen von Aktionen für die Azure Mobile Apps-Instanz, die Berechtigungen eines authentifizierten Benutzers zu erfordern, wird das Authentifizierungstoken enthalten sein.

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

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht, wie Sie Azure Active Directory B2C zum Bereitstellen von Authentifizierung und Autorisierung mit einer Instanz von Azure Mobile Apps mit Xamarin.Forms zu verwenden. Azure Active Directory B2C ist eine Cloudlösung für die Verwaltung von Identitäten für kundenorientierte Web- und mobile Anwendungen.


## <a name="related-links"></a>Verwandte Links

- [TodoAzureAuth ServerFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CServerFlow/)
- [TodoAzureAuth ClientFlow (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthADB2CClientFlow/)
- [Verwenden einer Azure-Mobile-App](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Authentifizieren von Benutzern mit Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Authentifizieren von Benutzern mit Azure Active Directory B2C](~/xamarin-forms/data-cloud/authentication/azure-ad-b2c.md)
- [Microsoft-Authentifizierungsbibliothek](https://www.nuget.org/packages/Microsoft.Identity.Client)
