---
title: Authentifizieren von RESTful-Webdienst
description: Standardauthentifizierung bietet Zugriff auf Ressourcen, um nur die Clients, die über die richtigen Anmeldeinformationen verfügen. In diesem Artikel erläutert die Standardauthentifizierung verwenden, um den Zugriff auf Ressourcen von RESTful-Web-Dienst zu schützen.
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 42680ed8b79560f6f4f9f12892f7da5637a7af16
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240960"
---
# <a name="authenticating-a-restful-web-service"></a>Authentifizieren von RESTful-Webdienst

_HTTP unterstützt die Verwendung von mehreren Authentifizierungsmechanismen zum Steuern des Zugriffs auf Ressourcen. Standardauthentifizierung bietet Zugriff auf Ressourcen, um nur die Clients, die über die richtigen Anmeldeinformationen verfügen. In diesem Artikel veranschaulicht die Standardauthentifizierung verwenden, um den Zugriff auf Ressourcen von RESTful-Web-Dienst zu schützen._

Die begleitende Xamarin.Forms-beispielanwendung nutzt einen Xamarin-gehostete REST-Dienst, der nur-Lese Zugriff auf den Webdienst bereitstellt. Aus diesem Grund werden die Vorgänge, die zu erstellen, aktualisieren und Löschen von Daten die in der Anwendung verwendeten Daten nicht geändert. Eine hostfähige Version der REST-Dienst ist jedoch verfügbar, in der *TodoRESTService* Ordner, in der beispielanwendung und Anweisungen zum Einrichten des Diensts kann es gefunden werden. Diese hostfähigen Version der REST-Dienst bietet vollständige erstellen, aktualisieren, lesen und löschen den Zugriff auf die Daten.

> [!NOTE]
> In iOS 9 und höher erzwingt App-Transport-Sicherheit (ATS) sichere Verbindungen zwischen Internetressourcen (z. B. die app-Back-End-Server) und der app, um das unbeabsichtigten Weitergabe von vertraulichen Informationen zu verhindern. Da ATS in apps für iOS 9 erstellt standardmäßig aktiviert ist, werden alle Verbindungen unterliegen ATS sicherheitsanforderungen. Wenn Verbindungen mit diesen Anforderungen nicht erfüllen, werden sie mit einer Ausnahme fehl.
> ATS können von hinzugewählt werden, ist es nicht möglich ist, verwenden Sie die `HTTPS` -Protokolls und sichere Kommunikation für Ressourcen im Internet. Dies kann erreicht werden, indem Sie die app aktualisiert **"Info.plist"** Datei. Weitere Informationen finden Sie unter [App Transportsicherheit](~/ios/app-fundamentals/ats.md).

## <a name="authenticating-users-over-http"></a>Authentifizieren von Benutzern über HTTP

Standardauthentifizierung ist die einfachste unterstützt von HTTP-Authentifizierungsmechanismus und umfasst die den Client sendet den Benutzernamen und das Kennwort als unverschlüsselten base64-codierte Text. Es funktioniert wie folgt:

- Wenn Sie ein Webdienst eine Anforderung für eine geschützte Ressource empfängt, lehnt die Anforderung mit HTTP-Statuscode 401 (Zugriff verweigert) und der WWW-Authenticate-Antwortheader, legt fest, wie im folgenden Diagramm dargestellt:

![](rest-images/basic-authentication-fail.png "Standardauthentifizierung fehlerhaften")

- Wenn Sie ein Webdienst eine Anforderung für eine geschützte Ressource mit empfängt die `Authorization` Header ordnungsgemäß festgelegt, der Web-Dienst antwortet mit einem HTTP-Statuscode 200 gibt an, dass die Anforderung erfolgreich war und dass die angeforderte Informationen in der Antwort. Dieses Szenario ist in der folgenden Abbildung dargestellt:

![](rest-images/basic-authentication-success.png "Adressiert, der Standardauthentifizierung")

> [!NOTE]
> Standardauthentifizierung sollte nur über eine HTTPS-Verbindung verwendet werden. Wenn Sie über eine HTTP-Verbindung verwendet das <code>Authorization</code> Header kann problemlos decodiert werden, wenn der HTTP-Datenverkehr von einem Angreifer erfasst wird.

## <a name="specifying-basic-authentication-in-a-web-request"></a>Angeben von Standardauthentifizierung in einer Webanforderung

