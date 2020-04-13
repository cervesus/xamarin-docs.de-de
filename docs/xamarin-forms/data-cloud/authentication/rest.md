---
title: Authentifizieren eines RESTful-Webdienstes
description: Die Standardauthentifizierung bietet nur den Clients, die über die richtigen Anmeldeinformationen verfügen, Zugriff auf Ressourcen. In diesem Artikel wird erläutert, wie Sie die Standardauthentifizierung verwenden, um den Zugriff auf RESTful-Webdienstressourcen zu schützen.
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
ms.openlocfilehash: 65606b72c3944809a09b8c70b9cdbb5524e0f856
ms.sourcegitcommit: 2a8eb8bce427e72d4e7edd06ce432e19f17dcdd7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388589"
---
# <a name="authenticate-a-restful-web-service"></a>Authentifizieren eines RESTful-Webdienstes

_HTTP unterstützt die Verwendung mehrerer Authentifizierungsmechanismen, um den Zugriff auf Ressourcen zu steuern. Die Standardauthentifizierung bietet nur den Clients, die über die richtigen Anmeldeinformationen verfügen, Zugriff auf Ressourcen. In diesem Artikel wird veranschaulicht, wie Sie die Standardauthentifizierung verwenden, um den Zugriff auf RESTful-Webdienstressourcen zu schützen._

> [!NOTE]
> In iOS 9 und höher erzwingt App Transport Security (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. dem Back-End-Server der App) und der App, wodurch eine versehentliche Offenlegung vertraulicher Informationen verhindert wird. Da ATS standardmäßig in Apps für iOS 9 aktiviert ist, unterliegen alle Verbindungen den ATS-Sicherheitsanforderungen. Wenn Verbindungen diese Anforderungen nicht erfüllen, schlagen sie mit einer Ausnahme fehl.
> ATS kann ausgeschlossen werden, wenn es nicht `HTTPS` möglich ist, das Protokoll und sichere Kommunikation für Internetressourcen zu verwenden. Dies kann durch Die Aktualisierung der **App Info.plist-Datei** erreicht werden. Weitere Informationen finden Sie unter [App Transport Security](~/ios/app-fundamentals/ats.md).

## <a name="authenticating-users-over-http"></a>Authentifizieren von Benutzern über HTTP

Die Standardauthentifizierung ist der einfachste Authentifizierungsmechanismus, der von HTTP unterstützt wird, und umfasst, dass der Client den Benutzernamen und das Kennwort als unverschlüsselten base64-codierten Text sendet. Dies funktioniert wie folgt:

- Wenn ein Webdienst eine Anforderung für eine geschützte Ressource empfängt, lehnt er die Anforderung mit dem HTTP-Statuscode 401 (Zugriff verweigert) ab und legt den ANTWORTheader WWW-Authenticate fest, wie im folgenden Diagramm dargestellt:

![](rest-images/basic-authentication-fail.png "Basic Authentication Failing")

- Wenn ein Webdienst eine Anforderung für eine `Authorization` geschützte Ressource empfängt und der Header korrekt festgelegt ist, antwortet der Webdienst mit einem HTTP-Statuscode 200, der angibt, dass die Anforderung erfolgreich war und dass sich die angeforderten Informationen in der Antwort befinden. Dieses Szenario wird im folgenden Diagramm dargestellt:

![](rest-images/basic-authentication-success.png "Basic Authentication Succeeding")

> [!NOTE]
> Die Standardauthentifizierung sollte nur über eine HTTPS-Verbindung verwendet werden. Bei Verwendung über eine `Authorization` HTTP-Verbindung kann der Header einfach decodiert werden, wenn der HTTP-Datenverkehr von einem Angreifer erfasst wird.

## <a name="specifying-basic-authentication-in-a-web-request"></a>Angeben der Standardauthentifizierung in einer Webanforderung

Die Verwendung der Standardauthentifizierung wird wie folgt angegeben:

1. Die Zeichenfolge "Basic" wird `Authorization` dem Header der Anforderung hinzugefügt.
1. Der Benutzername und das Kennwort werden zu einer Zeichenfolge mit dem Format "username:password" `Authorization` kombiniert, die dann base64 codiert und dem Header der Anforderung hinzugefügt wird.

Daher wird der Header mit einem Benutzernamen von 'XamarinUser' und dem Kennwort 'XamarinPassword':

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

Die `HttpClient` Klasse kann `Authorization` den Headerwert für die `HttpClient.DefaultRequestHeaders.Authorization` Eigenschaft festlegen. Da `HttpClient` die Instanz über mehrere `Authorization` Anforderungen hinweg vorhanden ist, muss der Header nur einmal festgelegt werden, anstatt bei jeder Anforderung, wie im folgenden Codebeispiel gezeigt:

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

Wenn dann eine Anforderung an einen Webdienstvorgang gestellt `Authorization` wird, wird die Anforderung mit dem Header signiert, was angibt, ob der Benutzer über die Berechtigung zum Aufrufen des Vorgangs verfügt.

> [!IMPORTANT]
> Dieser Code speichert Anmeldeinformationen zwar als Konstanten, sie sollten jedoch nicht in einem unsicheren Format in einer veröffentlichten Anwendung gespeichert werden.

## <a name="processing-the-authorization-header-server-side"></a>Verarbeiten der Berechtigungsheader-Serverseite

Der REST-Dienst sollte jede `[BasicAuthentication]` Aktion mit dem Attribut dekorieren. Dieses Attribut wird verwendet, `Authorization` um den Header zu analysieren und zu bestimmen, ob die base64-codierten Anmeldeinformationen gültig sind, indem sie mit Werten verglichen werden, die in *Web.config*gespeichert sind. Dieser Ansatz eignet sich zwar für einen Beispieldienst, erfordert jedoch eine Erweiterung für einen öffentlich zugänglichen Webdienst.

Im von IIS verwendeten Standardauthentifizierungsmodul werden Benutzer anhand ihrer Windows-Anmeldeinformationen authentifiziert. Daher müssen Benutzer über Konten in der Domäne des Servers verfügen. Das Standardauthentifizierungsmodell kann jedoch so konfiguriert werden, dass eine benutzerdefinierte Authentifizierung zulässig ist, bei der Benutzerkonten für eine externe Quelle, z. B. eine Datenbank, authentifiziert werden. Weitere Informationen finden Sie unter [Standardauthentifizierung in ASP.NET Web-API](https://www.asp.net/web-api/overview/security/basic-authentication) auf der ASP.NET-Website.

> [!NOTE]
> Die Standardauthentifizierung wurde nicht für die Verwaltung des Abmeldens entwickelt. Daher besteht der Standardansatz für die Standardauthentifizierung für das Abmelden darin, die Sitzung zu beenden.

## <a name="related-links"></a>Verwandte Links

- [Verwenden eines RESTful-Webdienstes](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
