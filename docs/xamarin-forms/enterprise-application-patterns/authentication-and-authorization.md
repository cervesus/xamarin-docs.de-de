---
title: Authentifizierung und Autorisierung
ms.topic: article
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 8b1715c8e7c3e9bb296577acd3d09a0f22488250
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Authentifizierung versteht man Identifikationsinformationen, z. B. Name und Kennwort eines Benutzers abrufen und überprüfen die Anmeldeinformationen anhand einer Autorität. Wenn die Anmeldeinformationen gültig sind, gilt die Entität, die die Anmeldeinformationen übermittelt eine authentifizierte Identität. Sobald eine Identität authentifiziert wurde, bestimmt ein Autorisierungsvorgang an, ob diese Identität auf eine bestimmte Ressource zugreifen kann.

Es gibt viele Ansätze zum Integrieren von Authentifizierung und Autorisierung in einer Xamarin.Forms-app, die Kommunikation mit einer ASP.NET MVC-Webanwendung, einschließlich der Verwendung von ASP.NET Core Identity, externe Authentifizierungsanbieter, z. B. Microsoft, Google, Facebook oder Twitter und Authentifizierung-Middleware. Die eShopOnContainers mobile-Anwendung führt die Authentifizierung und Autorisierung mit einem Microservice Datenvolumes Identität, die IdentityServer 4 verwendet. Die mobile Anwendung fordert von Sicherheitstoken aus IdentityServer, zum Authentifizieren eines Benutzers oder für den Zugriff auf eine Ressource an. Für IdentityServer zum Ausstellen von Token im Auftrag eines Benutzers der Benutzer muss zum IdentityServer anmelden. Allerdings stellt keine IdentityServer eine Benutzeroberfläche oder die Datenbank für die Authentifizierung bereit. Aus diesem Grund wird in der Anwendung eShopOnContainers Verweis ASP.NET Core Identität für diesen Zweck verwendet.

## <a name="authentication"></a>Authentifizierung

Authentifizierung ist erforderlich, wenn eine Anwendung die Identität des aktuellen Benutzers zu kennen muss. ASP.NET Core der primäre Mechanismus für die Identifizierung von Benutzern ist das Mitgliedschaftssystem ASP.NET Core Identität, mit dem Benutzerinformationen in einem Datenspeicher, der vom Entwickler konfiguriert gespeichert. In der Regel wird diesen Datenspeicher ein Store EntityFramework werden, wenn benutzerdefinierte Geschäfte oder Pakete von Drittanbietern verwendet werden können, um Identitätsinformationen in Azure-Speicher, DocumentDB oder anderen Speicherorten zu speichern.

Für die Authentifizierungsszenarien, die nutzen eines Datenspeichers lokaler Benutzer und die Identitätsinformationen zwischen Anforderungen mithilfe von Cookies (wie in ASP.NET MVC-Webanwendungen wird) beibehalten, ist ASP.NET Core Identity eine angemessene Lösung. Allerdings sind Cookies nicht immer eine natürliche Möglichkeit beibehalten und Übertragen von Daten. Beispielsweise müssen eine ASP.NET Core-Web-Anwendung, die REST-Endpunkten verfügbar macht, die aus einer mobilen app zugegriffen werden in der Regel trägertokenauthentifizierung, zu verwenden, da Cookies in diesem Szenario verwendet werden können. Allerdings können trägertoken problemlos abgerufen und in der Authorization-Header der webanforderungen über die mobile app enthalten.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Ausstellen von IdentityServer 4 mit Bearer-Token

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) ist ein open Source-OpenID Connect und OAuth 2.0-Framework für ASP.NET Core, die für viele Authentifizierung und Autorisierung Szenarien einschließlich der Ausgabe von Sicherheitstokens für lokale ASP.NET Core Identity-Benutzer verwendet werden kann.

> [!NOTE]
> OpenID Connect und OAuth 2.0 sind sehr ähnlich, während Sie eine andere Aufgaben.

OpenID Connect ist eine Authentifizierung-Schicht über das OAuth 2.0-Protokoll. OAuth-2 ist ein Protokoll, Anwendungen ermöglicht, Anfordern von Zugriffstoken aus einem Sicherheitstokendienst und für die Kommunkation mit APIs zu verwenden. Diese Delegierung reduziert Komplexität in Clientanwendungen und APIs, da die Authentifizierung und Autorisierung zentralisiert werden können.

