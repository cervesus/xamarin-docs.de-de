---
Title: "verwenden Sie die Anmeldung mit Apple für Xamarin.Forms " Description: "erfahren Sie, wie Sie die Anmeldung mit Apple in Ihren Xamarin.Forms mobilen Anwendungen implementieren."
ms. Prod: xamarin ms. assetid: 2e47e7f 2-93d4-4ca3-9e66-247466d25e4d ms. Technology: xamarin-Forms Author: davidortinau ms. Author: daortin ms. Date: 09/10/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="use-sign-in-with-apple-in-xamarinforms"></a>Verwenden von anmelden mit Apple inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/signinwithapple/)

Die Anmeldung mit Apple ist für alle neuen Anwendungen auf IOS 13, die Authentifizierungsdienste von Drittanbietern verwenden. Die Implementierungsdetails zwischen IOS und Android unterscheiden sich erheblich. In dieser Anleitung wird erläutert, wie Sie dies noch heute in tun können Xamarin.Forms .

In dieser Anleitung und im Beispiel werden bestimmte Platt Form Dienste verwendet, um die Anmeldung mit Apple zu behandeln:

- Android mit einem generischen Webdienst, der mit Azure Functions mit OpenID/openauth kommuniziert
- IOS verwendet die Native API für die Authentifizierung unter IOS 13 und greift auf einen generischen Webdienst für IOS 12 und niedriger zurück.

## <a name="a-sample-apple-sign-in-flow"></a>Ein Beispiel für einen Apple-Anmelde Fluss

Dieses Beispiel bietet eine Einführung in die Implementierung, um die Apple-Anmeldung in Ihrer APP zu erhalten Xamarin.Forms .

Wir verwenden zwei Azure Functions, um den Authentifizierungs Ablauf zu unterstützen:

1. `applesignin_auth`-Generiert die URL für die URL-Anmelde Autorisierung und leitet Sie an Sie weiter.  Dies geschieht auf der Serverseite und nicht auf dem Mobile App, daher können wir das Zwischenspeichern und überprüfen, `state` Wenn Apple-Server einen Rückruf senden.
2. `applesignin_callback`-Behandelt den Post-Rückruf von Apple und tauscht den Autorisierungs Code sicher für ein Zugriffs Token und ein ID-Token aus.  Schließlich leitet Sie zurück zum URI-Schema der APP und übergibt die Token in einem URL-Fragment.

Der Mobile App registriert sich selbst, um das benutzerdefinierte URI-Schema zu verarbeiten, das wir ausgewählt haben (in diesem Fall `xamarinformsapplesignin://` ), damit die `applesignin_callback` Funktion die Token zurück an die Funktion weiterleiten kann.

Wenn der Benutzer die Authentifizierung startet, werden die folgenden Schritte ausgeführt:

1. Der Mobile App generiert einen `nonce` - `state` Wert und einen-Wert und übergibt ihn an die `applesignin_auth` Azure-Funktion.
2. Die `applesignin_auth` Azure-Funktion generiert eine URL für die URL-Anmelde Autorisierung (unter Verwendung des bereitgestellten `state` und `nonce` ) und leitet den Mobile App Browser an Sie weiter.
3. Der Benutzer gibt seine Anmelde Informationen sicher auf der Seite "Apple Sign in Authorization" ein, die auf den Apple-Servern gehostet wird.
4. Nachdem der Apple-Anmelde Ablauf auf den Servern von Apple abgeschlossen ist, leitet Apple an die weiter, die `redirect_uri` die Azure-Funktion sein wird `applesignin_callback` .
5. Die Anforderung von Apple, die an die Funktion gesendet wird, `applesignin_callback` wird überprüft, um sicherzustellen `state` , dass die korrekte Rückgabe erfolgt und dass die ID-Tokenansprüche gültig sind.
6. Die `applesignin_callback` Azure-Funktion tauscht den, der `code` von Apple gepostet wird, für ein _Zugriffs Token_, ein _Aktualisierungs Token_und ein _ID-Token_ aus, das Ansprüche über die Benutzer-ID, den Namen und die e-Mail-Adresse enthält.
7. Die `applesignin_callback` Azure-Funktion führt schließlich eine Umleitung zurück zum URI-Schema der APP (), das `xamarinformsapplesignin://` ein URI-Fragment mit den Token anfügt (z. b. `xamarinformsapplesignin://#access_token=...&refresh_token=...&id_token=...` ).
8. Die Mobile App analysiert das URI-Fragment in ein `AppleAccount` und überprüft, dass der `nonce` empfangene Anspruch `nonce` mit dem zu Beginn des Flows generierten übereinstimmt.
9. Der Mobile App ist jetzt authentifiziert!

