---
title: Authentifizieren von Benutzern mit Azure Mobile Apps
description: In diesem Artikel wird erläutert, wie Sie Azure Mobile Apps zu verwenden, um die Verwaltung des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung ermöglichen.
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 428e536d6895ff16a928f8cc40a8a7976d087471
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61331327"
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Authentifizieren von Benutzern mit Azure Mobile Apps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)

_Azure Mobile Apps verwenden eine Vielzahl externer Identitätsanbieter zum Authentifizieren und autorisieren Benutzer von Anwendung, einschließlich Facebook, Google, Microsoft, Twitter und Azure Active Directory zu unterstützen. Berechtigungen können für Tabellen festgelegt werden, um den Zugriff nur für authentifizierte Benutzer einzuschränken. In diesem Artikel wird erläutert, wie Sie Azure Mobile Apps zu verwenden, um die Verwaltung des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung ermöglichen._

## <a name="overview"></a>Übersicht

Der Prozess für die Verwendung von Azure Mobile Apps, die Verwaltung des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung lautet wie folgt aus:

1. Registrieren Sie Ihre Azure Mobile App auf der Website eines Identitätsanbieters, und legen Sie dann die vom Anbieter generierten Anmeldeinformationen im Mobile Apps-Back-End. Weitere Informationen finden Sie unter [Registrieren Ihrer app für Authentifizierung und Konfigurieren von App Services](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services).
1. Definieren Sie ein neues URL-Schema für die Xamarin.Forms-Anwendung, die ermöglicht das Authentifizierungssystem die zurück an die Xamarin.Forms-Anwendung umgeleitet werden soll, sobald der Authentifizierungsprozess abgeschlossen ist. Weitere Informationen finden Sie unter [Hinzufügen Ihrer app zu den zulässigen externen Umleitungs-URLs](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl).
1. Beschränken des Zugriffs auf das Azure Mobile Apps-Back-End auf authentifizierte Clients. Weitere Informationen finden Sie unter [Einschränken von Berechtigungen für authentifizierte Benutzer](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users).
1. Rufen Sie die Authentifizierung über die Xamarin.Forms-Anwendung. Weitere Informationen finden Sie unter [Hinzufügen von Authentifizierung zur portablen Klassenbibliothek](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library), [Hinzufügen von Authentifizierung zur iOS-app](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app), [Hinzufügen von Authentifizierung zur Android-app](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app), und [ Hinzufügen von Authentifizierung zu Windows 10-app-Projekten](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects).

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

In der Vergangenheit haben mobilanwendungen eingebettete Webansichten verwendet, zum Ausführen der Authentifizierung mit des Identitätsanbieters. Dies wird nicht mehr aus den folgenden Gründen empfohlen:

- Die Anwendung, die Webansicht hostet, kann die Anmeldeinformationen des Benutzers vollständige Authentifizierung, nicht nur die Gewährung der Autorisierung zugreifen, die für die Anwendung bestimmt ist. Dies verstößt gegen das Prinzip der geringsten Rechte, wie die Anwendung Zugriff auf leistungsfähigere Anmeldeinformationen, die hat als potenziell erhöht die Angriffsfläche der Anwendung erfordert.
- Die hostanwendung konnte erfassen, Benutzernamen und Kennwörter, automatisch Senden von Formularen und umgehen Zustimmung des Benutzers, und Sitzungscookies zu kopieren und verwenden, um authentifizierte Aktionen als Benutzer ausführen.
- Eingebettete Webansichten freigeben nicht des Authentifizierungsstatus mit anderen Anwendungen oder Webbrowser des Geräts, dass der Benutzer sich anmelden für jede autorisierungsanforderung ein tiefgestelltes Bedienungserlebnis angesehen wird.
- Einige Autorisierungs-Endpunkten Maßnahmen zum Erkennen und Blockieren von autorisierungsanforderungen, die von Webansichten stammen.

Die Alternative ist die Verwendung der Webbrowser zum Durchführen der Authentifizierung wird der Ansatz, die neueste Version von der [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/). Mithilfe des Geräte-Browsers für authentifizierungsanforderungen verbessert die Verwendbarkeit einer Anwendung, wie die Benutzer müssen nur den betreffenden Identitätsanbieter anzeigt, einmal pro Gerät, Verbessern der konvertierungsraten-Anmeldung und Autorisierung von Flows in der Anwendung anmelden. Der Gerätebrowser bietet verbesserte Sicherheit auch, wie Anwendungen sind in der Lage, um zu überprüfen und Ändern von Inhalt in eine Webansicht, aber nicht im Browser angezeigten Inhalt.

## <a name="using-an-azure-mobile-apps-instance"></a>Verwenden einer Azure Mobile Apps-Instanz

Die [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) bietet die `MobileServiceClient` -Klasse, die von einer Xamarin.Forms-Anwendung verwendet wird, Zugriff auf die Azure Mobile Apps-Instanz.

Die beispielanwendung verwendet die Google als Identitätsanbieter, der Benutzer mit Google-Konten für die Anmeldung bei der Xamarin.Forms-Anwendung ermöglicht. Während Sie Google als Identitätsanbieter in diesem Artikel verwendet wird, ist der Ansatz mit anderen Identitätsanbietern gleichermaßen anwendbar.

<a name="logging-in" />

### <a name="logging-in-users"></a>Anmeldung von Benutzern

Der Anmeldebildschirm in der beispielanwendung wird in den folgenden Screenshots dargestellt:

![](azure-images/login.png "Anmeldeseite")

Während Sie Google als Identitätsanbieter verwendet wird, kann eine Vielzahl von anderen Identitätsanbietern verwendet werden, einschließlich Facebook, Microsoft, Twitter und Azure Active Directory.

