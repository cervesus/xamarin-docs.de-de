---
Title: "Authentifizierung und Autorisierung" Beschreibung: "in diesem Kapitel wird erläutert, wie der eshoponcontainers-Mobile App die Authentifizierung und Autorisierung für die containerisierten microservices ausführt."
ms. Prod: xamarin ms. assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 08/08/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Die Authentifizierung ist der Prozess, bei dem Identifikations Anmelde Informationen wie Name und Kennwort eines Benutzers erhalten und diese Anmelde Informationen anhand einer Autorität überprüft werden. Wenn die Anmelde Informationen gültig sind, wird die Entität, die die Anmelde Informationen übermittelt hat, als authentifizierte Identität angesehen. Nachdem eine Identität authentifiziert wurde, bestimmt ein Autorisierungs Prozess, ob diese Identität Zugriff auf eine bestimmte Ressource hat.

Es gibt viele Ansätze für die Integration von Authentifizierung und Autorisierung in eine- Xamarin.Forms app, die mit einer ASP.NET MVC-Webanwendung kommuniziert, einschließlich der Verwendung ASP.net Core Identität, externer Authentifizierungs Anbieter wie Microsoft, Google, Facebook oder Twitter und Authentifizierungs Middleware. Der eshoponcontainers-Mobile App führt die Authentifizierung und Autorisierung mit einem containerisierten Identitäts-microservice aus, der "identityserver 4" verwendet. Der Mobile App fordert Sicherheits Token von identityserver an, entweder zum Authentifizieren eines Benutzers oder zum Zugreifen auf eine Ressource. Damit identityserver Token im Namen eines Benutzers ausgibt, muss sich der Benutzer bei identityserver anmelden. Allerdings stellt identityserver keine Benutzeroberfläche oder Datenbank für die Authentifizierung bereit. Daher wird in der eshoponcontainers-Referenz Anwendung ASP.net Core Identität zu diesem Zweck verwendet.

## <a name="authentication"></a>Authentifizierung

Die Authentifizierung ist erforderlich, wenn eine Anwendung die Identität des aktuellen Benutzers kennen muss. Der primäre Mechanismus von ASP.net Core für die Identifizierung von Benutzern ist das ASP.net Core Identity-Mitgliedschaftssystem, das Benutzerinformationen in einem vom Entwickler konfigurierten Datenspeicher speichert. In der Regel handelt es sich bei diesem Datenspeicher um einen EntityFramework-Speicher, obwohl benutzerdefinierte Speicher oder Drittanbieter Pakete zum Speichern von Identitätsinformationen in Azure Storage, Azure Cosmos DB oder anderen Speicherorten verwendet werden können.

Für Authentifizierungs Szenarien, bei denen ein lokaler Benutzerdaten Speicher verwendet wird und die Identitätsinformationen zwischen Anforderungen über Cookies beibehalten (was in ASP.NET MVC-Webanwendungen typisch ist), ist ASP.net Core Identität eine passende Lösung. Cookies sind jedoch nicht immer eine natürliche Möglichkeit zum Speichern und übertragen von Daten. Beispielsweise muss eine ASP.net Core Webanwendung, die Rest-Endpunkte verfügbar macht, auf die von einem Mobile App zugegriffen wird, in der Regel bearertokenauthentifizierung verwenden, da Cookies in diesem Szenario nicht verwendet werden können. Bearertoken können jedoch problemlos abgerufen und in den Autorisierungs Header von Webanforderungen eingefügt werden, die aus der Mobile App abgerufen wurden.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Ausgeben von bearertoken mithilfe von identityserver 4

