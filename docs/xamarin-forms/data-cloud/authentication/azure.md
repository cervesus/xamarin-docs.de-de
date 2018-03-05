---
title: Authentifizieren von Benutzern mit Azure Mobile Apps
description: "Azure Mobile Apps verwenden eine Vielzahl von externen Identitätsanbieter zum Authentifizieren und autorisieren Benutzer der Anwendung, einschließlich Facebook, Google, Microsoft, Twitter und Azure Active Directory zu unterstützen. Berechtigungen können für Tabellen festgelegt werden, um den Zugriff nur für authentifizierte Benutzer einzuschränken. In diesem Artikel wird erläutert, wie Azure Mobile Apps zu verwenden, um den Authentifizierungsvorgang in einer Xamarin.Forms-Anwendung verwalten."
ms.topic: article
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 823dcdfdaca79045a407b62ec7e75079ee25d72f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Authentifizieren von Benutzern mit Azure Mobile Apps

_Azure Mobile Apps verwenden eine Vielzahl von externen Identitätsanbieter zum Authentifizieren und autorisieren Benutzer der Anwendung, einschließlich Facebook, Google, Microsoft, Twitter und Azure Active Directory zu unterstützen. Berechtigungen können für Tabellen festgelegt werden, um den Zugriff nur für authentifizierte Benutzer einzuschränken. In diesem Artikel wird erläutert, wie Azure Mobile Apps zu verwenden, um den Authentifizierungsvorgang in einer Xamarin.Forms-Anwendung verwalten._

## <a name="overview"></a>Übersicht

Der Prozess für die Verwendung von Azure Mobile Apps im Authentifizierungsprozess sorgt in einer Xamarin.Forms-Anwendung verwalten lautet wie folgt:

1. Registrieren Sie Ihre Azure-Mobile-Anwendung auf der Website des Identitätsanbieters, und legen Sie dann die Anmeldeinformationen vom Anbieter erstellte im Back-End für Mobile Apps. Weitere Informationen finden Sie unter [Registrieren Ihrer app für die Authentifizierung und App-Dienste konfigurieren](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services).
1. Definieren Sie ein neue URL-Schema für die Xamarin.Forms-Anwendung, wodurch das Authentifizierungssystem an Xamarin.Forms Anwendung umgeleitet werden soll, nachdem der Authentifizierungsprozess abgeschlossen ist. Weitere Informationen finden Sie unter [die zulässige externen Umleitungs-URLs Ihrer app hinzufügen](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl).
1. Beschränken des Zugriffs auf das Azure Mobile Apps-Back-End, nur authentifizierte Clients. Weitere Informationen finden Sie unter [schränken Sie die Berechtigungen an authentifizierte Benutzer](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users).
1. Rufen Sie die Authentifizierung über die Xamarin.Forms-Anwendung. Weitere Informationen finden Sie unter [Hinzufügen von Authentifizierung zu der portablen Klassenbibliothek](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library), [Hinzufügen von Authentifizierung zu iOS-app](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app), [Hinzufügen von Authentifizierung zu Android-app](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app), und [ Hinzufügen von Authentifizierung zu Windows 10-app-Projekte](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects).

> [!NOTE]
> In iOS 9 und höher erzwingt App-Transport-Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern. Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.
> ATS können von hinzugewählt werden, ist es nicht möglich ist, verwenden Sie die `HTTPS` -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).

In der Vergangenheit haben mobile Anwendungen eingebetteten Webansichten verwendet, zum Durchführen der Authentifizierung mit des Identitätsanbieters. Dies wird nicht mehr für den folgenden Gründen empfohlen:

- Die Anwendung, die der Webseitenansicht hostet, kann Anmeldeinformationen für den vollständigen Authentifizierung des Benutzers, nicht nur die autorisierungserteilung zugreifen, die für die Anwendung bestimmt wurde. Dies verstößt gegen das Prinzip der geringsten Rechte, wie die Anwendung Zugriff auf leistungsfähigeren Anmeldeinformationen hat als potenziell erhöht die Angriffsfläche der Anwendung erfordert.
- Die hostanwendung konnte erfassen, Benutzernamen und Kennwörter, automatisch Formulare und benutzerzustimmung, umgehen und Sitzungscookies kopieren und verwenden, um authentifizierte Aktionen als Benutzer ausführen.
- Eingebettete Webansichten freigeben nicht des Authentifizierungsstatus mit anderen Anwendungen oder Webbrowser des Geräts, da der Benutzer für jede autorisierungsanforderung anmelden, die ein kleiner als benutzererfahrung betrachtet wird.
- Einige Autorisierungs-Endpunkten Schritte zu erkennen und zu blockieren Autorisierung-Anforderungen, die von Webansichten stammen.

Die Alternative ist die Verwendung Webbrowser des Geräts zum Durchführen der Authentifizierung, also der Ansatz, die neueste Version der [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/). Die Verwendbarkeit einer Anwendung mit dem Gerätebrowser authentifizierungsanforderungen verbessert werden, da die Benutzer müssen nur zur Anmeldung an den Identitätsanbieter einmal pro Gerät Wechselkurse der Anmeldung und Autorisierung Datenflüsse in der Anwendung zu verbessern. Die Gerätebrowser bietet verbesserte Sicherheit auch, wie Anwendungen, um zu überprüfen und Ändern von Inhalt in einer Webansicht, aber nicht die im Browser angezeigte Inhalt können.