Im folgenden Codebeispiel wird veranschaulicht, wie der Anmeldeprozess aufgerufen wird:

```csharp
async void OnLoginButtonClicked(object sender, EventArgs e)
{
  ...
  if (App.Authenticator != null)
  {
    authenticated = await App.Authenticator.AuthenticateAsync();
  }
  ...
}
```

Die `App.Authenticator` -Eigenschaft ist ein `IAuthenticate` -Instanz, die von den plattformspezifischen Projekten festgelegt ist. Die `IAuthenticate` Schnittstelle gibt ein `AuthenticateAsync` Vorgang, der vom jedes plattformprojekt bereitgestellt werden muss. Daher hat das Aufrufen der `App.Authenticator.AuthenticateAsync` Methode führt die `IAuthenticate.AuthenticateAsync` -Methode in der ein Plattform-Projekt.

Alle von der Plattform `IAuthenticate.AuthenticateAsync` Methoden der `MobileServiceClient.LoginAsync` Methode, um eine Schnittstelle und Cache Anmeldedaten anzuzeigen. Das folgende Codebeispiel zeigt die `LoginAsync` -Methode für die iOS-Plattform:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    UIApplication.SharedApplication.KeyWindow.RootViewController,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Das folgende Codebeispiel zeigt die `LoginAsync` -Methode für die Android-Plattform:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    this,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Das folgende Codebeispiel zeigt die `LoginAsync` Methode für die universelle Windows-Plattform:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

Auf allen Plattformen die `MobileServiceAuthenticationProvider` Enumeration wird verwendet, um den Identitätsanbieter, die verwendet werden, bei der Authentifizierung angeben. Wenn die `MobileServiceClient.LoginAsync` -Methode wird aufgerufen, Azure Mobile Apps einen authentifizierungsdatenfluss initiiert, indem die Anmeldungsseite des ausgewählten Anbieters und generieren nach der erfolgreichen Anmeldung beim Identitätsanbieter einen Authentifizierungstoken. Die `MobileServiceClient.LoginAsync` Methode gibt eine `MobileServiceUser` -Instanz, die in gespeichert werden, die `MobileServiceClient.CurrentUser` Eigenschaft. Diese Eigenschaft bietet `UserId` und `MobileServiceAuthenticationToken` Eigenschaften. Diese stehen für den authentifizierten Benutzer und ein Authentifizierungstoken für den Benutzer. In allen Anforderungen, die versucht, die Azure Mobile Apps-Instanz, wodurch die Xamarin.Forms-Anwendung zum Ausführen von Aktionen für die Azure Mobile App-Instanz, die Berechtigungen eines authentifizierten Benutzers zu erfordern, wird das Authentifizierungstoken enthalten sein.

<a name="logging-out" />

### <a name="logging-out-users"></a>Benutzer abmelden

Im folgenden Codebeispiel wird veranschaulicht, wie der Abmeldevorgang aufgerufen wird:

```csharp
async void OnLogoutButtonClicked(object sender, EventArgs e)
{
  bool loggedOut = false;

  if (App.Authenticator != null)
  {
    loggedOut = await App.Authenticator.LogoutAsync ();
  }
  ...
}
```

Die `App.Authenticator` -Eigenschaft ist ein `IAuthenticate` -Instanz, die von jeder Platformproject festgelegt wird. Die `IAuthenticate` Schnittstelle gibt ein `LogoutAsync` Vorgang, der vom jedes plattformprojekt bereitgestellt werden muss. Daher hat das Aufrufen der `App.Authenticator.LogoutAsync` Methode führt die `IAuthenticate.LogoutAsync` -Methode in der ein Plattform-Projekt.

Alle von der Plattform `IAuthenticate.LogoutAsync` Methoden der `MobileServiceClient.LogoutAsync` Methode zum Aufheben den angemeldete Benutzer mit dem Identitätsanbieter zu authentifizieren. Das folgende Codebeispiel zeigt die `LogoutAsync` -Methode für die iOS-Plattform:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  foreach (var cookie in NSHttpCookieStorage.SharedStorage.Cookies)
  {
    NSHttpCookieStorage.SharedStorage.DeleteCookie (cookie);
  }
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Das folgende Codebeispiel zeigt die `LogoutAsync` -Methode für die Android-Plattform:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  CookieManager.Instance.RemoveAllCookie();
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Das folgende Codebeispiel zeigt die `LogoutAsync` Methode für die universelle Windows-Plattform:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Wenn die `IAuthenticate.LogoutAsync` -Methode wird aufgerufen, die alle Cookies, die vom Identitätsanbieter deaktiviert sind, bevor die `MobileServiceClient.LogoutAsync` Methode wird aufgerufen, um das Aufheben den angemeldete Benutzer mit dem Identitätsanbieter zu authentifizieren.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Azure Mobile Apps zu verwenden, um die Verwaltung des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung ermöglichen. Azure Mobile Apps verwenden eine Vielzahl externer Identitätsanbieter zum Authentifizieren und autorisieren Benutzer von Anwendung, einschließlich Facebook, Google, Microsoft, Twitter und Azure Active Directory zu unterstützen. Die `MobileServiceClient` Klasse wird von der Xamarin.Forms-Anwendung zum Steuern des Zugriffs auf die Azure Mobile Apps-Instanz verwendet.


## <a name="related-links"></a>Verwandte Links

- [TodoAzureAuth (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Verwenden einer Azure-Mobile-App](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Hinzufügen von Authentifizierung zu Ihrer Xamarin.Forms-app](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
