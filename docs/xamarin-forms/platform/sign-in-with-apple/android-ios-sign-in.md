---
title: Verwenden von Sign in with Apple for xamarin. Forms
description: Erfahren Sie, wie Sie die Anmeldung mit Apple in ihren mobilen xamarin. Forms-Anwendungen implementieren.
ms.prod: xamarin
ms.assetid: 2E47E7F2-93D4-4CA3-9E66-247466D25E4D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
ms.openlocfilehash: df011a6eb72b6eb30af0a197d4be48b0f2494384
ms.sourcegitcommit: fc689c1a6b641c124378dedc1bd157d96fc759a7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71319523"
---
# <a name="use-sign-in-with-apple-in-xamarinforms"></a>Verwenden Sie die Anmeldung mit Apple in xamarin. Forms.

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/signinwithapple/)

Die Anmeldung mit Apple ist für alle neuen Anwendungen auf IOS 13, die Authentifizierungsdienste von Drittanbietern verwenden. Die Implementierungsdetails zwischen IOS und Android unterscheiden sich erheblich. In dieser Anleitung wird erläutert, wie Sie dies heute in xamarin. Forms durchführen können.

In dieser Anleitung und im Beispiel werden bestimmte Platt Form Dienste verwendet, um die Anmeldung mit Apple zu behandeln:

- Android mit einem generischen Webdienst, der mit Azure Functions mit OpenID/openauth kommuniziert
- IOS verwendet die Native API für die Authentifizierung unter IOS 13 und greift auf einen generischen Webdienst für IOS 12 und niedriger zurück.

## <a name="a-sample-apple-sign-in-flow"></a>Ein Beispiel für einen Apple-Anmelde Fluss

Dieses Beispiel bietet eine Einführung in die Implementierung, um die Apple-Anmeldung in ihrer xamarin. Forms-APP zu erhalten.

Wir verwenden zwei Azure Functions, um den Authentifizierungs Ablauf zu unterstützen:

1. `applesignin_auth`-Generiert die URL für die URL-Anmelde Autorisierung und leitet Sie an Sie weiter.  Dies geschieht auf der Serverseite und nicht auf dem Mobile App, daher können wir das `state` Zwischenspeichern und überprüfen, wenn Apple-Server einen Rückruf senden.
2. `applesignin_callback`-Behandelt den Post-Rückruf von Apple und tauscht den Autorisierungs Code sicher für ein Zugriffs Token und ein ID-Token aus.  Schließlich leitet Sie zurück zum URI-Schema der APP und übergibt die Token in einem URL-Fragment.

Der Mobile App registriert sich selbst, um das benutzerdefinierte URI-Schema zu verarbeiten, das `xamarinformsapplesignin://`wir ausgewählt haben `applesignin_callback` (in diesem Fall), damit die Funktion die Token zurück an die Funktion weiterleiten kann.

Wenn der Benutzer die Authentifizierung startet, werden die folgenden Schritte ausgeführt:

1. Der Mobile App generiert einen `nonce` - `state` Wert und einen-Wert und `applesignin_auth` übergibt ihn an die Azure-Funktion.
2. Die `applesignin_auth` Azure-Funktion generiert eine URL für die URL-Anmelde Autorisierung `state` ( `nonce`unter Verwendung des bereitgestellten und) und leitet den Mobile App Browser an Sie weiter.
3. Der Benutzer gibt seine Anmelde Informationen sicher auf der Seite "Apple Sign in Authorization" ein, die auf den Apple-Servern gehostet wird.
4. Nachdem der Apple-Anmelde Ablauf auf den Servern von Apple abgeschlossen ist, leitet Apple an `redirect_uri` die weiter, die `applesignin_callback` die Azure-Funktion sein wird.
5. Die Anforderung von Apple, die an `applesignin_callback` die Funktion gesendet wird, wird überprüft `state` , um sicherzustellen, dass die korrekte Rückgabe erfolgt und dass die ID-Tokenansprüche gültig sind.
6. Die `applesignin_callback` Azure-Funktion tauscht `code` den, der von Apple gepostet wird, für ein _Zugriffs Token_, ein _Aktualisierungs Token_und ein _ID-Token_ aus, das Ansprüche über die Benutzer-ID, den Namen und die e-Mail-Adresse enthält.
7. Die `applesignin_callback` Azure-Funktion führt schließlich eine Umleitung zurück zum URI-Schema der`xamarinformsapplesignin://`app (), das ein URI-Fragment mit den Token `xamarinformsapplesignin://#access_token=...&refresh_token=...&id_token=...`anfügt (z. b.).
8. Die Mobile App analysiert das URI-Fragment in ein `AppleAccount` und überprüft, dass der `nonce` empfangene Anspruch mit dem `nonce` zu Beginn des Flows generierten übereinstimmt.
9. Der Mobile App ist jetzt authentifiziert!