## <a name="azure-functions"></a>Azure-Funktionen

In diesem Beispiel wird Azure Functions verwendet. Alternativ kann ein ASP.net Core Controller oder eine ähnliche Webserver Lösung die gleiche Funktionalität bereitzustellen.

### <a name="configuration"></a>Konfiguration

Bei der Verwendung Azure Functions müssen mehrere App-Einstellungen konfiguriert werden:

- `APPLE_SIGNIN_KEY_ID`-Dies ist der `KeyId` von früher.
- `APPLE_SIGNIN_TEAM_ID`-Dies ist normalerweise Ihre _Team-ID_ in Ihrem [Mitgliedschafts Profil](https://developer.apple.com/account/#/membership) .
- `APPLE_SIGNIN_SERVER_ID`: Dies ist der `ServerId` von früher.  Dabei handelt es sich *nicht* um die APP- _Bündel-ID_, sondern vielmehr um den *Bezeichner* der von Ihnen erstellten *Dienst-ID* .
- `APPLE_SIGNIN_APP_CALLBACK_URI`: Dies ist das benutzerdefinierte URI-Schema, das Sie mit an Ihre APP zurückleiten möchten.  In diesem Beispiel `xamarinformsapplesignin://` wird verwendet.
- `APPLE_SIGNIN_REDIRECT_URI`: Die *Umleitungs-URL* , die Sie beim Erstellen Ihrer *Dienst-ID* im Abschnitt *Apple Sign in* Configuration einrichten.  Zum Testen könnte es etwa wie folgt aussehen:`http://local.test:7071/api/applesignin_callback`
- `APPLE_SIGNIN_P8_KEY`: Der Text Inhalt der `.p8` Datei, wobei alle Zeilenumbrüche entfernt werden, `\n` sodass es sich um eine lange Zeichenfolge handelt.

### <a name="security-considerations"></a>Sicherheitshinweise

Speichern Sie Ihren P8-Schlüssel **niemals** innerhalb Ihres Anwendungs Codes. Der Anwendungscode kann einfach heruntergeladen und disassembliert werden. 

Es wird auch als bewährte Vorgehensweise verwendet, um `WebView` den Authentifizierungs Fluss zu hosten und URL-Navigations Ereignisse abzufangen, um den Autorisierungs Code zu erhalten. Zurzeit gibt es zurzeit keine vollständig sichere Methode, um die Anmeldung bei Apple auf nicht-iOS13 +-Geräten durchzuführen, ohne Code auf einem Server zu verwalten, um den tokenaustausch zu verarbeiten. Es wird empfohlen, den Autorisierungs-URL-Generierungs Code auf einem Server zu speichern, damit Sie den Status Zwischenspeichern und validieren können, wenn Apple einen Post-Rückruf an Ihren Server ausgibt

## <a name="a-cross-platform-sign-in-service"></a>Ein plattformübergreifender Anmeldedienst

Mithilfe Xamarin.Forms von dependencyservice können Sie separate Authentifizierungsdienste erstellen, die die Platt Form Dienste unter IOS verwenden, sowie einen generischen Webdienst für Android und andere nicht-IOS-Plattformen, die auf einer freigegebenen Schnittstelle basieren.

```csharp
public interface IAppleSignInService
{
    bool Callback(string url);

    Task<AppleAccount> SignInAsync();
}
```

Unter IOS werden die nativen APIs verwendet:

```csharp
public class AppleSignInServiceiOS : IAppleSignInService
{
#if __IOS__13
    AuthManager authManager;
#endif

    bool Is13 => UIDevice.CurrentDevice.CheckSystemVersion(13, 0);
    WebAppleSignInService webSignInService;

    public AppleSignInServiceiOS()
    {
        if (!Is13)
            webSignInService = new WebAppleSignInService();
    }

    public async Task<AppleAccount> SignInAsync()
    {
        // Fallback to web for older iOS versions
        if (!Is13)
            return await webSignInService.SignInAsync();

        AppleAccount appleAccount = default;

#if __IOS__13
        var provider = new ASAuthorizationAppleIdProvider();
        var req = provider.CreateRequest();

        authManager = new AuthManager(UIApplication.SharedApplication.KeyWindow);

        req.RequestedScopes = new[] { ASAuthorizationScope.FullName, ASAuthorizationScope.Email };
        var controller = new ASAuthorizationController(new[] { req });

        controller.Delegate = authManager;
        controller.PresentationContextProvider = authManager;

        controller.PerformRequests();

        var creds = await authManager.Credentials;

        if (creds == null)
            return null;

        appleAccount = new AppleAccount();
        appleAccount.IdToken = JwtToken.Decode(new NSString(creds.IdentityToken, NSStringEncoding.UTF8).ToString());
        appleAccount.Email = creds.Email;
        appleAccount.UserId = creds.User;
        appleAccount.Name = NSPersonNameComponentsFormatter.GetLocalizedString(creds.FullName, NSPersonNameComponentsFormatterStyle.Default, NSPersonNameComponentsFormatterOptions.Phonetic);
        appleAccount.RealUserStatus = creds.RealUserStatus.ToString();
#endif

        return appleAccount;
    }

    public bool Callback(string url) => true;
}

#if __IOS__13
class AuthManager : NSObject, IASAuthorizationControllerDelegate, IASAuthorizationControllerPresentationContextProviding
{
    public Task<ASAuthorizationAppleIdCredential> Credentials
        => tcsCredential?.Task;

    TaskCompletionSource<ASAuthorizationAppleIdCredential> tcsCredential;

    UIWindow presentingAnchor;

    public AuthManager(UIWindow presentingWindow)
    {
        tcsCredential = new TaskCompletionSource<ASAuthorizationAppleIdCredential>();
        presentingAnchor = presentingWindow;
    }

    public UIWindow GetPresentationAnchor(ASAuthorizationController controller)
        => presentingAnchor;

    [Export("authorizationController:didCompleteWithAuthorization:")]
    public void DidComplete(ASAuthorizationController controller, ASAuthorization authorization)
    {
        var creds = authorization.GetCredential<ASAuthorizationAppleIdCredential>();
        tcsCredential?.TrySetResult(creds);
    }

    [Export("authorizationController:didCompleteWithError:")]
    public void DidComplete(ASAuthorizationController controller, NSError error)
        => tcsCredential?.TrySetException(new Exception(error.LocalizedDescription));
}
#endif
```

Das Compile-Flag `__IOS__13` wird verwendet, um Unterstützung für IOS 13 und ältere Versionen bereitzustellen, die auf den generischen Webdienst zurückgreifen.

Unter Android wird der generische Webdienst mit Azure Functions verwendet:

```csharp
public class WebAppleSignInService : IAppleSignInService
{
    // IMPORTANT: This is what you register each native platform's url handler to be
    public const string CallbackUriScheme = "xamarinformsapplesignin";
    public const string InitialAuthUrl = "http://local.test:7071/api/applesignin_auth";

    string currentState;
    string currentNonce;

    TaskCompletionSource<AppleAccount> tcsAccount = null;

    public bool Callback(string url)
    {
        // Only handle the url with our callback uri scheme
        if (!url.StartsWith(CallbackUriScheme + "://"))
            return false;

        // Ensure we have a task waiting
        if (tcsAccount != null && !tcsAccount.Task.IsCompleted)
        {
            try
            {
                // Parse the account from the url the app opened with
                var account = AppleAccount.FromUrl(url);

                // IMPORTANT: Validate the nonce returned is the same as our originating request!!
                if (!account.IdToken.Nonce.Equals(currentNonce))
                    tcsAccount.TrySetException(new InvalidOperationException("Invalid or non-matching nonce returned"));

                // Set our account result
                tcsAccount.TrySetResult(account);
            }
            catch (Exception ex)
            {
                tcsAccount.TrySetException(ex);
            }
        }

        tcsAccount.TrySetResult(null);
        return false;
    }

    public async Task<AppleAccount> SignInAsync()
    {
        tcsAccount = new TaskCompletionSource<AppleAccount>();

        // Generate state and nonce which the server will use to initial the auth
        // with Apple.  The nonce should flow all the way back to us when our function
        // redirects to our app
        currentState = Util.GenerateState();
        currentNonce = Util.GenerateNonce();

        // Start the auth request on our function (which will redirect to apple)
        // inside a browser (either SFSafariViewController, Chrome Custom Tabs, or native browser)
        await Xamarin.Essentials.Browser.OpenAsync($"{InitialAuthUrl}?&state={currentState}&nonce={currentNonce}",
            Xamarin.Essentials.BrowserLaunchMode.SystemPreferred);

        return await tcsAccount.Task;
    }
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Schritte beschrieben, die zum Einrichten der Anmeldung bei Apple zur Verwendung in Ihren Anwendungen erforderlich sind Xamarin.Forms .

## <a name="related-links"></a>Verwandte Links

- [Xamarinformsapplesignin (Beispiel)](https://github.com/Redth/Xamarin.AppleSignIn.Sample)
- [Anmelden mit Apple-Richtlinien](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
