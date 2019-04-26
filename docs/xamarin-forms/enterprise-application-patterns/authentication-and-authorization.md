---
title: Authentifizierung und Autorisierung
description: In diesem Kapitel wird erläutert, wie die "eshoponcontainers" mobile app für Authentifizierung und Autorisierung für die Microservices in Containern ausführt.
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 9db9902dfbf602ba21b353f3a17920dc37b03ee5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382313"
---
# <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Authentifizierung ist der Prozess der ID-Anmeldeinformationen, z. B. Name und das Kennwort eines Benutzers abrufen und diese Anmeldeinformationen über eine Authentifizierungsstelle zu validieren. Wenn die Anmeldeinformationen gültig sind, gilt die Entität, die die Anmeldeinformationen übermittelt eine authentifizierte Identität. Nachdem eine Identität authentifiziert wurde, bestimmt ein Autorisierungsprozesses, ob dieser Identität Zugriff auf eine bestimmte Ressource verfügt.

Es gibt viele Ansätze zum Integrieren von Authentifizierung und Autorisierung in einer Xamarin.Forms-app, die mit einer ASP.NET MVC-Webanwendung, einschließlich der Verwendung von ASP.NET Core Identity kann z. B. Microsoft, Google externe Authentifizierungsanbieter kommuniziert, Facebook oder Twitter und Authentifizierung-Middleware. Die eShopOnContainers-mobile-app führt die Authentifizierung und Autorisierung mit einem Container Identitäts-Microservice, der IdentityServer 4 verwendet. Die mobile app wird die inhaltsortsanfrage Sicherheitstoken Identity Server, für die Authentifizierung eines Benutzers oder für den Zugriff auf eine Ressource. Für IdentityServer zum Ausstellen von Token im Auftrag eines Benutzers der Benutzer muss zum Identity Server anmelden. Jedoch bereitstellen keine Identity Server eine Benutzeroberfläche oder die Datenbank für die Authentifizierung. Aus diesem Grund wird in der referenzanwendung "eshoponcontainers" ASP.NET Core-Identität für diesen Zweck verwendet.

## <a name="authentication"></a>Authentifizierung

Authentifizierung ist erforderlich, wenn eine Anwendung die Identität des aktuellen Benutzers kennen muss. ASP.NET Core primären Mechanismus zur Identifizierung von Benutzern ist das ASP.NET Core Identity-Mitgliedschaftssystem, dem Benutzerinformationen in einem vom Entwickler konfigurierten Datenspeicher gespeichert. In der Regel werden dieser Datenspeicher einem EntityFramework-Datenspeicher, aber benutzerdefinierte Speicher oder Pakete von Drittanbietern verwendet werden können, um Identitätsinformationen in Azure Storage, Azure Cosmos DB oder anderen Speicherorten zu speichern.

Für authentifizierungsszenarios, verwenden Sie einen Datenspeicher für lokale Benutzer und die Informationen zur Identität zwischen Anforderungen mithilfe von Cookies (was typisch für ASP.NET MVC-Webanwendungen ist) beibehalten, ist ASP.NET Core Identity eine angemessene Lösung. Allerdings sind Cookies nicht immer eine natürliche Möglichkeit persistent gespeichert und Übertragen von Daten. Beispielsweise müssen eine ASP.NET Core-Webanwendung, die RESTful-Endpunkte verfügbar macht, die über eine mobile app zugegriffen werden in der Regel Bearer-token-Authentifizierung, verwenden, da Cookies in diesem Szenario verwendet werden können. Allerdings können Bearer-Tokens problemlos werden abgerufen und in der Authorization-Header der webanforderungen über die mobile app enthalten.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Ausstellen von Bearer-Token, die Verwendung von Identity Server 4

