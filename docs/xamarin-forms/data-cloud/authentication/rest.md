---
title: Die Authentifizierung eines RESTful-Web-Diensts
description: Die Standardauthentifizierung bietet Zugriff auf Ressourcen auf ausschließlich Clients, die die richtigen Anmeldeinformationen verfügen. In diesem Artikel erläutert die Standardauthentifizierung zu verwenden, um den Zugriff auf RESTful-Web-Service-Ressourcen zu schützen.
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: e2ab6c053901ad6c1668c5ae5be9ab04d9d05e8a
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050335"
---
# <a name="authenticating-a-restful-web-service"></a>Die Authentifizierung eines RESTful-Web-Diensts

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)

_HTTP unterstützt die Verwendung mehrere Authentifizierungsmechanismen zum Steuern des Zugriffs auf Ressourcen. Die Standardauthentifizierung bietet Zugriff auf Ressourcen auf ausschließlich Clients, die die richtigen Anmeldeinformationen verfügen. In diesem Artikel veranschaulicht die Standardauthentifizierung zu verwenden, um den Zugriff auf RESTful-Web-Service-Ressourcen zu schützen._

Die begleitende Xamarin.Forms-beispielanwendung verwendet einen Xamarin-gehosteten REST-Dienst, der schreibgeschützten Zugriff auf den Webdienst bereitstellt. Aus diesem Grund werden die Vorgänge, die zu erstellen, aktualisieren und Löschen von Daten nicht geändert werden die Daten in der Anwendung genutzt. Eine hostfähigen Version der REST-Dienst ist jedoch verfügbar, in der *TodoRESTService* Ordner in der beispielanwendung, und Anweisungen zum Einrichten des Diensts finden Sie es. Diese hostfähigen Version der REST-Dienst bietet vollständige erstellen, aktualisieren, lesen und Löschen der Zugriff auf die Daten.

> [!NOTE]
> In iOS 9 und höher erzwingt (App Transport Security, ATS) sichere Verbindungen zwischen der Internet-Ressourcen (z. B. die app Back-End-Server) und die app, und verhindert versehentliche Offenlegung vertraulicher Informationen. Da ATS in apps für iOS 9, die standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS-sicherheitsanforderungen. Wenn die Verbindungen nicht über diese Anforderungen erfüllen, werden sie mit einer Ausnahme fehlschlagen.
> ATS können von entschieden werden, ist dies nicht möglich, verwenden Sie die `HTTPS` -Protokolls und Sichern der Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, durch die Aktualisierung der app **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="authenticating-users-over-http"></a>Authentifizieren von Benutzern über HTTP

Standardauthentifizierung ist der einfachste Authentifizierungsmechanismus, der von HTTP unterstützten und den Client sendet den Benutzernamen und das Kennwort als unverschlüsselten base64-codierte Text umfasst. Dies funktioniert wie folgt:

- Wenn ein Webdienst eine Anforderung für eine geschützte Ressource empfängt, lehnt die Anforderung mit HTTP-Statuscode 401 (Zugriff verweigert) und den WWW-Authenticate Response Header legt fest, wie im folgenden Diagramm dargestellt:

![](rest-images/basic-authentication-fail.png "Standardauthentifizierung ein")

- Wenn ein Webdienst eine Anforderung für eine geschützte Ressource, mit erhält die `Authorization` Header ordnungsgemäß festgelegt, das Web-Dienst antwortet mit einem HTTP-Statuscode 200, der angibt, dass die Anforderung erfolgreich war und die angeforderte Informationen in der Antwort ist. Dieses Szenario ist in der folgenden Abbildung dargestellt:

![](rest-images/basic-authentication-success.png "Adressiert, der Standardauthentifizierung")

> [!NOTE]
> Standardauthentifizierung sollte nur über eine HTTPS-Verbindung verwendet werden. Wenn Sie über eine HTTP-Verbindung verwendet das <code>Authorization</code> Header einfach decodiert werden kann, wenn ein Angreifer der HTTP-Datenverkehr erfasst wird.

## <a name="specifying-basic-authentication-in-a-web-request"></a>Angeben von Standardauthentifizierung in einer Webanforderung

