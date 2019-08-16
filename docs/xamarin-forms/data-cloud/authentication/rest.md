---
title: Authentifizieren eines Rest-Webdiensts
description: Die Standardauthentifizierung bietet Zugriff auf Ressourcen auf ausschließlich Clients, die die richtigen Anmeldeinformationen verfügen. In diesem Artikel erläutert die Standardauthentifizierung zu verwenden, um den Zugriff auf RESTful-Web-Service-Ressourcen zu schützen.
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: 5a0e820c8a9f04b7ad9173893852285d53dbe7a6
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529205"
---
# <a name="authenticate-a-restful-web-service"></a>Authentifizieren eines Rest-Webdiensts

_HTTP unterstützt die Verwendung mehrere Authentifizierungsmechanismen zum Steuern des Zugriffs auf Ressourcen. Die Standardauthentifizierung bietet Zugriff auf Ressourcen auf ausschließlich Clients, die die richtigen Anmeldeinformationen verfügen. In diesem Artikel veranschaulicht die Standardauthentifizierung zu verwenden, um den Zugriff auf RESTful-Web-Service-Ressourcen zu schützen._

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
> Standardauthentifizierung sollte nur über eine HTTPS-Verbindung verwendet werden. Wenn Sie über eine HTTP-Verbindung verwendet das `Authorization` Header einfach decodiert werden kann, wenn ein Angreifer der HTTP-Datenverkehr erfasst wird.

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
  HttpClient _client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    _client = new HttpClient ();
    _client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

Und dann die Anforderung mit, wenn eine Anforderung an einen Webdienstvorgang erfolgt signiert wird die `Authorization` -Header, der angibt, ob der Benutzer über die Berechtigung zum Aufrufen des Vorgangs verfügt.

> [!NOTE]
> Während dieser Code Anmelde Informationen als Konstanten speichert, sollten Sie nicht in einem unsicheren Format in einer veröffentlichten Anwendung gespeichert werden. Die [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet bietet Funktionen zum sicheren Speichern von Anmeldeinformationen. Weitere Informationen finden Sie unter [speichern und Abrufen von Informationen auf Geräten](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="processing-the-authorization-header-server-side"></a>Verarbeiten von der Serverseite der Authorization-Header

Der Rest-Dienst muss jede Aktion mit dem `[BasicAuthentication]` -Attribut ergänzen. Dieses Attribut wird verwendet, um den `Authorization` Header zu analysieren und zu bestimmen, ob die Base64-codierten Anmelde Informationen gültig sind, indem Sie mit den in " *Web. config*" gespeicherten Werten verglichen werden Obwohl dieser Ansatz für einen Beispiel Dienst geeignet ist, ist eine Erweiterung für einen öffentlich ausgerichteten Webdienst erforderlich.

Im basic-Authentifizierung-Modul von IIS verwendet wird werden Benutzer anhand ihrer Windows-Anmeldeinformationen authentifiziert. Aus diesem Grund müssen Benutzer über Konten in der Domäne des Servers verfügen. Allerdings kann das Modell für die Standardauthentifizierung konfiguriert werden, um benutzerdefinierte Authentifizierung zu ermöglichen, in denen Benutzerkonten für eine externe Quelle, z. B. eine Datenbank authentifiziert. Weitere Informationen finden Sie unter [grundlegenden Authentifizierung in ASP.NET Web-API](http://www.asp.net/web-api/overview/security/basic-authentication) auf der ASP.NET-Website.

> [!NOTE]
> Standardauthentifizierung wurde nicht entwickelt, um durch die Abmeldung zu verwalten. Daher ist der standard-Standardauthentifizierung Ansatz für die Abmeldung zum Beenden der Sitzung.

## <a name="related-links"></a>Verwandte Links

- [Nutzen eines Rest-Webdiensts](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