Die Kombination von OpenID Connect und OAuth 2.0 kombinieren, die zwei grundlegende Sicherheitsaspekte der Authentifizierung und API-Zugriff und IdentityServer 4 ist eine Implementierung der Protokolle.

In Anwendungen, die direkte Kommunikation von Client-zu-Microservice, z. B. die eShopOnContainers Verweis-Anwendung verwenden eine dedizierte Authentifizierung Microservice fungiert als ein Sicherheitstokendienst (STS) dienen zum Authentifizieren von Benutzern, wie in Abbildung 9-1. Weitere Informationen über die direkte Kommunikation von Client-zu-Microservice finden Sie unter [Kommunikation zwischen Client und Microservices](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Authentifizierung durch eine dedizierte Authentifizierung microservice")

**Abbildung 9 – 1:** Authentifizierung, indem eine dedizierte Authentifizierung Microservice

Die mobile app eShopOnContainers kommuniziert mit der Identity-Microservice, die IdentityServer 4 verwendet, um Authentifizierung und Zugriffssteuerung für APIs. Aus diesem Grund wird die inhaltsortsanfrage der mobilen Anwendung Token IdentityServer, zum Authentifizieren eines Benutzers oder für den Zugriff auf eine Ressource:

-   Authentifizieren von Benutzern mit IdentityServer erfolgt durch die mobile Anwendung anfordern einer *Identität* -Token, das das Ergebnis der Authentifizierung darstellt. Mindestens eine bare enthält es einen Bezeichner für den Benutzer und Informationen wie und wann der Benutzer authentifiziert. Sie können auch zusätzliche Identitätsdaten enthalten.
-   Zugreifen auf eine Ressource mit IdentityServer erfolgt durch die mobile Anwendung anfordern einer *Zugriff* -Token, das Zugriff auf eine API-Ressource ermöglicht. Clients anfordern von Zugriffstoken und an die API weitergeleitet werden. Zugriffstoken enthalten Informationen über den Client und der Benutzer (falls vorhanden). -APIs verwenden, die Informationen dann beim Autorisieren des Zugriffs auf ihre Daten.

> [!NOTE]
> **Hinweis**: ein Client muss mit IdentityServer registriert werden, bevor es Token anfordern kann.

### <a name="adding-identityserver-to-a-web-application"></a>Hinzufügen von IdentityServer zu einer Webanwendung

In der Reihenfolge für eine Webanwendung ASP.NET Core IdentityServer 4 verwenden müssen sie die Webanwendung von Visual Studio-Projektmappe hinzugefügt werden. Weitere Informationen finden Sie unter [Setup- und Übersicht über die](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) in der IdentityServer-Dokumentation.

Nach IdentityServer in Visual Studio-Projektmappe für die Webanwendung enthalten ist, müssen sie die Webanwendung die HTTP-anforderungsverarbeitung Pipeline hinzugefügt werden, damit er die Anforderungen an OpenID Connect und OAuth 2.0-Endpunkte verarbeiten kann. Dies erfolgt der `Configure` Methode in der Webanwendung `Startup` Klasse, wie im folgenden Codebeispiel gezeigt:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Reihenfolge ist wichtig, in der Webanwendung HTTP-Anforderung, die Verarbeitung der Pipeline. Aus diesem Grund muss IdentityServer der Pipeline vor der Benutzeroberflächen-Framework hinzugefügt werden, die den Anmeldebildschirm implementiert.

### <a name="configuring-identityserver"></a>Konfigurieren von IdentityServer

IdentityServer muss konfiguriert werden, der `ConfigureServices` Methode in der Webanwendung `Startup` Klasse durch Aufrufen der `services.AddIdentityServer` -Methode, wie im folgenden Codebeispiel aus der eShopOnContainers Referenz-Anwendung veranschaulicht:

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

Nach dem Aufruf der `services.AddIdentityServer` -Methode, zusätzliche fluent-APIs werden aufgerufen, um Folgendes zu konfigurieren:

-   Anmeldeinformationen, die zum Signieren verwendet werden.
-   API und Identität Ressourcen, die Benutzer anfordern können der Zugriff auf.
-   Clients, die mit Token anzufordern herstellen möchte.
-   ASP.NET Core Identity.

>💡 **Tipp**: dynamisch geladen wird, die IdentityServer 4-Konfiguration. IdentityServer-4-APIs ermöglichen die Konfiguration von IdentityServer aus einer Liste der in-Memory-Konfigurationsobjekte. In der Anwendung eShopOnContainers Verweis sind diese Auflistungen im Arbeitsspeicher an, in der Anwendung hartcodiert. Allerdings können in Produktionsszenarien sie geladen dynamisch aus einer Konfigurationsdatei oder aus einer Datenbank werden.

Informationen zum Konfigurieren der Verwendung von ASP.NET Core Identity IdentityServer finden Sie unter [mithilfe von ASP.NET Core Identity](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) in der IdentityServer-Dokumentation.

#### <a name="configuring-api-resources"></a>Konfigurieren von API-Ressourcen

Beim Konfigurieren von API-Ressourcen, die `AddInMemoryApiResources` Methode erwartet ein `IEnumerable<ApiResource>` Auflistung. Das folgende Codebeispiel zeigt die `GetApis` -Methode, die diese enthält in der eShopOnContainers verweisen Anwendung:

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

Diese Methode gibt an, dass IdentityServer der Aufträge und die Warenkorb-APIs geschützt werden sollten. Aus diesem Grund verwaltet IdentityServer Zugriff Token werden erforderlich sein, wenn Aufrufe dieser APIs. Weitere Informationen zu den `ApiResource` finden Sie unter [-API-Ressource](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) in der IdentityServer 4-Dokumentation.

#### <a name="configuring-identity-resources"></a>Konfigurieren von Identity-Ressourcen

Beim Konfigurieren der Identity-Ressourcen, die `AddInMemoryIdentityResources` Methode erwartet ein `IEnumerable<IdentityResource>` Auflistung. Ressourcen für IDENTITY handelt es sich um Daten, z. B. Benutzer-ID, Name oder e-Mail-Adresse. Jede Ressource Identität besitzt einen eindeutigen Namen, und beliebige Anspruchstypen können zugewiesen werden, klicken Sie dann in das Identitätstoken für den Benutzer aufzunehmen. Das folgende Codebeispiel zeigt die `GetResources` -Methode, die diese enthält in der eShopOnContainers verweisen Anwendung:

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

Die OpenID Connect-Spezifikation gibt einige [Standardidentität Ressourcen](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Die Mindestanforderung ist, dass die Unterstützung zum Ausgeben von eine eindeutige ID für Benutzer bereitgestellt wird. Dies wird erreicht, indem Sie verfügbar machen die `IdentityResources.OpenId` Identität Ressource.

> [!NOTE]
> Die `IdentityResources` -Klasse unterstützt alle Bereiche in der OpenID Connect-Spezifikation (Openid, e-Mail-Profil, Telefonnummer und Adresse) definiert.

IdentityServer unterstützt auch das Definieren von benutzerdefinierten Identität-Ressourcen. Weitere Informationen finden Sie unter [Definieren von benutzerdefinierten Identität Ressourcen](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) in der IdentityServer-Dokumentation. Weitere Informationen zu den `IdentityResource` finden Sie unter [Identität Ressource](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) in der IdentityServer 4-Dokumentation.

#### <a name="configuring-clients"></a>Konfigurieren von Clients

Clients sind Anwendungen, die Token von IdentityServer anfordern können. In der Regel müssen die folgenden Einstellungen für jeden Client mindestens definiert werden:

-   Eine eindeutige Client-ID
-   Die zulässigen Interaktionen mit dem token-Dienst (bekannt als die Grant-Typ).
-   Der Speicherort, in der Identitäts- und Zugriffs-Token zu gesendet werden (als umleitungs-URI bezeichnet).
-   Eine Liste der Ressourcen, die der Client Zugriff auf zulässig ist (bezeichnet als Bereiche).

Beim Konfigurieren von Clients, die `AddInMemoryClients` Methode erwartet ein `IEnumerable<Client>` Auflistung. Das folgende Codebeispiel zeigt die Konfiguration für die mobile eShopOnContainers-app in der `GetClients` -Methode, die diese enthält in der eShopOnContainers verweisen Anwendung:

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

Diese Konfiguration gibt Daten für die folgenden Eigenschaften:

-   `ClientId`: Eine eindeutige ID für den Client.
-   `ClientName`: Der Client-Anzeigenamen, die für die Protokollierung und die Zustimmung Bildschirm verwendet wird.
-   `AllowedGrantTypes`: Gibt an, wie ein Client mit IdentityServer interagieren möchte. Weitere Informationen finden Sie unter [Konfigurieren der Authentifizierung Flow](#configuring_the_authentication_flow).
-   `ClientSecrets`: Gibt an, die für den geheimen Clientanmeldeinformationen, die beim Anfordern von token-Endpunkt Token verwendet werden.
-   `RedirectUris`: Gibt die zulässige URIs die Token oder Autorisierungscodes zurückgegeben werden sollen.
-   `RequireConsent`: Gibt an, ob ein Bildschirm Zustimmung erforderlich ist.
-   `RequirePkce`: Gibt an, ob Clients mithilfe eines Autorisierungscodes Prüfschlüssel senden müssen.
-   `PostLogoutRedirectUris`: Gibt die zulässige URIs zum Umleiten an nach der Abmeldung an.
-   `AllowedCorsOrigins`: Gibt den Ursprung des Clients an, damit IdentityServer Cross-Origin-Aufrufe vom Ursprung ermöglichen kann.
-   `AllowedScopes`: Gibt die Ressourcen, denen auf der Client möglich ist. Standardmäßig hat ein Client keinen Zugriff auf Ressourcen.
-   `AllowOfflineAccess`: Gibt an, ob der Client Aktualisierungstoken anfordern kann.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Der Authentifizierungsablauf konfigurieren

Der Authentifizierungsablauf zwischen einem Client und IdentityServer können durch Angabe der Grant-Typen in konfiguriert werden die `Client.AllowedGrantTypes` Eigenschaft. Die OpenID Connect und OAuth 2.0-Spezifikationen definieren eine Reihe von missbrauchen, einschließlich:

-   Implizite. Diesem Datenstrom ist für die browserbasierte Anwendungen optimiert und sollte verwendet werden, für Benutzer nur Authentifizierung oder Authentifizierung und tokenanforderungen. Alle Token werden über den Browser übertragen und aus diesem Grund erweiterte Funktionen wie Aktualisierungstoken sind nicht zulässig.
-   Autorisierungscode. Diesen Datenfluss bietet die Möglichkeit, eine hinteren Kanals im Gegensatz zu den Front-Browser-Kanal-Token abzurufen, und auch die Clientauthentifizierung unterstützt.
-   Hybrid: Dieser Ablauf ist eine Kombination aus der impliziten und Autorisierungscode gewähren Typen. Das Identitätstoken wird über den Browser Kanal übertragen und enthält die Protokollantwort mit Vorzeichen zusammen mit anderen Artefakten z. B. den Autorisierungscode verwendet. Nach erfolgreicher Validierung der Antwort sollte hinteren Kanals zum Abrufen des Zugriffs und ein Aktualisierungstoken verwendet werden.

> [!TIP]
> Mithilfe des Ablaufs der Hybrid-Authentifizierung. Der Authentifizierungsablauf Hybrid führt zu eine Anzahl von Angriffen, die für den Kanal Browser gelten, und ist der empfohlene Ablauf für systemeigene Anwendungen, die zum Abrufen von Zugriffstoken (und möglicherweise Aktualisierungstoken) werden soll.

Weitere Informationen zu missbrauchen, finden Sie unter [Erteilungstypen](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) in der IdentityServer 4-Dokumentation.

### <a name="performing-authentication"></a>Ausführen der Authentifizierung

Für IdentityServer zum Ausstellen von Token im Auftrag eines Benutzers der Benutzer muss zum IdentityServer anmelden. Allerdings stellt keine IdentityServer eine Benutzeroberfläche oder die Datenbank für die Authentifizierung bereit. Aus diesem Grund wird in der Anwendung eShopOnContainers Verweis ASP.NET Core Identität für diesen Zweck verwendet.

Die mobile app eShopOnContainers authentifiziert mit IdentityServer mit der Hybrid-Authentifizierungsablauf, der in Abbildung 9 – 2 dargestellt ist.

![](authentication-and-authorization-images/sign-in.png "Allgemeine Übersicht über den Anmeldevorgang")

**Abbildung 9 – 2:** allgemeine Übersicht über den Anmeldevorgang

-In wird eine Anforderung zum `<base endpoint>:5105/connect/authorize`. Nach erfolgreicher Authentifizierung gibt IdentityServer eine Authentifizierungsantwort, enthält einen Autorisierungscode und ein Identitätstoken an. Der Autorisierungscode wird dann an gesendet `<base endpoint>:5105/connect/token`, die mit Access, Identitäts- und Aktualisierungstoken reagiert.

Die eShopOnContainers Verwaltungsrichtlinien für mobile Apps signiert-Out-of IdentityServer durch Senden einer Anforderung zum `<base endpoint>:5105/connect/endsession`, mit zusätzlichen Parametern. Nach der Abmeldung wird reagiert IdentityServer durch einen Post Logout umleitungs-URI zurück an die mobile Anwendung zu senden. Abbildung 9 – 3 zeigt diesen Vorgang.

![](authentication-and-authorization-images/sign-out.png "Allgemeine Übersicht über den Abmeldevorgang")

**Abbildung 9 – 3:** allgemeine Übersicht über den Abmeldevorgang

In der mobilen app eShopOnContainers erfolgt die Kommunikation mit IdentityServer durch die `IdentityService` Klasse implementiert die `IIdentityService` Schnittstelle. Diese Schnittstelle gibt an, dass die implementierende Klasse bereitstellen muss `CreateAuthorizationRequest`, `CreateLogoutRequest`, und `GetTokenAsync` Methoden.

#### <a name="signing-in"></a>Anmelden

Wenn der Benutzer tippt der **Anmeldung** auf die Schaltfläche der `LoginView`, die `SignInCommand` in der `LoginViewModel` Klasse ausgeführt wird, die wiederum führt die `SignInAsync` Methode. Im folgenden Codebeispiel wird diese Methode veranschaulicht:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Diese Methode ruft die `CreateAuthorizationRequest` Methode in der `IdentityService` -Klasse, die im folgenden Codebeispiel gezeigt wird:

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

Diese Methode erstellt den URI für den IdentityServer [autorisierungsendpunkt](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), mit den notwendigen Parametern aus. Der autorisierungsendpunkt befindet sich auf `/connect/authorize` über port 5105 des Basis-Endpunkts als eine benutzereinstellung verfügbar gemacht. Weitere Informationen zu den benutzereinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Die Angriffsfläche der mobilen Anwendung eShopOnContainers wird durch Implementieren der Proof-Schlüssel für Code Exchange (PKCE)-Erweiterung für OAuth reduziert. PKCE schützt den Autorisierungscode verwendet wird, wenn er abgefangen wird. Dies erfolgt durch den Client generiert einen geheimen Verifier, ein Hash der in der autorisierungsanforderung übergeben wird, und der präsentiert wird nicht gehashten Format vorliegen, wenn den Autorisierungscode einlösen. Weitere Informationen zu PKCE, finden Sie unter [Proof-Schlüssel für Code Exchange Public OAuth-Clients](https://tools.ietf.org/html/rfc7636) auf der Website der Internet Engineering Task Force.

Der zurückgegebene URI befindet sich in der `LoginUrl` Eigenschaft von der `LoginViewModel` Klasse. Wenn die `IsLogin` Eigenschaft `true`, die [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) in der `LoginView` wird angezeigt. Die `WebView` bindet die Daten seine [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) Eigenschaft, um die `LoginUrl` Eigenschaft der `LoginViewModel` -Klasse und stellt daher eine anmeldeanforderung an IdentityServer bei der `LoginUrl` Eigenschaft auf festgelegt ist Der IdentityServer-autorisierungsendpunkt. Wenn IdentityServer diese Anforderung empfängt und der Benutzer ist nicht authentifiziert, die `WebView` werden umgeleitet, auf die konfigurierten Anmeldeseite an, die in Abbildung 9 – 4 angezeigt wird.

![](authentication-and-authorization-images/login.png "Anmeldeseite angezeigt, die für die Webansicht")

**Abbildung 9 – 4:** Anmeldeseite angezeigt, die für die Webansicht

Nach Abschluss der Anmeldung die [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) an eine return-URI umgeleitet werden. Dies `WebView` Navigation führt dazu, dass die `NavigateAsync` Methode in der `LoginViewModel` -Klasse, die ausgeführt werden, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

Diese Methode analysiert die Authentifizierungsantwort, die in der return-URI enthalten ist, und vorausgesetzt, dass ein gültige Autorisierungscode vorhanden ist, sendet er eine Anfrage an den IdentityServer [tokenendpunkt](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), übergeben den Autorisierungscode der Für den geheimen Verifier PKCE und andere erforderliche Parameter. Die token-Endpunkt ist am `/connect/token` über port 5105 des Basis-Endpunkts als eine benutzereinstellung verfügbar gemacht. Weitere Informationen zu den benutzereinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **Tipp**: Überprüfen URIs zurück. Obwohl die mobilen Anwendung für eShopOnContainers return URI überprüft nicht, besteht die bewährte Methode zum Überprüfen, dass der Rückgabewert URI verweist, an einem bekannten Speicherort ein, um öffnen-Redirect-Angriffe zu verhindern.

Die token-Endpunkt einen gültigen Autorisierungs-Code und eine für den geheimen Verifier PKCE erhält, antwortet der Dienst mit einem Zugriffstoken, Identität, und Aktualisierungstoken. Das Zugriffstoken (die Zugriff auf API-Ressourcen kann) und das Identitätstoken werden dann als Anwendungseinstellungen gespeichert, und Seitennavigation erfolgt. Der Gesamteffekt in der mobilen Anwendung für eShopOnContainers Dies ist daher: vorausgesetzt, dass Benutzer mit IdentityServer erfolgreich authentifizieren können, sie navigiert wird, werden die `MainView` Seite, welche eine [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Zeigt, die die `CatalogView` als der ausgewählten Registerkarte.

Informationen zur Seitennavigation finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informationen zur Verwendung [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Navigation bewirkt, dass die Methode eine Ansicht Modell ausgeführt werden, finden Sie unter [aufrufen Navigation mithilfe von Verhaltensweisen](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Informationen zu Anwendungseinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Die eShopOnContainers kann auch eine simulierte anmelden, wenn die app konfiguriert ist, verwenden Sie simulierte Dienste in der `SettingsView`. In diesem Modus kommunizieren nicht die app mit IdentityServer, die den Benutzer, melden Sie sich mit Anmeldeinformationen zulässt.

#### <a name="signing-out"></a>Signieren von horizontaler

Wenn der Benutzer tippt der **ABMELDEN** Schaltfläche der `ProfileView`, die `LogoutCommand` in der `ProfileViewModel` Klasse ausgeführt wird, die wiederum führt die `LogoutAsync` Methode. Diese Methode führt die Seitennavigation, die `LoginView` Seite, und übergeben ein `LogoutParameter` Instanz festgelegt wurde `true` als Parameter. Weitere Informationen zum Übergeben von Parametern während der Seitennavigation finden Sie unter [übergeben von Parametern während der Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Wenn eine Sicht erstellt wird und navigiert, die `InitializeAsync` Methode von der Sicht zugeordneten Ansichtsmodell ausgeführt wird, die ausgeführt und dann die `Logout` Methode der `LoginViewModel` -Klasse, die im folgenden Codebeispiel gezeigt wird:

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

Diese Methode ruft die `CreateLogoutRequest` Methode in der `IdentityService` Klasse, und übergeben das Identitätstoken abgerufen aus Anwendungseinstellungen als Parameter. Weitere Informationen zu Anwendungseinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Das folgende Codebeispiel zeigt die `CreateLogoutRequest` Methode:

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

Diese Methode erstellt den URI des IdentityServer [Beenden der Sitzung Endpunkt](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), mit den notwendigen Parametern aus. Der Endpunkt der End-Sitzung befindet sich auf `/connect/endsession` über port 5105 des Basis-Endpunkts als eine benutzereinstellung verfügbar gemacht. Weitere Informationen zu den benutzereinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Der zurückgegebene URI befindet sich in der `LoginUrl` Eigenschaft von der `LoginViewModel` Klasse. Während der `IsLogin` Eigenschaft ist `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) in der `LoginView` sichtbar ist. Die `WebView` bindet die Daten seine [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) Eigenschaft, um die `LoginUrl` Eigenschaft der `LoginViewModel` -Klasse und stellt daher einen abmeldeanforderung an IdentityServer bei der `LoginUrl` Eigenschaft auf festgelegt ist IdentityServer des End-Sitzung Endpunkt. Wenn IdentityServer diese Anforderung empfängt, vorausgesetzt, dass der Benutzer angemeldet ist, kommt es abmelden. Authentifizierung wird mit einem Cookie, das von der cookieauthentifizierungsmiddleware von ASP.NET Core verwaltet nachverfolgt. Aus diesem Grund IdentityServer Abmelden entfernt das Authentifizierungscookie und sendet eine Post-Abmelde-Umleitung URI zurück an den Client.

In der mobilen Anwendung die [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) werden an den Post Logout umleitungs-URI umgeleitet. Dies `WebView` Navigation führt dazu, dass die `NavigateAsync` Methode in der `LoginViewModel` -Klasse, die ausgeführt werden, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

Diese Methode löscht das Identitätstoken und dem Zugriffstoken aus Anwendungseinstellungen, und legt sie fest der `IsLogin` Eigenschaft, um `false`, wodurch die [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) auf die `LoginView` Seite nicht sichtbar . Schließlich die `LoginUrl` Eigenschaftensatz, der URI des IdentityServer des [autorisierungsendpunkt](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), mit den notwendigen Parametern als Vorbereitung für das nächste Mal die Benutzer initiiert einer Anmeldung an.

Informationen zur Seitennavigation finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informationen zur Verwendung [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Navigation bewirkt, dass die Methode eine Ansicht Modell ausgeführt werden, finden Sie unter [aufrufen Navigation mithilfe von Verhaltensweisen](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Informationen zu Anwendungseinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Die eShopOnContainers kann auch ein Mock Abmelden bei die app mit simulierten Dienste in der SettingsView konfiguriert ist. In diesem Modus wird die app nicht mit IdentityServer kommunizieren und stattdessen löscht keine gespeicherten Token Anwendungseinstellungen.

<a name="authorization" />

## <a name="authorization"></a>Autorisierung

Nach der Authentifizierung, ASP.NET Core Web-APIs häufig beim Autorisieren des Zugriffs benötigen, kann die ein Dienst APIs stellen einige authentifizierten Benutzern zur Verfügung, aber nicht alle.

Einschränken des Zugriffs auf eine Route Core ASP.NET-MVC kann erreicht werden, indem ein Authorize-Attribut auf einen Controller angewendet oder Aktion, die Zugriff auf den Controller beschränkt oder Aktion aus, um authentifizierte Benutzer, wie im folgenden Codebeispiel gezeigt:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Wenn ein nicht autorisierter Benutzer versucht, den Zugriff auf einen Controller oder Aktion, die mit der `Authorize` -Attribut, das MVC-Framework gibt einen 401 (nicht autorisiert) HTTP-Statuscode zurück.

> [!NOTE]
> Parameter können angegeben werden, auf die `Authorize` Attribut, um eine API auf bestimmte Benutzer beschränken. Weitere Informationen finden Sie unter [Autorisierung](/aspnet/core/security/authorization/introduction/).

IdentityServer kann in den autorisierungsworkflow integriert werden, damit das Zugriffstoken steuern die Autorisierung bereitgestellt. Dieser Ansatz ist in Abbildung 9 – 5 angezeigt.

![](authentication-and-authorization-images/authorization.png "Autorisierung Zugriffstoken")

**Abbildung 9 – 5:** Autorisierung Zugriffstoken

Die mobilen Anwendung für eShopOnContainers kommuniziert mit der Identität Microservice und fordert ein Zugriffstoken im Rahmen des Authentifizierungsprozesses. Das Zugriffstoken wird dann an die APIs, die von der Sortierung und Warenkorb Microservices verfügbar gemacht, als Teil der zugriffsanforderungen weitergeleitet. Zugriffstoken enthalten Informationen über den Client und der Benutzer an. -APIs verwenden, die Informationen dann beim Autorisieren des Zugriffs auf ihre Daten. Informationen zum Konfigurieren von IdentityServer zum Schützen von APIs finden Sie unter [-API-Ressourcen konfigurieren](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Konfigurieren von IdentityServer für die Autorisierung

Um Autorisierung mit IdentityServer auszuführen, muss die Webanwendung HTTP-Anforderungspipeline die Autorisierung Middleware hinzugefügt werden. Die Middleware wird hinzugefügt, der `ConfigureAuth` Methode in der Webanwendung `Startup` -Klasse, die aufgerufen wird, aus der `Configure` -Methode, und wird im folgenden Codebeispiel aus der eShopOnContainers Referenz-Anwendung veranschaulicht:

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

Diese Methode wird sichergestellt, dass die API nur mit einer gültigen Zugriffstoken zugegriffen werden kann. Die Middleware überprüft das eingehende Token, um sicherzustellen, dass es von einem vertrauenswürdigen Aussteller gesendet wird, und überprüft, ob das Token gültig ist, mit der API verwendet werden soll, die es empfängt. Aus diesem Grund wird an den Sortier- oder Warenkorb Controller Durchsuchen einer 401 (nicht autorisiert) HTTP-Statuscode zurückgegeben, gibt an, dass ein Zugriffstoken erforderlich ist.

> [!NOTE]
> Des IdentityServer Autorisierung Middleware muss die Webanwendung HTTP-Anforderung Pipeline hinzugefügt werden, vor dem Hinzufügen von MVC mit `app.UseMvc()` oder `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>Zugriffsanforderungen APIs vorgenommen.

Beim Anforderungen an die Sortier- und Warenkorb Microservices, den Zugriff token abgerufenes IdentityServer während des Authentifizierungsprozesses muss in der Anforderung enthalten sein, wie im folgenden Codebeispiel gezeigt:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Das Zugriffstoken wird als eine anwendungseinstellung gespeichert und aus dem plattformspezifischen Speicher abgerufen und enthalten, die im Aufruf der `GetOrderAsync` Methode in der `OrderService` Klasse.

Auf ähnliche Weise muss das Zugriffstoken enthalten sein, wenn-API Senden von Daten an eine IdentityServer geschützt werden wie im folgenden Codebeispiel gezeigt:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Das Zugriffstoken ist aus plattformspezifischen Speicher abgerufen und in den Aufruf von der `UpdateBasketAsync` Methode in der `BasketService` Klasse.

Die `RequestProvider` -Klasse, in der mobilen app eShopOnContainers verwendet die `HttpClient` Klasse, um die Anforderungen an die Rest-APIs verfügbar gemacht werden, von der eShopOnContainers Verweis Anwendung auszuführen. Wenn machen, auf die Reihenfolge und die Warenkorb-APIs, die Autorisierung erfordern anfordert, muss ein gültiger Zugriffstoken mit der Anforderung enthalten sein. Dies wird erreicht, indem Sie das Zugriffstoken an die Header der der `HttpClient` Instanz, wie im folgenden Codebeispiel gezeigt:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

Die `DefaultRequestHeaders` Eigenschaft von der `HttpClient` Klasse macht die Header, die mit jeder Anforderung gesendet werden, und das Zugriffstoken hinzugefügt wird die `Authorization` Header, die die Zeichenfolge mit dem Präfix `Bearer`. Wenn die Anforderung gesendet wird, um eine RESTful-API, die den Wert des der `Authorization` Header extrahiert und überprüft, um sicherzustellen, dass sie von einem vertrauenswürdigen Aussteller gesendet wurde, und verwendet, um zu bestimmen, ob der Benutzer über die Berechtigung zum Aufrufen der API, die es empfängt.

Weitere Informationen dazu, wie die mobilen Anwendung für eShopOnContainers webanforderungen vereinfacht, finden Sie unter [Zugriff auf Remotedaten](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Zusammenfassung

Es gibt viele Ansätze zum Integrieren von Authentifizierung und Autorisierung in einer Xamarin.Forms-app, die mit einer ASP.NET MVC-Web-Anwendung kommuniziert. Die eShopOnContainers mobile-Anwendung führt die Authentifizierung und Autorisierung mit einem Microservice Datenvolumes Identität, die IdentityServer 4 verwendet. IdentityServer ist ein open Source-OpenID Connect und OAuth 2.0-Framework für ASP.NET Core, die mit ASP.NET Core Identität zum Ausführen von trägertokenauthentifizierung integriert wird.

Die mobile Anwendung fordert von Sicherheitstoken aus IdentityServer, zum Authentifizieren eines Benutzers oder für den Zugriff auf eine Ressource an. Beim Zugriff auf eine Ressource, muss ein Zugriffstoken in der Anforderung an APIs enthalten sein, die eine Autorisierung erfordern. IdentityServer der Middleware überprüft eingehende Zugriffstoken, um sicherzustellen, dass sie von einem vertrauenswürdigen Aussteller gesendet werden, und mit der API verwendet werden soll, die sie empfängt gültig sind.


## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