Verwendung der Standardauthentifizierung wird wie folgt angegeben:

1. Die Zeichenfolge "Basis", um hinzugefügt wird die `Authorization` -Header der Anforderung.
1. Benutzername und Kennwort werden in eine Zeichenfolge mit dem Format "Benutzername", die base64-codiert und hinzugefügt, dann ist kombiniert die `Authorization` -Header der Anforderung.

Daher wird mit dem Benutzernamen "XamarinUser" und dem Kennwort "XamarinPassword", der Header:

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

Die `HttpClient` "Class" festgelegt werden kann die `Authorization` Headerwert für die `HttpClient.DefaultRequestHeaders.Authorization` Eigenschaft. Da die `HttpClient` Instanz vorhanden ist, in mehreren Anforderungen der `Authorization` Header muss nur einmal festgelegt werden, anstatt Wenn jeder Anforderung ausgeben, wie im folgenden Codebeispiel gezeigt:

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

Und dann die Anforderung mit, wenn eine Anforderung an einen Webdienstvorgang erfolgt signiert wird die `Authorization` -Header, der angibt, und zwar unabhängig davon, ob der Benutzer über die Berechtigung zum Aufrufen des Vorgangs verfügt.

> [!NOTE]
> Während der REST-Beispieldienst Anmeldeinformationen als Konstanten speichert, sollten sie nicht in einem unsicheren Format in eine veröffentlichte Anwendung gespeichert werden. Die [Xamarith.Auth](https://www.nuget.org/packages/Xamarin.Auth/) NuGet bietet Funktionen zum sicheren Speichern von Anmeldeinformationen. Weitere Informationen finden Sie unter [speichern und Abrufen von Kontoinformationen auf Geräten](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="processing-the-authorization-header-server-side"></a>Verarbeiten von der Serverseite der Authorization-Header

Die begleitende REST-Beispieldienst ergänzt jede Aktion mit den `[BasicAuthentication]` Attribut. Dieses Attribut wird dadurch implementiert, die `BasicAuthenticationAttribute` -Klasse in der Projektmappe, und wird verwendet, um das Analysieren der `Authorization` Header und zu bestimmen, ob die base64-codierte-Anmeldeinformationen gültig sind, durch die in gespeicherten Werte vergleichen *"Web.config"*. Während dieser Ansatz für den Beispieldienst geeignet ist, ist die Erweiterung für eine öffentliche Webdienst erforderlich.

In der Standardauthentifizierung-Modul von IIS verwendet werden Benutzer in ihrer Windows-Anmeldeinformationen authentifiziert. Daher müssen Benutzer Konten für die Domäne des Servers verfügen. Allerdings kann das Modell für die Standardauthentifizierung konfiguriert werden benutzerdefinierten Authentifizierung zulassen, in dem Benutzerkonten werden in einer externen Quelle, z. B. eine Datenbank authentifiziert. Weitere Informationen finden Sie unter [Standardauthentifizierung in ASP.NET Web API](http://www.asp.net/web-api/overview/security/basic-authentication) auf der ASP.NET-Website.

> [!NOTE]
> Standardauthentifizierung wurde nicht entwickelt, um Abmelden zu verwalten. Daher ist der standard-Standardauthentifizierung Ansatz für die Abmeldung zum Beenden der Sitzung.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel demonstriert, zum Hinzufügen von Standardauthentifizierung zu webanforderungen von einer Xamarin.Forms-Anwendung mithilfe der `HttpClient` Klasse. Standardauthentifizierung bietet Zugriff auf Ressourcen, um nur die Clients, die über die richtigen Anmeldeinformationen verfügen. Informationen zur Verwendung von [Xamarin.Auth](https://www.nuget.org/packages/Xamarin.Auth/) zur Verwaltung des Authentifizierungsprozesses in einer Xamarin.Forms-Anwendung, damit Benutzer eine Back-End-freigeben können, während Sie nur Zugriff auf ihre Daten eine finden Sie unter [Authentifizieren von Benutzern mit einem Identitätsanbieter](~/xamarin-forms/data-cloud/authentication/oauth.md).


## <a name="related-links"></a>Verwandte Links

- [TodoREST (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [Nutzen einen RESTful-Webdienst](~/xamarin-forms/data-cloud/consuming/rest.md)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