[Identity Server 4](https://github.com/IdentityServer/IdentityServer4) ist ein open-Source-Framework für OpenID Connect und OAuth 2.0 für ASP.NET Core, die für viele Authentifizierung und Autorisierung-Szenarien einschließlich Ausstellen von Sicherheitstoken für lokale ASP.NET Core Identity-Benutzer verwendet werden kann.

> [!NOTE]
> OpenID Connect und OAuth 2.0 sind sehr ähnlich, aber unterschiedliche Aufgaben verantwortlich.

OpenID Connect ist eine Authentifizierung-Schicht über das OAuth 2.0-Protokoll. OAuth 2 ist ein Protokoll, Anwendungen ermöglicht das Anfordern von Zugriffstoken von einem Security token Service, und verwenden sie für die Kommunikation mit APIs. Diese Delegierung reduziert die Komplexität in Client-Anwendungen und APIs, da es sich bei Authentifizierung und Autorisierung zentral verwaltet werden können.

Die Kombination von OpenID Connect und OAuth 2.0 kombinieren, die zwei grundlegende Sicherheitsaspekte der Authentifizierung und API-Zugriff und IdentityServer 4 ist eine Implementierung dieser Protokolle.

In Anwendungen, die direkte Client-zu-Microservice-Kommunikation, z. B. der referenzanwendung "eshoponcontainers" verwendet ein dedizierter authentifizierungsmicroservice als ein Sicherheitstokendienst (STS) dienen zum Authentifizieren von Benutzern, wie in Abbildung 9 – 1. Weitere Informationen über die direkte Client-zu-Microservice-Kommunikation finden Sie unter [Kommunikation zwischen Client und Microservices](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Authentifizierung durch eine dedizierte Authentifizierung-microservice")

**Abbildung 9 – 1:** Authentifizierung durch eine dedizierte Authentifizierung-microservice

Die eShopOnContainers-mobile-app kommuniziert mit dem Identitäts-Microservice, der 4 von Identity Server führen die Authentifizierung und Zugriffssteuerung für APIs verwendet. Aus diesem Grund wird die inhaltsortsanfrage der mobilen app Token Identity Server, für die Authentifizierung eines Benutzers oder für den Zugriff auf eine Ressource:

-   Authentifizieren von Benutzern bei Identity Server wird erreicht, indem Sie die mobile app Anfordern einer *Identität* -Token, das das Ergebnis eines Prozesses für die Authentifizierung darstellt. Mindestanforderungen enthält es einen Bezeichner für den Benutzer, und Informationen, wie und wann der Benutzer authentifiziert. Sie können auch zusätzliche Identitätsdaten enthalten.
-   Zugreifen auf eine Ressource mit Identity Server wird erreicht, indem Sie die mobile app Anfordern einer *Zugriff* -Token, das Zugriff auf eine API-Ressource ermöglicht. -Clients anfordern von Zugriffstoken und an die API weitergeleitet werden. Zugangs-Token enthalten Informationen zu den Client und der Benutzer (falls vorhanden). APIs verwenden diese Informationen dann zum Autorisieren des Zugriffs auf ihre Daten.

> [!NOTE]
> Ein Client muss bei Identity Server registriert werden, bevor es Token anfordern kann.

### <a name="adding-identityserver-to-a-web-application"></a>Hinzufügen von Identity Server eine Webanwendung

In der Reihenfolge für eine Webanwendung für ASP.NET Core Identity Server 4 verwenden müssen sie der Webanwendung Visual Studio-Projektmappe hinzugefügt werden. Weitere Informationen finden Sie unter [Übersicht](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) in Identity Server-Dokumentation.

Sobald Identity Server in der Webanwendung von Visual Studio-Projektmappe enthalten ist, muss sie der Webanwendung HTTP-Anforderungsverarbeitungspipeline, hinzugefügt werden, damit sie Anforderungen an OpenID Connect und OAuth 2.0-Endpunkte dienen kann. Dies wird erreicht, der `Configure` -Methode in der Webanwendung `Startup` Klasse, wie im folgenden Codebeispiel gezeigt:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Reihenfolge von Bedeutung ist in der Webanwendung HTTP-Anforderungsverarbeitungspipeline. Identity Server muss daher an die Pipeline aus, bevor Sie das Framework für UI hinzugefügt werden, die den Anmeldebildschirm implementiert.

### <a name="configuring-identityserver"></a>Konfigurieren von Identity Server

Identity Server muss konfiguriert werden, der `ConfigureServices` -Methode in der Webanwendung `Startup` Klasse durch Aufrufen der `services.AddIdentityServer` -Methode, wie im folgenden Codebeispiel wird aus der referenzanwendung "eshoponcontainers" veranschaulicht:

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

Nach dem Aufruf der `services.AddIdentityServer` Methode zusätzliche fluent-APIs werden aufgerufen, um Folgendes zu konfigurieren:

-   Anmeldeinformationen, die zum Signieren verwendet werden.
-   API und die Identity-Ressourcen, die Benutzer anfordern können den Zugriff auf.
-   Clients, die zum Anfordern von Token eine Verbindung hergestellt werden soll.
-   ASP.NET Core-Identität.

>💡 **Tipp**: Die Identity Server 4-Konfiguration dynamisch zu laden. Identity Server 4 APIs ermöglichen die Konfiguration von Identity Server aus einer Liste in-Memory-Objekte. In der referenzanwendung "eshoponcontainers" sind diese Auflistungen im Arbeitsspeicher an, in der Anwendung hartcodiert. Allerdings können in der Produktion sie geladen dynamisch aus einer Konfigurationsdatei oder aus einer Datenbank werden.

Weitere Informationen zum Konfigurieren von Identity Server, um ASP.NET Core Identity zu verwenden, finden Sie unter [mithilfe von ASP.NET Core Identity](https://identityserver4.readthedocs.io/en/latest/quickstarts/8_aspnet_identity.html) in Identity Server-Dokumentation.

#### <a name="configuring-api-resources"></a>Konfigurieren von API-Ressourcen

Beim Konfigurieren der API-Ressourcen, die `AddInMemoryApiResources` Methode erwartet, dass ein `IEnumerable<ApiResource>` Auflistung. Das folgende Codebeispiel zeigt die `GetApis` -Methode, die diese enthält in der eShopOnContainers-referenzanwendung:

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

Diese Methode gibt an, dass es sich bei Identity Server die Bestellungen und Warenkorb APIs geschützt werden sollte. Aus diesem Grund verwaltet Identity Server Access Token werden erforderlich sein, wenn Sie Aufrufe dieser APIs vornehmen. Weitere Informationen zu den `ApiResource` finden Sie unter [-API-Ressource](https://identityserver4.readthedocs.io/en/latest/reference/api_resource.html) in der Dokumentation IdentityServer 4.

#### <a name="configuring-identity-resources"></a>Konfigurieren von Identitätsressourcen

Beim Konfigurieren der Identity-Ressourcen, die `AddInMemoryIdentityResources` Methode erwartet, dass ein `IEnumerable<IdentityResource>` Auflistung. Identity-Ressourcen handelt es sich um Daten wie z. B. Benutzer-ID, Name oder e-Mail-Adresse. Jede Identität-Ressource hat einen eindeutigen Namen ein, und beliebige Anspruchstypen können zugewiesen werden, klicken Sie dann in das Identitätstoken für den Benutzer aufzunehmen. Das folgende Codebeispiel zeigt die `GetResources` -Methode, die diese enthält in der eShopOnContainers-referenzanwendung:

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

Der OpenID Connect-Spezifikation gibt an, einige [standard identitätsressourcen](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Die Mindestanforderung ist, dass die Unterstützung zum Ausgeben von eine eindeutige ID für Benutzer verfügbar ist. Dies erfolgt durch Verfügbarmachen der `IdentityResources.OpenId` Identity-Ressource.

> [!NOTE]
> Die `IdentityResources` Klasse unterstützt alle Bereiche in der OpenID Connect-Spezifikation (Openid, e-Mail-Adresse, Profil, Telefonnummer und Adresse) definiert.

Identity Server unterstützt auch das Definieren von benutzerdefinierten Identität-Ressourcen. Weitere Informationen finden Sie unter [Definieren von benutzerdefinierten identitätsressourcen](http://docs.identityserver.io/en/latest/topics/resources.html#defining-custom-identity-resources) in Identity Server-Dokumentation. Weitere Informationen zu den `IdentityResource` finden Sie unter [Identität Ressource](https://identityserver4.readthedocs.io/en/latest/reference/identity_resource.html) in der Dokumentation IdentityServer 4.

#### <a name="configuring-clients"></a>Konfigurieren von Clients

Clients sind Anwendungen, die von Identity Server Token anfordern können. In der Regel müssen die folgenden Einstellungen für jeden Client mindestens definiert werden:

-   Eine eindeutige Client-ID
-   Die zulässigen Interaktionen mit dem Tokendienst (bekannt als der gewährungstyp).
-   Der Speicherort, in Identitäts- und Zugriffstoken gesendet werden (als umleitungs-URI bezeichnet).
-   Eine Liste der Ressourcen, die der Client Zugriff auf zulässig ist (bezeichnet als Bereiche).

Konfiguration von Clients, die `AddInMemoryClients` Methode erwartet, dass ein `IEnumerable<Client>` Auflistung. Das folgende Codebeispiel zeigt die Konfiguration für die eShopOnContainers-mobile-app in der `GetClients` -Methode, die diese enthält in der eShopOnContainers-referenzanwendung:

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

Diese Konfiguration gibt Daten für die folgenden Eigenschaften an:

-   `ClientId`: Eine eindeutige ID für den Client.
-   `ClientName`: Der Client-Anzeigename, der für die Protokollierung und der zustimmungsbildschirm verwendet wird.
-   `AllowedGrantTypes`: Gibt an, wie ein Client mit Identity Server interagieren möchte. Weitere Informationen finden Sie unter [Konfigurieren der Authentifizierungsfluss](#configuring_the_authentication_flow).
-   `ClientSecrets`: Gibt an, den geheimen Client-Anmeldeinformationen, die beim Anfordern von aus den token-Endpunkt Token verwendet werden.
-   `RedirectUris`: Gibt die zulässigen URI, der den Token oder Autorisierungscodes zurück an.
-   `RequireConsent`: Gibt an, ob ein genehmigungsbildschirm erforderlich ist.
-   `RequirePkce`: Gibt an, ob Clients, die mit einem Autorisierungscode einen Prüfschlüssel senden müssen.
-   `PostLogoutRedirectUris`: Gibt den zulässigen URI, nach der Abmeldung umgeleitet.
-   `AllowedCorsOrigins`: Gibt den Ursprung des Clients an, sodass IdentityServer ursprungsübergreifende Aufrufe vom Ursprung zu ermöglichen kann.
-   `AllowedScopes`: Gibt die Ressourcen, auf denen der Client zugreifen kann. Standardmäßig verfügt ein Client keinen Zugriff auf alle Ressourcen.
-   `AllowOfflineAccess`: Gibt an, ob der Client Aktualisierungstoken anfordern kann.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Konfigurieren den Authentifizierungsablauf

Der Ablauf der Authentifizierung zwischen einem Client und Identity Server können konfiguriert werden, indem die Grant-Typen in angeben der `Client.AllowedGrantTypes` Eigenschaft. Die OpenID Connect und OAuth 2.0-Spezifikationen definieren Sie eine Anzahl von authentifizierungsabläufe wie z.B.:

-   Implizite. Dieser Flow ist für die browserbasierte Anwendungen optimiert und sollte verwendet werden, entweder für Benutzer nur Authentifizierung oder tokenanforderungen für Authentifizierung und Zugriff. Alle Token werden über den Browser übertragen und aus diesem Grund erweiterte Funktionen wie die Aktualisierungstoken nicht zulässig sind.
-   Autorisierungscode. Dieser Flow ermöglicht das Abrufen von Token in einem Kanal zurück, im Gegensatz zu den Front-Browser-Kanal, während die Clientauthentifizierung unterstützt.
-   Hybrid: Dieser Flow ist eine Kombination aus den impliziten und Autorisierungscode-Berechtigungstypen. Das Identitätstoken wird über den Browser-Kanal übertragen, und es enthält die Protokollantwort mit Vorzeichen zusammen mit anderen Artefakten wie z. B. den Autorisierungscode. Nach erfolgreicher Validierung der Antwort sollte den Rückkanal rufen den Zugriff und Aktualisierungstoken verwendet werden.

> [!TIP]
> Verwenden Sie den Hybrid-authentifizierungsfluss. Der Hybrid-Authentifizierungsablauf vermindert eine Anzahl von Angriffen, die für den Kanal Browser gelten und ist der empfohlene Ablauf für native Anwendungen, die zum Abrufen von Zugriffstoken (und möglicherweise Aktualisierungstoken) werden sollen.

Weitere Informationen zu-authentifizierungsabläufen finden Sie unter [Grant Types](https://identityserver4.readthedocs.io/en/latest/topics/grant_types.html) in der Dokumentation IdentityServer 4.

### <a name="performing-authentication"></a>Ausführen der Authentifizierung

Für IdentityServer zum Ausstellen von Token im Auftrag eines Benutzers der Benutzer muss zum Identity Server anmelden. Jedoch bereitstellen keine Identity Server eine Benutzeroberfläche oder die Datenbank für die Authentifizierung. Aus diesem Grund wird in der referenzanwendung "eshoponcontainers" ASP.NET Core-Identität für diesen Zweck verwendet.

Die eShopOnContainers-mobile-app authentifiziert mit Identity Server mit der Hybrid-Authentifizierung, die in Abbildung 9-2 dargestellt wird.

![](authentication-and-authorization-images/sign-in.png "Allgemeine Übersicht über den Anmeldevorgang")

**Abbildung 9 – 2:** Allgemeine Übersicht über den Anmeldevorgang

Eine anmeldeanforderung wird versucht, `<base endpoint>:5105/connect/authorize`. Nach erfolgreicher Authentifizierung gibt Identity Server eine Authentifizierungsantwort, enthält einen Autorisierungscode und ein Identitätstoken an. Der Autorisierungscode wird dann gesendet, um `<base endpoint>:5105/connect/token`, der antwortet mit dem Zugriff, Identity und Aktualisierungstoken.

Die "eshoponcontainers" mobile app meldet-Out-of Identity Server durch Senden einer Anforderung zum `<base endpoint>:5105/connect/endsession`, mit zusätzlichen Parametern. Nach der Abmeldung auftritt, sendet daraufhin Identity Server einen Beitrag Logout umleitungs-URI zurück an die mobile app. Dieser Prozess wird in Abbildung 9 – 3 dargestellt.

![](authentication-and-authorization-images/sign-out.png "Allgemeine Übersicht über den Abmeldevorgang ab")

**Abbildung 9 – 3:** Allgemeine Übersicht über den Abmeldevorgang ab

In der mobilen Anwendung "eshoponcontainers", erfolgt die Kommunikation mit Identity Server durch die `IdentityService` Klasse implementiert die `IIdentityService` Schnittstelle. Diese Schnittstelle gibt an, dass die implementierende Klasse bereitstellen muss `CreateAuthorizationRequest`, `CreateLogoutRequest`, und `GetTokenAsync` Methoden.

#### <a name="signing-in"></a>Anmeldung

Wenn der Benutzer tippt auf die **Anmeldung** auf auf die Schaltfläche der `LoginView`, wird die `SignInCommand` in der `LoginViewModel` Klasse ausgeführt wird, das wiederum führt die `SignInAsync` Methode. Im folgenden Codebeispiel wird diese Methode veranschaulicht:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Diese Methode ruft die `CreateAuthorizationRequest` -Methode in der die `IdentityService` -Klasse, die im folgenden Codebeispiel gezeigt wird:

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

Diese Methode erstellt den URI für die Identity Server [autorisierungsendpunkt](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html), mit den notwendigen Parametern aus. Der autorisierungsendpunkt wird am `/connect/authorize` 5105 des Basis-Endpunkts verfügbar gemacht werden, wie eine benutzereinstellung für die Ports. Weitere Informationen zu den benutzereinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Die Angriffsfläche der mobilen Anwendung "eshoponcontainers" wird durch die Implementierung der Proof-Schlüssel für Code Exchange (PKCE)-Erweiterung für OAuth reduziert. PKCE schützt den Autorisierungscode verwendet werden, wenn er abgefangen wird. Dies erfolgt durch den Client generiert einen geheimen Prüfung der ein Hash der in der autorisierungsanforderung übergeben wird, und der präsentiert wird nicht gehashten Format vorliegen, wenn den Autorisierungscode einlösen. Weitere Informationen zu PKCE, finden Sie unter [Proof Key für Code Exchange von öffentlichen Clients OAuth](https://tools.ietf.org/html/rfc7636) auf der Website der Internet Engineering Task Force.

Der zurückgegebene URI befindet sich in der `LoginUrl` Eigenschaft der `LoginViewModel` Klasse. Wenn die `IsLogin` Eigenschaft `true`, die [ `WebView` ](xref:Xamarin.Forms.WebView) in der `LoginView` sichtbar wird. Die `WebView` Datenbindung der [ `Source` ](xref:Xamarin.Forms.WebView.Source) Eigenschaft, um die `LoginUrl` Eigenschaft der `LoginViewModel` Klasse, und sendet Sie also eine anmeldeanforderung an Identity Server bei der `LoginUrl` -Eigenschaftensatz auf Identity Server autorisierungsendpunkt. Bei Identity Server diese Anforderung empfängt und der Benutzer ist nicht authentifiziert, die `WebView` gelangen auf die konfigurierten Anmeldeseite, die in Abbildung 9-4 dargestellt wird.

![](authentication-and-authorization-images/login.png "Anmeldeseite, die von der Webansicht angezeigt")

**Abbildung 9 – 4:** Anmeldeseite, die von der Webansicht angezeigt

Nach der Anmeldung abgeschlossen ist, die [ `WebView` ](xref:Xamarin.Forms.WebView) gelangen zu einem URI zurück. Dies `WebView` Navigation führt dazu, dass die `NavigateAsync` -Methode in der die `LoginViewModel` -Klasse, die ausgeführt werden, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

Diese Methode analysiert die Authentifizierungsantwort, die in der return-URI enthalten ist, und vorausgesetzt, dass Sie ein gültigen Autorisierungs-Code vorhanden ist, sie sendet eine Anfrage an Identity Server [-token-Endpunkt](https://identityserver4.readthedocs.io/en/latest/endpoints/token.html), übergeben den Autorisierungscode, den Geheime Verifier PKCE und andere erforderliche Parameter. Die token-Endpunkt ist `/connect/token` 5105 des Basis-Endpunkts verfügbar gemacht werden, wie eine benutzereinstellung für die Ports. Weitere Informationen zu den benutzereinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **Tipp**: Überprüfen Sie die return-URIs. Die eShopOnContainers-mobile-app den zurück-URI nicht zu überprüfen, zwar die bewährte Methode zum Überprüfen, dass der URI zurück an einem bekannten Speicherort, um Open-Redirect-Angriffe zu verhindern bezieht.

Der token-Endpunkt eine gültige Autorisierungscode und die geheimen Verifier PKCE empfängt, antwortet er mit einem Zugriffstoken, Identität, und Aktualisierungstoken. Das Zugriffstoken (die Zugriff auf API-Ressourcen ermöglicht) und eine-Identitätstoken werden dann als Anwendungseinstellungen gespeichert, und die Seitennavigation erfolgt. Stärker wirkt sich in der mobilen app für "eshoponcontainers" Dies ist daher: vorausgesetzt, dass der Benutzer erfolgreich mit Identity Server authentifizieren können, sie navigiert wird, werden die `MainView` Seite ist eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Zeigt, die die `CatalogView` als der ausgewählten Registerkarte.

Weitere Informationen zu navigieren, finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informationen zur Verwendung [ `WebView` ](xref:Xamarin.Forms.WebView) Navigation bewirkt, dass eine View Model-Methode ausgeführt werden, finden Sie unter [aufrufen Navigation mithilfe von Verhaltensweisen](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Informationen zu Anwendungseinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Bestellung von eShopOnContainers wird auch ein mock-Anmeldung ermöglicht, wenn die app konfiguriert ist, mit pseudodiensten in die `SettingsView`. In diesem Modus kommunizieren nicht die app mit Identity Server, die Benutzer melden Sie sich mit Anmeldeinformationen zulässt.

#### <a name="signing-out"></a>Signatur-out

Wenn der Benutzer tippt der **ABMELDEN** Schaltfläche der `ProfileView`, wird die `LogoutCommand` in die `ProfileViewModel` Klasse ausgeführt wird, das wiederum führt die `LogoutAsync` Methode. Diese Methode führt die Seitennavigation auf der `LoginView` Seite, und übergeben einen `LogoutParameter` Instanz festgelegt wurde `true` als Parameter. Weitere Informationen zum Übergeben von Parametern während der Seitennavigation finden Sie unter [übergeben von Parametern während der Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Wenn eine Sicht erstellt wird und navigiert, die `InitializeAsync` -Methode der zugeordneten Ansichtsmodell der Ansicht ausgeführt wird, der ausgeführt wird, klicken Sie dann die `Logout` Methode der `LoginViewModel` -Klasse, die im folgenden Codebeispiel gezeigt wird:

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

Diese Methode ruft die `CreateLogoutRequest` -Methode in der die `IdentityService` Klasse, und übergeben das Identitätstoken abgerufen aus den Anwendungseinstellungen als Parameter. Weitere Informationen zu Anwendungseinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Die `CreateLogoutRequest`-Methode wird in folgendem Codebeispiel veranschaulicht:

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

Diese Methode erstellt den URI des IdentityServer [Beenden der Sitzung Endpunkt](https://identityserver4.readthedocs.io/en/latest/endpoints/endsession.html#refendsession), mit den erforderlichen Parametern. Die Sitzung End-Endpunkt ist `/connect/endsession` 5105 des Basis-Endpunkts verfügbar gemacht werden, wie eine benutzereinstellung für die Ports. Weitere Informationen zu den benutzereinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Der zurückgegebene URI befindet sich in der `LoginUrl` Eigenschaft der `LoginViewModel` Klasse. Während der `IsLogin` Eigenschaft `true`, [ `WebView` ](xref:Xamarin.Forms.WebView) in die `LoginView` sichtbar ist. Die `WebView` Datenbindung der [ `Source` ](xref:Xamarin.Forms.WebView.Source) Eigenschaft, um die `LoginUrl` Eigenschaft der `LoginViewModel` Klasse, und sendet Sie also eine Anforderung zur Abmelde an Identity Server bei der `LoginUrl` -Eigenschaftensatz auf Identity Server End-Sitzung-Endpunkt. Bei Identity Server diese Anforderung empfängt, vorausgesetzt, dass der Benutzer angemeldet ist, tritt ein, Abmelden. Authentifizierung wird mit einem Cookie verwaltet werden, indem die cookieauthentifizierungs-Middleware von ASP.NET Core nachverfolgt. Aus diesem Grund wird das Abmelden von Identity Server entfernt das Authentifizierungscookie und sendet eine Post Logout Redirect-URI zurück an den Client.

In der mobilen app die [ `WebView` ](xref:Xamarin.Forms.WebView) werden an den Post Logout umleitungs-URI umgeleitet. Dies `WebView` Navigation führt dazu, dass die `NavigateAsync` -Methode in der die `LoginViewModel` -Klasse, die ausgeführt werden, die im folgenden Codebeispiel gezeigt wird:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

Diese Methode löscht das Identitätstoken und das Zugriffstoken aus Anwendungseinstellungen, und legt sie fest der `IsLogin` Eigenschaft `false`, bewirkt, dass die [ `WebView` ](xref:Xamarin.Forms.WebView) auf die `LoginView` Seite nicht sichtbar sind . Zum Schluss die `LoginUrl` -Eigenschaftensatz auf den URI von Identity Server [autorisierungsendpunkt](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html), mit den notwendigen Parametern aus, als Vorbereitung für das nächste Mal der Benutzer initiiert eine Anmeldung.

Weitere Informationen zu navigieren, finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informationen zur Verwendung [ `WebView` ](xref:Xamarin.Forms.WebView) Navigation bewirkt, dass eine View Model-Methode ausgeführt werden, finden Sie unter [aufrufen Navigation mithilfe von Verhaltensweisen](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Informationen zu Anwendungseinstellungen finden Sie unter [Konfigurationsverwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Bestellung von eShopOnContainers kann auch ein Mock abmelden, wenn die app konfiguriert ist, mit pseudodiensten in anzeigen. In diesem Modus kann die app keine Kommunikation mit Identity Server, und stattdessen löscht alle gespeicherten Token aus den Anwendungseinstellungen.

<a name="authorization" />

## <a name="authorization"></a>Autorisierung

Nach der Authentifizierung, ASP.NET Core-Web-APIs häufig zum Autorisieren des Zugriffs müssen, kann die ein Dienst APIs stellen einige authentifizierten Benutzern zur Verfügung, aber nicht für alle.

Einschränken des Zugriffs auf eine ASP.NET Core MVC-Route kann erreicht werden, durch Anwenden eines Authorize-Attributs auf einen Controller oder Aktion, die Zugriff auf den Controller beschränkt oder eine Aktion auf authentifizierte Benutzer, wie im folgenden Codebeispiel gezeigt:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Wenn ein nicht autorisierter Benutzer versucht, auf einen Controller oder die Aktion, die mit der `Authorize` -Attribut, das MVC-Framework gibt 401 (nicht autorisiert) HTTP-Statuscode zurück.

> [!NOTE]
> Parameter können angegeben werden, auf die `Authorize` Attribut, um eine API auf bestimmte Benutzer beschränken. Weitere Informationen finden Sie unter [Autorisierung](/aspnet/core/security/authorization/introduction/).

Identity Server kann in den autorisierungsworkflow integriert werden, sodass die Token des Access Control-Authentifizierung bietet. Dieser Ansatz wird in Abbildung 9-5 angezeigt.

![](authentication-and-authorization-images/authorization.png "Autorisierung von Zugriffstoken")

**Abbildung 9-5:** Autorisierung von Zugriffstoken

Die eShopOnContainers-mobile-app kommuniziert mit dem Identitäts-Microservice und fordert ein Zugriffstoken als Teil des Authentifizierungsprozesses. Das Zugriffstoken wird dann an die APIs, die von den Microservices Sortier- und Warenkorb verfügbar gemacht, als Teil der Anforderungen weitergeleitet. Access-Token enthalten Informationen zu den Client und der Benutzer. APIs verwenden diese Informationen dann zum Autorisieren des Zugriffs auf ihre Daten. Informationen zum Konfigurieren von Identity Server, um APIs zu schützen, finden Sie unter [-API-Ressourcen konfigurieren](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Konfigurieren von Identity Server für die Autorisierung

Um die Autorisierung mit Identity Server auszuführen, muss der Webanwendung HTTP-Anforderungspipeline die autorisierungsmiddleware hinzugefügt werden. Die Middleware hinzugefügt wird, die `ConfigureAuth` -Methode in der Webanwendung `Startup` -Klasse, die aufgerufen wird, aus der `Configure` -Methode und wird im folgenden Codebeispiel wird aus der referenzanwendung "eshoponcontainers" veranschaulicht:

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

Diese Methode wird sichergestellt, dass die API nur mit einem gültigen Zugriffstoken zugegriffen werden kann. Die Middleware überprüft das eingehende Token, um sicherzustellen, dass sie von einem vertrauenswürdigen Aussteller gesendet wird, und überprüft, ob das Token ist gültig, mit der API verwendet werden soll, die er empfängt. Aus diesem Grund gibt Navigieren zu der Sortierung oder die Warenkorb-Controller eine 401 (nicht autorisiert) HTTP-Statuscode, der angibt, dass ein Zugriffstoken erforderlich ist.

> [!NOTE]
> Identity Server die autorisierungsmiddleware muss der Webanwendung HTTP-Anforderungspipeline hinzugefügt werden, vor dem Hinzufügen von MVC mit `app.UseMvc()` oder `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>Anforderungen an APIs machen

Bei Anforderungen, die Reihenfolge und Basket Microservices, den Zugriff token von Identity Server abgerufen werden, während des Authentifizierungsprozesses muss in der Anforderung enthalten sein, wie im folgenden Codebeispiel gezeigt:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Das Zugriffstoken wird als anwendungseinstellung, gespeichert und aus plattformspezifischen Speicher abgerufen und in den Aufruf von enthalten die `GetOrderAsync` -Methode in der die `OrderService` Klasse.

Analog dazu muss das Zugriffstoken enthalten sein, wenn-API Senden von Daten an eine Identity-Server geschützt werden. wie im folgenden Codebeispiel gezeigt:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Das Zugriffstoken ist aus plattformspezifischen Speicher abgerufen und in den Aufruf der `UpdateBasketAsync` -Methode in der die `BasketService` Klasse.

Die `RequestProvider` -Klasse, in der mobilen app "eshoponcontainers" verwendet die `HttpClient` Klasse, um die Anforderungen an den RESTful-APIs verfügbar gemacht werden, indem der referenzanwendung "eshoponcontainers" zu senden. Wenn machen, um die Bestellung und die Warenkorb-APIs, die Autorisierung erforderlich ist anfordert, muss ein gültiges Zugriffstoken in der Anforderung enthalten sein. Dies erfolgt durch Hinzufügen des Zugriffstokens auf die Header der der `HttpClient` -Instanz, wie im folgenden Codebeispiel gezeigt:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

Die `DefaultRequestHeaders` Eigenschaft der `HttpClient` Klasse verfügbar macht, die Header, die mit jeder Anforderung gesendet werden, und das Zugriffstoken wird hinzugefügt. der `Authorization` Header, die mit der Zeichenfolge das Präfix `Bearer`. Wenn die Anforderung wird gesendet, einer RESTful-API, die den Wert des der `Authorization` Header extrahiert und überprüft, um sicherzustellen, dass sie von einem vertrauenswürdigen Aussteller gesendet hat, und verwendet, um zu bestimmen, ob der Benutzer die Berechtigung zum Aufrufen der API, die es empfängt.

Weitere Informationen dazu, wie die "eshoponcontainers" mobile app-webanforderungen macht, finden Sie unter [den Zugriff auf Remotedaten](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Zusammenfassung

Es gibt viele Ansätze zum Integrieren von Authentifizierung und Autorisierung in einer Xamarin.Forms-app, die mit einer ASP.NET MVC-Web-Anwendung kommuniziert. Die eShopOnContainers-mobile-app führt die Authentifizierung und Autorisierung mit einem Container Identitäts-Microservice, der IdentityServer 4 verwendet. Identity Server ist ein open-Source-Framework für OpenID Connect und OAuth 2.0 für ASP.NET Core, die in ASP.NET Core-Identität zum Ausführen von Bearer-token-Authentifizierung integriert.

Die mobile app wird die inhaltsortsanfrage Sicherheitstoken Identity Server, für die Authentifizierung eines Benutzers oder für den Zugriff auf eine Ressource. Wenn Sie eine Ressource zugreifen zu können, muss ein Zugriffstoken für den in der Anforderung an APIs enthalten sein, die eine Autorisierung erfordern. Identity Server Middleware überprüft eingehende Zugriffstoken, um sicherzustellen, dass sie von einem vertrauenswürdigen Aussteller gesendet werden und sie sind mit der API verwendet werden soll, die diese empfängt gültig.


## <a name="related-links"></a>Verwandte Links

- [E-Book (2Mb PDF-Datei) herunterladen](https://aka.ms/xamarinpatternsebook)
- ["eshoponcontainers" (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