[Identityserver 4](https://github.com/IdentityServer/IdentityServer4) ist ein OpenID Connect-und OAuth 2,0-Open-Source-Framework für ASP.net Core, das für viele Authentifizierungs-und Autorisierungs Szenarios verwendet werden kann, einschließlich der Ausgabe von Sicherheits Token für lokale ASP.net Core Identitäts Benutzer.

> [!NOTE]
> OpenID Connect und OAuth 2,0 sind bei unterschiedlichen Zuständigkeiten sehr ähnlich.

OpenID Connect ist eine Authentifizierungs Ebene oberhalb des OAuth 2,0-Protokolls. OAuth 2 ist ein Protokoll, mit dem Anwendungen Zugriffs Token von einem Sicherheitstokendienst anfordern und für die Kommunikation mit APIs verwenden können. Diese Delegierung reduziert die Komplexität sowohl bei Client Anwendungen als auch bei APIs, da Authentifizierung und Autorisierung zentralisiert werden können.

Die Kombination von OpenID Connect und OAuth 2,0 kombiniert die beiden grundlegenden Sicherheitsaspekte der Authentifizierung und des API-Zugriffs, und identityserver 4 ist eine Implementierung dieser Protokolle.

In Anwendungen, die die direkte Kommunikation zwischen Client und-Dienst verwenden, z. b. die eshoponcontainers-Referenz Anwendung, kann ein dedizierter authentifizierungsmikrodienst, der als Sicherheitstokendienst (STS) fungiert, zum Authentifizieren von Benutzern verwendet werden, wie in Abbildung 9-1 dargestellt. Weitere Informationen zur direkten Kommunikation zwischen Client und mikroservice finden Sie unter [Kommunikation zwischen Client und-Dienst](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication-between-client-and-microservices).

![](authentication-and-authorization-images/authentication.png "Authentication by a dedicated authentication microservice")

**Abbildung 9-1:** Authentifizierung durch einen dedizierten authentifizierungmikrodienst

Der eshoponcontainers-Mobile App kommuniziert mit dem Identitäts-mikrodienst, der "identityserver 4" für die Authentifizierung und die Zugriffs Steuerung für APIs verwendet. Daher fordert der Mobile App Token von identityserver an, entweder zum Authentifizieren eines Benutzers oder zum Zugreifen auf eine Ressource:

- Das Authentifizieren von Benutzern mit identityserver wird durch die Mobile App erzielt, die ein *Identitäts* Token anfordert, das das Ergebnis eines Authentifizierungs Vorgangs darstellt. Es enthält mindestens einen Bezeichner für den Benutzer sowie Informationen dazu, wie und wann sich der Benutzer authentifiziert hat. Sie kann auch zusätzliche Identitätsdaten enthalten.
- Der Zugriff auf eine Ressource mit identityserver wird durch den Mobile App erreicht, der ein *Zugriffs* Token anfordert, das den Zugriff auf eine API-Ressource ermöglicht. Clients fordern Zugriffs Token an und leiten Sie an die API weiter. Zugriffs Token enthalten Informationen über den Client und den Benutzer (falls vorhanden). Die APIs verwenden diese Informationen dann, um den Zugriff auf Ihre Daten zu autorisieren.

> [!NOTE]
> Ein Client muss bei identityserver registriert sein, bevor Token angefordert werden können.

### <a name="adding-identityserver-to-a-web-application"></a>Hinzufügen von identityserver zu einer Webanwendung

Damit eine ASP.net Core Webanwendung identityserver 4 verwenden kann, muss Sie der Visual Studio-Projekt Mappe der Webanwendung hinzugefügt werden. Weitere Informationen finden Sie unter [Übersicht](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) in der identityserver-Dokumentation.

Nachdem identityserver in der Visual Studio-Projekt Mappe der Webanwendung enthalten ist, muss Sie der Pipeline für die HTTP-Anforderungs Verarbeitung der Webanwendung hinzugefügt werden, damit Sie Anforderungen an OpenID Connect-und OAuth 2,0-Endpunkte verarbeiten kann. Dies wird in der `Configure` -Methode in der-Klasse der Webanwendung erreicht `Startup` , wie im folgenden Codebeispiel gezeigt:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Die Reihenfolge spielt in der HTTP-Anforderungs Verarbeitungs Pipeline der Webanwendung eine Rolle. Daher muss identityserver der Pipeline vor dem UI-Framework hinzugefügt werden, das den Anmeldebildschirm implementiert.

### <a name="configuring-identityserver"></a>Konfigurieren von identityserver

Identityserver sollte in der `ConfigureServices` -Methode in der-Klasse der Webanwendung konfiguriert werden `Startup` , indem die-Methode aufgerufen wird `services.AddIdentityServer` , wie im folgenden Codebeispiel aus der eshoponcontainers-Referenz Anwendung veranschaulicht:

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

Nach dem Aufrufen der- `services.AddIdentityServer` Methode werden weitere fließende APIs aufgerufen, um Folgendes zu konfigurieren:

- Zum Signieren verwendete Anmelde Informationen.
- API-und Identitäts Ressourcen, auf die Benutzer möglicherweise Zugriff anfordern.
- Clients, die eine Verbindung mit Anforderungs Token herstellen.
- ASP.net Core Identität.

> [!TIP]
> Dynamisches Laden der identityserver 4-Konfiguration. Die APIs von identityserver 4 ermöglichen das Konfigurieren von identityserver aus einer Speicher internen Liste von Konfigurationsobjekten. In der eshoponcontainers-Referenz Anwendung sind diese in-Memory-Auflistungen in der Anwendung hart codiert. In Produktionsszenarien können Sie jedoch dynamisch aus einer Konfigurationsdatei oder aus einer Datenbank geladen werden.

Informationen zum Konfigurieren von identityserver für die Verwendung ASP.net Core der Identität finden [Sie unter Using ASP.net Core Identity](https://identityserver4.readthedocs.io/en/latest/quickstarts/6_aspnet_identity.html) in der identityserver-Dokumentation.

#### <a name="configuring-api-resources"></a>Konfigurieren von API-Ressourcen

Beim Konfigurieren von API-Ressourcen `AddInMemoryApiResources` erwartet die-Methode eine-Auflistung `IEnumerable<ApiResource>` . Im folgenden Codebeispiel wird die- `GetApis` Methode veranschaulicht, die diese Sammlung in der eshoponcontainers-Referenz Anwendung bereitstellt:

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

Diese Methode gibt an, dass identityserver die Aufträge und die Warenkorb-APIs schützen soll. Daher sind die identityserver-Zugriffs Token, die beim Aufrufen dieser APIs verwendet werden, erforderlich. Weitere Informationen zum- `ApiResource` Typ finden Sie unter [API-Ressource](https://identityserver4.readthedocs.io/en/latest/reference/api_resource.html) in der identityserver 4-Dokumentation.

#### <a name="configuring-identity-resources"></a>Konfigurieren von Identitäts Ressourcen

Beim Konfigurieren von Identitäts Ressourcen erwartet die- `AddInMemoryIdentityResources` Methode eine-Auflistung `IEnumerable<IdentityResource>` . Identitäts Ressourcen sind Daten wie z. b. Benutzer-ID, Name oder e-Mail-Adresse. Jede Identitäts Ressource hat einen eindeutigen Namen, und es können beliebige Anspruchs Typen zugewiesen werden, die dann in das Identitäts Token für den Benutzer eingeschlossen werden. Im folgenden Codebeispiel wird die- `GetResources` Methode veranschaulicht, die diese Sammlung in der eshoponcontainers-Referenz Anwendung bereitstellt:

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

In der OpenID Connect-Spezifikation sind einige [Standard-Identitäts Ressourcen](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)angegeben. Die Mindestanforderung besteht darin, dass die Unterstützung für die Ausgabe einer eindeutigen ID für Benutzer bereitgestellt wird. Dies wird erreicht, indem die Identity-Ressource verfügbar gemacht wird `IdentityResources.OpenId` .

> [!NOTE]
> Die- `IdentityResources` Klasse unterstützt alle Bereiche, die in der OpenID Connect-Spezifikation (OpenID, e-Mail, Profil, Telefon und Adresse) definiert sind.

Identityserver unterstützt auch das Definieren von benutzerdefinierten Identitäts Ressourcen. Weitere Informationen finden Sie unter [Definieren von benutzerdefinierten Identitäts Ressourcen](http://docs.identityserver.io/en/latest/topics/resources.html#defining-custom-identity-resources) in der identityserver-Dokumentation. Weitere Informationen zum- `IdentityResource` Typ finden Sie unter [Identity-Ressource](https://identityserver4.readthedocs.io/en/latest/reference/identity_resource.html) in der identityserver 4-Dokumentation.

#### <a name="configuring-clients"></a>Configuring Clients (Konfigurieren von Clients)

Clients sind Anwendungen, die Token von identityserver anfordern können. In der Regel müssen die folgenden Einstellungen für jeden Client als minimal definiert werden:

- Eine eindeutige Client-ID.
- Die zulässigen Interaktionen mit dem Tokendienst (als "Grant Type" bezeichnet).
- Der Speicherort, an den Identitäts-und Zugriffs Token gesendet werden (als Umleitungs-URI bezeichnet).
- Eine Liste von Ressourcen, auf die der Client Zugriff hat (als Bereiche bezeichnet).

Beim Konfigurieren von Clients erwartet die- `AddInMemoryClients` Methode eine-Auflistung `IEnumerable<Client>` . Das folgende Codebeispiel zeigt die Konfiguration für die eshoponcontainers-Mobile App in der- `GetClients` Methode, die diese Sammlung in der eshoponcontainers-Referenz Anwendung bereitstellt:

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

- `ClientId`: Eine eindeutige ID für den Client.
- `ClientName`: Der Client Anzeige Name, der für die Protokollierung und den Zustimmungs Bildschirm verwendet wird.
- `AllowedGrantTypes`: Gibt an, wie ein Client mit identityserver interagieren möchte. Weitere Informationen finden Sie [unter Konfigurieren des Authentifizierungs Flusses](#configuring-the-authentication-flow).
- `ClientSecrets`: Gibt die geheimen Client Schlüssel Anmelde Informationen an, die beim Anfordern von Token vom tokenendpunkt verwendet werden.
- `RedirectUris`: Gibt die zulässigen URIs an, an die Token oder Autorisierungscodes zurückgegeben werden sollen.
- `RequireConsent`: Gibt an, ob ein Zustimmungs Bildschirm erforderlich ist.
- `RequirePkce`: Gibt an, ob Clients, die einen Autorisierungs Code verwenden, einen Nachweis Schlüssel senden müssen.
- `PostLogoutRedirectUris`: Gibt die zulässigen URIs an, an die nach der Abmeldung umgeleitet werden soll.
- `AllowedCorsOrigins`: Gibt den Ursprung des Clients an, sodass identityserver Ursprungs übergreifende Aufrufe vom Ursprung zulassen kann.
- `AllowedScopes`: Gibt die Ressourcen an, auf die der Client Zugriff hat. Standardmäßig hat ein Client keinen Zugriff auf Ressourcen.
- `AllowOfflineAccess`: Gibt an, ob der Client Aktualisierungs Token anfordern kann.

#### <a name="configuring-the-authentication-flow"></a>Konfigurieren des Authentifizierungs Flusses

Der Authentifizierungs Fluss zwischen einem Client und identityserver kann konfiguriert werden, indem die Grant-Typen in der-Eigenschaft angegeben werden `Client.AllowedGrantTypes` . Die OpenID Connect-und OAuth 2,0-Spezifikationen definieren eine Reihe von Authentifizierungs Läufen, einschließlich:

- Implicit. Dieser Flow ist für browserbasierte Anwendungen optimiert und sollte entweder für Benutzerauthentifizierung oder für Authentifizierungs-und zugriffstokenanforderungen verwendet werden. Alle Token werden über den Browser übertragen. Daher sind erweiterte Funktionen wie Aktualisierungs Token nicht zulässig.
- Autorisierungs Code. Dieser Flow bietet die Möglichkeit zum Abrufen von Token in einem backchannel, im Gegensatz zum Browser-Front-Channel, während gleichzeitig die Client Authentifizierung unterstützt wird.
- Hybrid. Dieser Flow ist eine Kombination der impliziten Typen für die Autorisierungs Code Gewährung. Das Identitäts Token wird über den Browser Kanal übertragen und enthält die signierte Protokoll Antwort zusammen mit anderen Artefakten, wie z. b. dem Autorisierungs Code. Nach erfolgreicher Überprüfung der Antwort sollte der Back-Channel verwendet werden, um das Zugriffs-und Aktualisierungs Token abzurufen.

> [!TIP]
> Verwenden Sie den Hybrid-Authentifizierungs Fluss. Der Hybrid Authentifizierungs Fluss reduziert eine Reihe von Angriffen, die für den browserchannel gelten, und ist der empfohlene Fluss für native Anwendungen, die Zugriffs Token abrufen möchten (und möglicherweise Aktualisierungs Token).

Weitere Informationen zu Authentifizierungs Abläufen finden Sie unter [Grant Types](https://identityserver4.readthedocs.io/en/latest/topics/grant_types.html) in der identityserver 4-Dokumentation.

### <a name="performing-authentication"></a>Authentifizierung wird durchgeführt

Damit identityserver Token im Namen eines Benutzers ausgibt, muss sich der Benutzer bei identityserver anmelden. Allerdings stellt identityserver keine Benutzeroberfläche oder Datenbank für die Authentifizierung bereit. Daher wird in der eshoponcontainers-Referenz Anwendung ASP.net Core Identität zu diesem Zweck verwendet.

Die eshoponcontainers-Mobile App authentifiziert sich mit identityserver mit dem Hybrid Authentifizierungs Fluss, der in Abbildung 9-2 veranschaulicht wird.

![](authentication-and-authorization-images/sign-in.png "High-level overview of the sign-in process")

**Abbildung 9-2:** Allgemeine Übersicht über den Anmeldevorgang

Es erfolgt eine Anmelde Anforderung an `<base endpoint>:5105/connect/authorize` . Nach erfolgreicher Authentifizierung gibt identityserver eine Authentifizierungs Antwort mit einem Autorisierungs Code und einem Identitäts Token zurück. Der Autorisierungs Code wird dann an gesendet `<base endpoint>:5105/connect/token` , was mit Zugriffs-, Identitäts-und Aktualisierungs Token antwortet.

Die eshoponcontainers-Mobile App von identityserver abgemeldet, indem eine Anforderung an `<base endpoint>:5105/connect/endsession` mit zusätzlichen Parametern gesendet wird. Nach der Abmeldung antwortet identityserver, indem er einen Umleitungs-URI nach der Abmeldung an den Mobile App zurücksendet. In Abbildung 9-3 wird dieser Prozess veranschaulicht.

![](authentication-and-authorization-images/sign-out.png "High-level overview of the sign-out process")

**Abbildung 9-3:** Allgemeine Übersicht über den Abmeldevorgang

Im eshoponcontainers-Mobile App wird die Kommunikation mit identityserver von der- `IdentityService` Klasse ausgeführt, die die- `IIdentityService` Schnittstelle implementiert. Diese Schnittstelle gibt an, dass die implementierende Klasse `CreateAuthorizationRequest` , `CreateLogoutRequest` -und-Methoden bereitstellen muss `GetTokenAsync` .

#### <a name="signing-in"></a>Anmelden

Wenn der Benutzer auf die **Anmelde** Schaltfläche in tippt `LoginView` , `SignInCommand` wird die in der- `LoginViewModel` Klasse ausgeführt, die wiederum die- `SignInAsync` Methode ausführt. Im folgenden Codebeispiel wird diese Methode veranschaulicht:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Diese Methode ruft die- `CreateAuthorizationRequest` Methode in der- `IdentityService` Klasse auf, die im folgenden Codebeispiel gezeigt wird:

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
    dic.Add("redirect_uri", GlobalSetting.Instance.Callback);
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

Diese Methode erstellt den URI für den [Autorisierungs Endpunkt](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)von identityserver mit den erforderlichen Parametern. Der Autorisierungs Endpunkt befindet sich am `/connect/authorize` Port 5105 des Basis Endpunkts, der als Benutzereinstellung verfügbar gemacht wird. Weitere Informationen zu Benutzereinstellungen finden Sie unter [Konfigurations Verwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Die Angriffsfläche des eshoponcontainers-Mobile App wird verringert, indem der Prüfschlüssel für die Code Austausch Erweiterung (pkce) in OAuth implementiert wird. Pkce schützt den Autorisierungs Code vor der Verwendung, wenn er abgefangen wird. Dies wird erreicht, indem der Client eine geheime Überprüfung erstellt, ein Hash, der in der Autorisierungs Anforderung übermittelt wird und der beim Einlösen des Autorisierungscodes als unhashwert dargestellt wird. Weitere Informationen zu pkce finden Sie unter [Prüfschlüssel für den Code Austausch durch öffentliche OAuth-Clients](https://tools.ietf.org/html/rfc7636) auf der Website "Internet Engineering Task Force".

Der zurückgegebene URI wird in der- `LoginUrl` Eigenschaft der- `LoginViewModel` Klasse gespeichert. Wenn die- `IsLogin` Eigenschaft wird `true` , [`WebView`](xref:Xamarin.Forms.WebView) wird der in der `LoginView` sichtbar. Die `WebView` [`Source`](xref:Xamarin.Forms.WebView.Source) -Eigenschaft bindet die-Eigenschaft an die `LoginUrl` -Eigenschaft der `LoginViewModel` -Klasse und führt daher eine Anmelde Anforderung an identityserver aus, wenn die `LoginUrl` -Eigenschaft auf den Autorisierungs Endpunkt von identityserver festgelegt ist. Wenn identityserver diese Anforderung empfängt und der Benutzer nicht authentifiziert ist, `WebView` wird der an die konfigurierte Anmeldeseite umgeleitet, die in Abbildung 9-4 dargestellt wird.

![](authentication-and-authorization-images/login.png "Login page displayed by the WebView")

**Abbildung 9-4:** Von der WebView angezeigte Anmeldeseite

Nachdem die Anmeldung abgeschlossen ist, [`WebView`](xref:Xamarin.Forms.WebView) wird der an einen Rückgabe-URI umgeleitet. Diese `WebView` Navigation bewirkt `NavigateAsync` , dass die-Methode in der-Klasse ausgeführt wird. dies `LoginViewModel` wird im folgenden Codebeispiel gezeigt:

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

Diese Methode analysiert die Authentifizierungs Antwort, die im Rückgabe-URI enthalten ist, und stellt fest, dass ein gültiger Autorisierungs Code vorhanden ist, sendet eine Anforderung an den [tokenendpunkt](https://identityserver4.readthedocs.io/en/latest/endpoints/token.html)von identityserver, übergibt den Autorisierungs Code, den geheimen Schlüssel Prüfer und andere erforderliche Parameter. Der tokenendpunkt befindet sich am `/connect/token` Port 5105 des Basis Endpunkts, der als Benutzereinstellung verfügbar gemacht wird. Weitere Informationen zu Benutzereinstellungen finden Sie unter [Konfigurations Verwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!TIP]
> Validieren Sie Rückgabe-URIs. Obwohl der eshoponcontainers-Mobile App den Rückgabe-URI nicht überprüft, empfiehlt es sich, zu überprüfen, ob der Rückgabe-URI auf einen bekannten Speicherort verweist, um Open-Redirect-Angriffe zu verhindern.

Wenn der tokenendpunkt einen gültigen Autorisierungs Code und einen geheimen pkce-Verifizierer erhält, antwortet er mit einem Zugriffs Token, einem Identitäts Token und einem Aktualisierungs Token. Das Zugriffs Token (das den Zugriff auf API-Ressourcen zulässt) und das Identitäts Token werden dann als Anwendungseinstellungen gespeichert, und die Seitennavigation wird durchgeführt. Daher ist der Gesamteffekt in den eshoponcontainers-Mobile App dies: vorausgesetzt, dass sich Benutzer erfolgreich bei identityserver authentifizieren können, werden Sie zu der Seite navigiert `MainView` , in der [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) die `CatalogView` als ausgewählte Registerkarte angezeigt wird.

Weitere Informationen zur Seitennavigation finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informationen dazu, wie [`WebView`](xref:Xamarin.Forms.WebView) die Navigation bewirkt, dass eine Ansichts Modell Methode ausgeführt wird, finden [Sie unter Aufrufen der Navigation mithilfe von Verhaltensweisen](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking-navigation-using-behaviors). Weitere Informationen zu Anwendungseinstellungen finden Sie unter [Konfigurations Verwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Eshoponcontainers ermöglicht außerdem eine Mock-Anmeldung, wenn die APP für die Verwendung von mockdiensten in der konfiguriert ist `SettingsView` . In diesem Modus kommuniziert die APP nicht mit identityserver, sondern ermöglicht es dem Benutzer, sich mit beliebigen Anmelde Informationen anzumelden.

#### <a name="signing-out"></a>Abmelden

Wenn der Benutzer auf die **Abmelde** Schaltfläche in der tippt `ProfileView` , `LogoutCommand` wird die in der- `ProfileViewModel` Klasse ausgeführt, die wiederum die- `LogoutAsync` Methode ausführt. Diese Methode führt die Seitennavigation zur `LoginView` Seite durch und übergibt dabei eine-Instanz, die `LogoutParameter` als Parameter auf festgelegt ist `true` . Weitere Informationen zum Übergeben von Parametern während der Seitennavigation finden Sie unter [übergeben von Parametern während der Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing-parameters-during-navigation).

Wenn eine Sicht erstellt und dorthin navigiert wird, `InitializeAsync` wird die-Methode des zugeordneten Ansichts Modells der Sicht ausgeführt, das dann die- `Logout` Methode der-Klasse ausführt, die `LoginViewModel` im folgenden Codebeispiel gezeigt wird:

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

Diese Methode ruft die- `CreateLogoutRequest` Methode in der- `IdentityService` Klasse auf und übergibt das Identitäts Token, das aus den Anwendungseinstellungen abgerufen wurde, als Parameter. Weitere Informationen zu Anwendungseinstellungen finden Sie unter [Konfigurations Verwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). Die `CreateLogoutRequest`-Methode wird in folgendem Codebeispiel veranschaulicht:

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

Diese Methode erstellt den URI für den [endsitzungs Endpunkt](https://identityserver4.readthedocs.io/en/latest/endpoints/endsession.html#refendsession)von identityserver mit den erforderlichen Parametern. Der Endpunkt der Endsitzung befindet sich am `/connect/endsession` Port 5105 des Basis Endpunkts, der als Benutzereinstellung verfügbar gemacht wird. Weitere Informationen zu Benutzereinstellungen finden Sie unter [Konfigurations Verwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Der zurückgegebene URI wird in der- `LoginUrl` Eigenschaft der- `LoginViewModel` Klasse gespeichert. Während die- `IsLogin` Eigenschaft ist `true` , [`WebView`](xref:Xamarin.Forms.WebView) ist der in der `LoginView` sichtbar. Die `WebView` [`Source`](xref:Xamarin.Forms.WebView.Source) -Eigenschaft bindet die-Eigenschaft an die `LoginUrl` -Eigenschaft der `LoginViewModel` -Klasse und führt daher eine Abmelde Anforderung an identityserver aus, wenn die `LoginUrl` -Eigenschaft auf den endsitzungs Endpunkt von identityserver festgelegt ist. Wenn identityserver diese Anforderung empfängt, sofern der Benutzer angemeldet ist, erfolgt die Abmeldung. Die Authentifizierung wird mit einem Cookie nachverfolgt, das von der Cookie-Authentifizierungs Middleware aus ASP.net Core verwaltet wird. Daher wird beim Abmelden von identityserver das Authentifizierungs Cookie entfernt und ein Umleitungs-URI nach der Abmeldung an den Client zurückgesendet.

Im Mobile App [`WebView`](xref:Xamarin.Forms.WebView) wird der an den Umleitungs-URI nach der Abmeldung umgeleitet. Diese `WebView` Navigation bewirkt `NavigateAsync` , dass die-Methode in der-Klasse ausgeführt wird. dies `LoginViewModel` wird im folgenden Codebeispiel gezeigt:

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

Diese Methode löscht sowohl das Identitäts Token als auch das Zugriffs Token aus den Anwendungseinstellungen und legt die- `IsLogin` Eigenschaft auf fest `false` [`WebView`](xref:Xamarin.Forms.WebView) . Dadurch wird der auf der `LoginView` Seite unsichtbar. Zum Schluss `LoginUrl` wird die-Eigenschaft auf den URI des [Autorisierungs Endpunkts](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)von identityserver mit den erforderlichen Parametern festgelegt, der für das nächste Mal vorbereitet wird, wenn der Benutzer eine Anmeldung initiiert.

Weitere Informationen zur Seitennavigation finden Sie unter [Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md). Informationen dazu, wie [`WebView`](xref:Xamarin.Forms.WebView) die Navigation bewirkt, dass eine Ansichts Modell Methode ausgeführt wird, finden [Sie unter Aufrufen der Navigation mithilfe von Verhaltensweisen](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking-navigation-using-behaviors). Weitere Informationen zu Anwendungseinstellungen finden Sie unter [Konfigurations Verwaltung](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Eshoponcontainers ermöglicht außerdem eine Mock-Abmeldung, wenn die APP für die Verwendung von mockdiensten in der settingsview konfiguriert ist. In diesem Modus kommuniziert die APP nicht mit identityserver und löscht stattdessen alle gespeicherten Token aus den Anwendungseinstellungen.

## <a name="authorization"></a>Authorization

Nach der Authentifizierung müssen ASP.net Core Web-APIs oft den Zugriff autorisieren, sodass ein Dienst APIs für einige authentifizierte Benutzer verfügbar machen kann, aber nicht für alle.

Das Einschränken des Zugriffs auf eine ASP.net Core MVC-Route kann durch Anwenden eines Autorisierungs Attributs auf einen Controller oder eine Aktion erreicht werden, wodurch der Zugriff auf den Controller oder die Aktion auf authentifizierte Benutzer beschränkt wird, wie im folgenden Codebeispiel gezeigt:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Wenn ein nicht autorisierter Benutzer versucht, auf einen Controller oder eine Aktion zuzugreifen, der mit dem-Attribut markiert ist `Authorize` , gibt das MVC-Framework den HTTP-Statuscode 401 (nicht autorisiert) zurück.

> [!NOTE]
> Parameter können für das-Attribut angegeben werden `Authorize` , um eine API auf bestimmte Benutzer zu beschränken. Weitere Informationen finden Sie unter [Autorisierung](/aspnet/core/security/authorization/introduction/).

Identityserver kann in den Autorisierungs Workflow integriert werden, sodass die Zugriffs Token, die es bereitstellt, die Autorisierung steuern. Diese Vorgehensweise ist in Abbildung 9-5 dargestellt.

![](authentication-and-authorization-images/authorization.png "Authorization by access token")

**Abbildung 9-5:** Autorisierung durch Zugriffs Token

Der eshoponcontainers-Mobile App kommuniziert mit dem Identitäts-mikrodienst und fordert im Rahmen des Authentifizierungs Vorgangs ein Zugriffs Token an. Das Zugriffs Token wird dann an die APIs weitergeleitet, die von der Bestellung und den Warenkorb-microservices als Teil der Zugriffs Anforderungen verfügbar gemacht werden. Zugriffs Token enthalten Informationen über den Client und den Benutzer. Die APIs verwenden diese Informationen dann, um den Zugriff auf Ihre Daten zu autorisieren. Informationen zum Konfigurieren von identityserver zum Schützen von APIs finden Sie unter [Konfigurieren von API-Ressourcen](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Konfigurieren von identityserver zum Ausführen der Autorisierung

Um die Autorisierung mit identityserver auszuführen, muss die zugehörige Autorisierungs Middleware zur HTTP-Anforderungs Pipeline der Webanwendung hinzugefügt werden. Die Middleware wird in der `ConfigureAuth` -Methode in der-Klasse der Webanwendung hinzugefügt `Startup` , die von der-Methode aufgerufen wird `Configure` , und wird im folgenden Codebeispiel aus der eshoponcontainers-Referenz Anwendung veranschaulicht:

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

Diese Methode stellt sicher, dass auf die API nur mit einem gültigen Zugriffs Token zugegriffen werden kann. Die Middleware überprüft das eingehende Token, um sicherzustellen, dass es von einem vertrauenswürdigen Aussteller gesendet wird, und überprüft, ob das Token für die Verwendung mit der API gültig ist, die es empfängt. Daher wird durch das Navigieren zu der Reihenfolge oder dem Korb Controller der HTTP-Statuscode 401 (nicht autorisiert) zurückgegeben, der angibt, dass ein Zugriffs Token erforderlich ist.

> [!NOTE]
> Die Autorisierungs Middleware von identityserver muss der HTTP-Anforderungs Pipeline der Webanwendung hinzugefügt werden, bevor MVC mit oder hinzugefügt wird `app.UseMvc()` `app.UseMvcWithDefaultRoute()` .

### <a name="making-access-requests-to-apis"></a>Herstellen von Zugriffs Anforderungen an APIs

Beim Anfordern von Anforderungen an die Bestell-und waren Korb-microservices muss das Zugriffs Token, das von identityserver während des Authentifizierungsprozesses abgerufen wird, in der Anforderung enthalten sein, wie im folgenden Codebeispiel gezeigt:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Das Zugriffs Token wird als Anwendungs Einstellung gespeichert und aus dem plattformspezifischen Speicher abgerufen und im Aufrufen der- `GetOrderAsync` Methode in der- `OrderService` Klasse enthalten.

Ebenso muss das Zugriffs Token beim Senden von Daten an eine von identityserver geschützte API eingeschlossen werden, wie im folgenden Codebeispiel gezeigt:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Das Zugriffs Token wird aus dem plattformspezifischen Speicher abgerufen und im Aufrufen der- `UpdateBasketAsync` Methode in der- `BasketService` Klasse enthalten.

Die- `RequestProvider` Klasse in der eshoponcontainers-Mobile App verwendet die-Klasse, um Anforderungen an die Rest-APIs zu senden, die `HttpClient` von der eshoponcontainers-Referenz Anwendung verfügbar gemacht werden. Beim Anfordern von Anforderungen an die Bestell-und Korb-APIs, die eine Autorisierung erfordern, muss ein gültiges Zugriffs Token in der Anforderung enthalten sein. Dies wird erreicht, indem das Zugriffs Token den Headern der- `HttpClient` Instanz hinzugefügt wird, wie im folgenden Codebeispiel gezeigt:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

Die `DefaultRequestHeaders` -Eigenschaft der- `HttpClient` Klasse macht die Header verfügbar, die mit jeder Anforderung gesendet werden, und das Zugriffs Token wird dem Header hinzugefügt, dem `Authorization` die Zeichenfolge als Präfix vorangestellt ist `Bearer` . Wenn die Anforderung an eine Rest-API gesendet wird, wird der Wert des `Authorization` -Headers extrahiert und überprüft, um sicherzustellen, dass er von einem vertrauenswürdigen Aussteller gesendet wird. er wird verwendet, um zu bestimmen, ob der Benutzer über die Berechtigung zum Aufrufen der API verfügt, die ihn empfängt.

Weitere Informationen zur Verwendung von Webanforderungen durch das eshoponcontainers-Mobile App finden Sie unter [zugreifen auf Remote Daten](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Zusammenfassung

Es gibt viele Ansätze für die Integration von Authentifizierung und Autorisierung in eine- Xamarin.Forms app, die mit einer ASP.NET MVC-Webanwendung kommuniziert. Der eshoponcontainers-Mobile App führt die Authentifizierung und Autorisierung mit einem containerisierten Identitäts-microservice aus, der "identityserver 4" verwendet. Identityserver ist ein OpenID Connect-und OAuth 2,0-Open-Source-Framework für ASP.net Core, die in ASP.net Core Identity integriert ist, um bearertokenauthentifizierung auszuführen.

Der Mobile App fordert Sicherheits Token von identityserver an, entweder zum Authentifizieren eines Benutzers oder zum Zugreifen auf eine Ressource. Beim Zugriff auf eine Ressource muss ein Zugriffs Token in die Anforderung an APIs eingeschlossen werden, die eine Autorisierung erfordern. Die Middleware von identityserver überprüft eingehende Zugriffs Token, um sicherzustellen, dass Sie von einem vertrauenswürdigen Aussteller gesendet werden, und dass Sie für die Verwendung mit der API, die Sie empfängt, gültig sind.

## <a name="related-links"></a>Verwandte Links

- [Download-e-Book (2 MB PDF)](https://aka.ms/xamarinpatternsebook)
- [eshoponcontainers (GitHub) (Beispiel)](https://github.com/dotnet-architecture/eShopOnContainers)