## <a name="azure-functions"></a>Überprüfung auf

In diesem Beispiel wird Azure Functions verwendet. Alternativ kann ein ASP.net Core Controller oder eine ähnliche Webserver Lösung die gleiche Funktionalität bereitzustellen.

### <a name="configuration"></a>Konfiguration

Bei der Verwendung Azure Functions müssen mehrere App-Einstellungen konfiguriert werden:

- `APPLE_SIGNIN_KEY_ID`-Dies ist der `KeyId` von früher.
- `APPLE_SIGNIN_TEAM_ID`-Dies ist normalerweise Ihre _Team-ID_ in Ihrem [Mitgliedschafts Profil](https://developer.apple.com/account/#/membership) .
- `APPLE_SIGNIN_SERVER_ID`: Dies ist der `ServerId` von früher.  Dabei handelt es sich *nicht* um die APP- _Bündel-ID_, sondern vielmehr um den *Bezeichner* der von Ihnen erstellten *Dienst-ID* .
- `APPLE_SIGNIN_APP_CALLBACK_URI`: Dies ist das benutzerdefinierte URI-Schema, das Sie mit an Ihre APP zurückleiten möchten.  In diesem Beispiel `xamarinformsapplesignin://` wird verwendet.
- `APPLE_SIGNIN_REDIRECT_URI`: Die *Umleitungs-URL* , die Sie beim Erstellen Ihrer *Dienst-ID* im Abschnitt *Apple Sign in* Configuration einrichten.  Zum Testen könnte es etwa wie folgt aussehen:`http://local.test:7071/api/applesignin_callback`
- `APPLE_SIGNIN_P8_KEY`: Der Text Inhalt der `.p8` Datei, wobei `\n` alle Zeilenumbrüche entfernt werden, sodass es sich um eine lange Zeichenfolge handelt.

### <a name="security-considerations"></a>Sicherheitsüberlegungen

Speichern Sie Ihren P8-Schlüssel **niemals** innerhalb Ihres Anwendungs Codes. Der Anwendungscode kann einfach heruntergeladen und disassembliert werden. 

Es wird auch als bewährte Vorgehensweise verwendet `WebView` , um den Authentifizierungs Fluss zu hosten und URL-Navigations Ereignisse abzufangen, um den Autorisierungs Code zu erhalten. Zurzeit gibt es zurzeit keine vollständig sichere Methode, um die Anmeldung bei Apple auf nicht-iOS13 +-Geräten durchzuführen, ohne Code auf einem Server zu verwalten, um den tokenaustausch zu verarbeiten. Es wird empfohlen, den Autorisierungs-URL-Generierungs Code auf einem Server zu speichern, damit Sie den Status Zwischenspeichern und validieren können, wenn Apple einen Post-Rückruf an Ihren Server ausgibt

## <a name="a-cross-platform-sign-in-service"></a>Ein plattformübergreifender Anmeldedienst

Mithilfe des "xamarin. Forms dependencyservice" können Sie separate Authentifizierungsdienste erstellen, die die Platt Form Dienste unter IOS verwenden, und einen generischen Webdienst für Android und andere nicht-IOS-Plattformen basierend auf einer freigegebenen Schnittstelle.

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

Das Compile- `__IOS__13` Flag wird verwendet, um Unterstützung für IOS 13 und ältere Versionen bereitzustellen, die auf den generischen Webdienst zurückgreifen.

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

In diesem Artikel wurden die Schritte beschrieben, die zum Einrichten der Anmeldung bei Apple zur Verwendung in ihren xamarin. Forms-Anwendungen erforderlich sind.

## <a name="related-links"></a>Verwandte Links

- [Xamarinformsapplesignin (Beispiel)](https://github.com/Redth/Xamarin.AppleSignIn.Sample)
- [Anmelden mit Apple-Richtlinien](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