## <a name="using-an-azure-mobile-apps-instance"></a>Verwenden einer Instanz von Azure Mobile Apps

Die [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) bietet die `MobileServiceClient` Klasse, die von einer Xamarin.Forms-Anwendung verwendet wird, Zugriff auf die Azure-Mobile Apps-Instanz.

Die beispielanwendung verwendet die Google als Identitätsanbieter, die den Benutzern mit Google-Konten anmelden, um die Xamarin.Forms-Anwendung. Während Sie Google als Identitätsanbieter in diesem Artikel verwendet wird, ist der Ansatz gleichermaßen anwendbar für anderer Identitätsanbieter entgegen.

<a name="logging-in" />

### <a name="logging-in-users"></a>Anmeldung von Benutzern

Anmeldebildschirm in der beispielanwendung wird in den folgenden Screenshots veranschaulicht:

![](azure-images/login.png "Anmeldeseite")

Während Sie Google als Identitätsanbieter verwendet wird, kann eine Vielzahl von anderer Identitätsanbieter entgegen verwendet werden, einschließlich Facebook, Microsoft, Twitter und Azure Active Directory.

Im folgenden Codebeispiel wird veranschaulicht, wie der Anmeldevorgang aufgerufen wird:

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

Die `App.Authenticator` Eigenschaft ist ein `IAuthenticate` -Instanz, die von jeder plattformspezifisches Projekt festgelegt wird. Die `IAuthenticate` Schnittstelle gibt ein `AuthenticateAsync` Vorgang, die von jeder plattformprojekt bereitgestellt werden müssen. Deshalb Aufrufen der `App.Authenticator.AuthenticateAsync` Methode führt die `IAuthenticate.AuthenticateAsync` -Methode in einem plattformprojekt.

Alle von der Plattform `IAuthenticate.AuthenticateAsync` Aufruf der Methoden der `MobileServiceClient.LoginAsync` Methode, um eine Schnittstelle und Cache Anmeldedaten anzuzeigen. Das folgende Codebeispiel zeigt die `LoginAsync` Methode für die iOS-Plattform:

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

Das folgende Codebeispiel zeigt die `LoginAsync` Methode für die Android-Plattform:

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

Auf allen Plattformen die `MobileServiceAuthenticationProvider` Enumeration wird verwendet, um den Identitätsanbieter, die verwendet werden, bei der Authentifizierung angeben. Wenn die `MobileServiceClient.LoginAsync` Methode wird aufgerufen, Azure Mobile Apps initiiert eine Authentifizierungsablauf durch Anzeigen der Anmeldeseite des ausgewählten Anbieters und generieren ein Authentifizierungstoken nach der erfolgreichen Anmeldung mit dem Identitätsanbieter. Die `MobileServiceClient.LoginAsync` Methode gibt ein `MobileServiceUser` -Instanz, die in gespeichert werden, die `MobileServiceClient.CurrentUser` Eigenschaft. Diese Eigenschaft ermöglicht `UserId` und `MobileServiceAuthenticationToken` Eigenschaften. Diese repräsentieren die authentifizierten Benutzer und ein Authentifizierungstoken für den Benutzer. In aller erfolgten Anforderungen an die Azure Mobile Apps-Instanz, die Xamarin.Forms Anwendung zum Ausführen von Aktionen auf der Azure-Mobile-App-Instanz, die authentifizierten Benutzerberechtigungen erfordern, wird das Authentifizierungstoken enthalten sein.

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

Die `App.Authenticator` Eigenschaft ist ein `IAuthenticate` -Instanz, die von jeder Platformproject festgelegt wird. Die `IAuthenticate` Schnittstelle gibt ein `LogoutAsync` Vorgang, die von jeder plattformprojekt bereitgestellt werden müssen. Deshalb Aufrufen der `App.Authenticator.LogoutAsync` Methode führt die `IAuthenticate.LogoutAsync` -Methode in einem plattformprojekt.

Alle von der Plattform `IAuthenticate.LogoutAsync` Aufruf der Methoden der `MobileServiceClient.LogoutAsync` Methode, um dem angemeldeten Benutzer mit dem Identitätsanbieter Deduplizierung zu authentifizieren. Das folgende Codebeispiel zeigt die `LogoutAsync` Methode für die iOS-Plattform:

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

Das folgende Codebeispiel zeigt die `LogoutAsync` Methode für die Android-Plattform:

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

Wenn die `IAuthenticate.LogoutAsync` Methode aufgerufen wird, alle Cookies vom Identitätsanbieter festgelegt deaktiviert sind, bevor die `MobileServiceClient.LogoutAsync` Methode wird aufgerufen, um den angemeldeten Benutzer mit dem Identitätsanbieter Deduplizierung zu authentifizieren.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Azure Mobile Apps zu verwenden, um den Authentifizierungsvorgang in einer Xamarin.Forms-Anwendung verwalten. Azure Mobile Apps verwenden eine Vielzahl von externen Identitätsanbieter zum Authentifizieren und autorisieren Benutzer der Anwendung, einschließlich Facebook, Google, Microsoft, Twitter und Azure Active Directory zu unterstützen. Die `MobileServiceClient` Klasse wird von der Anwendung Xamarin.Forms zum Steuern des Zugriffs auf die Azure-Mobile Apps-Instanz verwendet.


## <a name="related-links"></a>Verwandte Links

- [TodoAzureAuth (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Verwenden einer mobilen Anwendung für Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Hinzufügen von Authentifizierung zu Ihrer app Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
