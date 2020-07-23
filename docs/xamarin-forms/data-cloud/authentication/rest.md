---
title: Authentifizieren eines Rest-Webdiensts
description: Die Standard Authentifizierung ermöglicht nur Clients, die über die richtigen Anmelde Informationen verfügen, Zugriff auf Ressourcen. In diesem Artikel wird erläutert, wie die Standard Authentifizierung verwendet wird, um den Zugriff auf Rest-Webdienst Ressourcen zu schützen
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a46d099520103ba6f1552d5bda3f619c07b743d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928522"
---
# <a name="authenticate-a-restful-web-service"></a>Authentifizieren eines Rest-Webdiensts

_HTTP unterstützt die Verwendung mehrerer Authentifizierungsmechanismen, um den Zugriff auf Ressourcen zu steuern. Die Standard Authentifizierung ermöglicht nur Clients, die über die richtigen Anmelde Informationen verfügen, Zugriff auf Ressourcen. In diesem Artikel wird veranschaulicht, wie die Standard Authentifizierung verwendet wird, um den Zugriff auf Rest-Webdienst Ressourcen zu schützen_

> [!NOTE]
> In ios 9 und höher erzwingt App-Transport Sicherheit (app Transport Security, ATS) sichere Verbindungen zwischen Internetressourcen (z. b. dem Back-End-Server der APP) und der APP, wodurch eine versehentliche Offenlegung vertraulicher Informationen verhindert wird. Da ATS in apps, die für IOS 9 erstellt wurden, standardmäßig aktiviert ist, unterliegen alle Verbindungen den Sicherheitsanforderungen. Wenn Verbindungen diese Anforderungen nicht erfüllen, können Sie mit einer Ausnahme fehlschlagen.
> Die Ate können deaktiviert werden, wenn es nicht möglich ist, das `HTTPS` Protokoll und die sichere Kommunikation für Internetressourcen zu verwenden. Dies kann durch Aktualisieren der Datei " **Info. plist** " der APP erreicht werden. Weitere Informationen finden Sie unter [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md).

## <a name="authenticating-users-over-http"></a>Authentifizieren von Benutzern über http

Die Standard Authentifizierung ist der einfachste von http unterstützte Authentifizierungsmechanismus und erfordert, dass der Client den Benutzernamen und das Kennwort als unverschlüsselten Base64-codierten Text sendet. Dies funktioniert wie folgt:

- Wenn ein Webdienst eine Anforderung für eine geschützte Ressource empfängt, lehnt er die Anforderung mit dem HTTP-Statuscode 401 (Zugriff verweigert) ab und legt den WWW-Authenticate-Antwortheader fest, wie im folgenden Diagramm dargestellt:

![Fehler bei der Standard Authentifizierung](rest-images/basic-authentication-fail.png)

- Wenn ein Webdienst eine Anforderung für eine geschützte Ressource empfängt und der- `Authorization` Header ordnungsgemäß festgelegt ist, antwortet der Webdienst mit dem HTTP-Statuscode 200, der angibt, dass die Anforderung erfolgreich war und die angeforderten Informationen in der Antwort angezeigt werden. Dieses Szenario ist im folgenden Diagramm dargestellt:

![Standard Authentifizierung erfolgreich](rest-images/basic-authentication-success.png)

> [!NOTE]
> Die Standard Authentifizierung sollte nur über eine HTTPS-Verbindung verwendet werden. Wenn Sie über eine HTTP-Verbindung verwendet wird, `Authorization` kann der Header problemlos decodiert werden, wenn der HTTP-Datenverkehr von einem Angreifer aufgezeichnet wird.

## <a name="specifying-basic-authentication-in-a-web-request"></a>Angeben der Standard Authentifizierung in einer Webanforderung

Die Verwendung der Standard Authentifizierung wird wie folgt angegeben:

1. Die Zeichenfolge "Basic" wird dem `Authorization` Header der Anforderung hinzugefügt.
1. Der Benutzername und das Kennwort werden in einer Zeichenfolge mit dem Format "username: Password" kombiniert, das dann Base64-codiert und dem `Authorization` Header der Anforderung hinzugefügt wird.

Daher wird der Header mit dem Benutzernamen ' xamarinuser ' und dem Kennwort ' xamarinpassword ' wie folgt angezeigt:

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

Die- `HttpClient` Klasse kann den- `Authorization` Header Wert für die- `HttpClient.DefaultRequestHeaders.Authorization` Eigenschaft festlegen. Da die- `HttpClient` Instanz über mehrere Anforderungen hinweg vorhanden ist, `Authorization` muss der Header nur einmal festgelegt werden, anstatt jede Anforderung zu erstellen, wie im folgenden Codebeispiel gezeigt:

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

Wenn eine Anforderung an einen Webdienst Vorgang durchgeführt wird, wird die Anforderung mit dem- `Authorization` Header signiert, was angibt, ob der Benutzer über die Berechtigung zum Aufrufen des Vorgangs verfügt.

> [!IMPORTANT]
> Während dieser Code Anmelde Informationen als Konstanten speichert, sollten Sie nicht in einem unsicheren Format in einer veröffentlichten Anwendung gespeichert werden.

## <a name="processing-the-authorization-header-server-side"></a>Verarbeiten der Server Seite des Autorisierungs Headers

Der Rest-Dienst muss jede Aktion mit dem- `[BasicAuthentication]` Attribut ergänzen. Dieses Attribut wird verwendet, um den `Authorization` -Header zu analysieren und zu bestimmen, ob die Base64-codierten Anmelde Informationen gültig sind, indem Sie mit in *Web.config*gespeicherten Werten verglichen werden. Obwohl dieser Ansatz für einen Beispiel Dienst geeignet ist, ist eine Erweiterung für einen öffentlich ausgerichteten Webdienst erforderlich.

Im Standard Authentifizierungs Modul, das von IIS verwendet wird, werden die Benutzer mit Ihren Windows-Anmelde Informationen authentifiziert. Daher müssen Benutzer über Konten in der Domäne des Servers verfügen. Das Standard Authentifizierungs Modell kann jedoch so konfiguriert werden, dass eine benutzerdefinierte Authentifizierung zulässig ist, bei der Benutzerkonten für eine externe Quelle (z. b. eine Datenbank) authentifiziert werden. Weitere Informationen finden Sie unter Standard [Authentifizierung in ASP.net-Web-API](https://www.asp.net/web-api/overview/security/basic-authentication) auf der ASP.NET-Website.

> [!NOTE]
> Die Standard Authentifizierung wurde nicht für die Verwaltung der Protokollierung konzipiert. Daher ist der standardmäßige Standard Authentifizierungs Ansatz für die Abmeldung das Beenden der Sitzung.

## <a name="related-links"></a>Ähnliche Themen

- [Nutzen eines Rest-Webdiensts](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