Verwendung der Standardauthentifizierung wird wie folgt angegeben:

1. Die Zeichenfolge "Basic", um hinzugefügt wird die `Authorization` -Header der Anforderung.
1. Benutzername und Kennwort werden in eine Zeichenfolge mit dem Format "Benutzername: Kennwort", die base64-codiert und hinzugefügt, dann kann kombiniert die `Authorization` -Header der Anforderung.

Aus diesem Grund mit dem Benutzernamen 'XamarinUser' und dem Kennwort "XamarinPassword" wird der Header:

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

Die `HttpClient` -Klasse festgelegt kann die `Authorization` Headerwert mit der `HttpClient.DefaultRequestHeaders.Authorization` Eigenschaft. Da der `HttpClient` Instanz vorhanden ist, über mehrere Anforderungen hinweg der `Authorization` Header nur einmal statt bei festgelegt werden muss jede Anforderung ausgeben, wie im folgenden Codebeispiel gezeigt:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    client = new HttpClient ();
    ...
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

Und dann die Anforderung mit, wenn eine Anforderung an einen Webdienstvorgang erfolgt signiert wird die `Authorization` -Header, der angibt, ob der Benutzer über die Berechtigung zum Aufrufen des Vorgangs verfügt.

> [!NOTE]
> Während der Beispiel – REST-Dienst als Konstanten Anmeldeinformationen gespeichert werden, sollten sie in einem unsicheren Format in eine veröffentlichte Anwendung nicht gespeichert werden. Die [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet bietet Funktionen zum sicheren Speichern von Anmeldeinformationen. Weitere Informationen finden Sie unter [speichern und Abrufen von Informationen auf Geräten](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="processing-the-authorization-header-server-side"></a>Verarbeiten von der Serverseite der Authorization-Header

Die begleitende Beispiel – REST-Dienst ergänzt jede Aktion wird mit der `[BasicAuthentication]` Attribut. Dieses Attribut wird implementiert, indem die `BasicAuthenticationAttribute` -Klasse in der Projektmappe, und wird verwendet, um das Analysieren der `Authorization` Header und zu bestimmen, ob die base64-codierte Anmeldeinformationen gültig sind, durch einen Vergleich mit Werten, die in gespeicherten *"Web.config"*. Während dieser Ansatz für den Beispieldienst geeignet ist, ist die Erweiterung für einen Webdienst öffentlich zugänglichen erforderlich.

Im basic-Authentifizierung-Modul von IIS verwendet wird werden Benutzer anhand ihrer Windows-Anmeldeinformationen authentifiziert. Aus diesem Grund müssen Benutzer über Konten in der Domäne des Servers verfügen. Allerdings kann das Modell für die Standardauthentifizierung konfiguriert werden, um benutzerdefinierte Authentifizierung zu ermöglichen, in denen Benutzerkonten für eine externe Quelle, z. B. eine Datenbank authentifiziert. Weitere Informationen finden Sie unter [grundlegenden Authentifizierung in ASP.NET Web-API](http://www.asp.net/web-api/overview/security/basic-authentication) auf der ASP.NET-Website.

> [!NOTE]
> Standardauthentifizierung wurde nicht entwickelt, um durch die Abmeldung zu verwalten. Daher ist der standard-Standardauthentifizierung Ansatz für die Abmeldung zum Beenden der Sitzung.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Hinzufügen von Standardauthentifizierung zu Anforderungen, die von einer Xamarin.Forms-Anwendung mithilfe der `HttpClient` Klasse. Die Standardauthentifizierung bietet Zugriff auf Ressourcen auf ausschließlich Clients, die die richtigen Anmeldeinformationen verfügen. Informationen zur Verwendung von [Xamarin.Auth](https://www.nuget.org/packages/Xamarin.Auth/) um des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung, damit Benutzer ein Back-End gemeinsam nutzen können, während Sie nur Zugriff auf ihre Daten zu verwalten, finden Sie unter [Authentifizieren von Benutzern Bei einem Identitätsanbieter](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="related-links"></a>Verwandte Links

- [TodoREST (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [Verwenden eines RESTful-Web-Diensts](~/xamarin-forms/data-cloud/consuming/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
