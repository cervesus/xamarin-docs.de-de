---
title: Anmelden mit Apple in xamarin. IOS
description: Bei der Anmeldung mit Apple wird Identitätsschutz bereitstellt, wenn Authentifizierungsdienste von Drittanbietern verwendet werden.
ms.prod: xamarin
ms.assetid: CDA59BBF-AAE1-4D4F-B4AE-DB37220D41EB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
ms.openlocfilehash: 5c5191a6a7490ec0301bdea7b7f5aa2217b80c96
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71206364"
---
# <a name="sign-in-with-apple-in-xamarinios"></a>Anmelden mit Apple in xamarin. IOS

Die Anmeldung mit Apple ist ein neuer Dienst, der Identitätsschutz für Benutzer von Authentifizierungsdiensten von Drittanbietern bereitstellt. Ab IOS 13 erfordert Apple, dass jede neue APP, die Authentifizierungsdienste von Drittanbietern verwendet, auch die Anmeldung bei Apple bereitstellen sollte. Vorhandene apps, die aktualisiert werden, müssen sich bis zum 2020 nicht mehr mit Apple anmelden.

In diesem Dokument wird erläutert, wie Sie die Anmeldung mit Apple zu IOS 13-Anwendungen hinzufügen können.

## <a name="apple-developer-setup"></a>Apple-Entwickler-Setup

Bevor Sie eine APP mit der Anmeldung bei Apple entwickeln und ausführen, müssen Sie diese Schritte ausführen. In [Apple Developer-Zertifikaten, Bezeichner & profile][5] Portal:

1. Erstellen Sie einen neuen **App-IDs** -Bezeichner.
2. Legen Sie im Feld **Beschreibung** eine Beschreibung fest.
3. Wählen Sie eine **explizite** Bündel-ID `com.xamarin.AddingTheSignInWithAppleFlowToYourApp` aus, und legen Sie Sie im Feld fest
4. Aktivieren Sie **Anmelden mit der Apple** -Funktion, und registrieren Sie die neue Identität.
5. Erstellen Sie ein neues Bereitstellungs Profil mit der neuen Identität.
6. Herunterladen und Installieren des Geräts auf Ihrem Gerät.
7. Aktivieren Sie in Visual Studio die Funktion "bei **Apple anmelden** " in der Datei " **Berechtigungen. plist** ".

## <a name="check-sign-in-status"></a>Anmeldestatus überprüfen

Wenn Ihre APP beginnt oder Sie zuerst den Authentifizierungs Status eines Benutzers überprüfen müssen, instanziieren Sie einen `ASAuthorizationAppleIdProvider` , und überprüfen Sie den aktuellen Status:

```csharp
var appleIdProvider = new ASAuthorizationAppleIdProvider ();
appleIdProvider.GetCredentialState (KeychainItem.CurrentUserIdentifier, (credentialState, error) => {
    switch (credentialState) {
    case ASAuthorizationAppleIdProviderCredentialState.Authorized:
        // The Apple ID credential is valid.
        break;
    case ASAuthorizationAppleIdProviderCredentialState.Revoked:
        // The Apple ID credential is revoked.
        break;
    case ASAuthorizationAppleIdProviderCredentialState.NotFound:
        // No credential was found, so show the sign-in UI.
        InvokeOnMainThread (() => {
            var storyboard = UIStoryboard.FromName ("Main", null);

            if (!(storyboard.InstantiateViewController (nameof (LoginViewController)) is LoginViewController viewController))
                return;

            viewController.ModalPresentationStyle = UIModalPresentationStyle.FormSheet;
            viewController.ModalInPresentation = true;
            Window?.RootViewController?.PresentViewController (viewController, true, null);
        });
        break;
    }
});
```

In diesem Code, der während `FinishedLaunching` `AppDelegate.cs`im aufgerufen wird, verarbeitet die APP, wenn ein Zustand `NotFound` ist, und `LoginViewController` stellt dem Benutzer die zur Verfügung. Wenn der Zustand den Wert `Authorized` oder `Revoked`zurückgibt, kann dem Benutzer eine andere Aktion angezeigt werden.

## <a name="a-loginviewcontroller-for-sign-in-with-apple"></a>Ein loginviewcontroller für die Anmeldung bei Apple

Der `UIViewController` , der die Anmelde Logik implementiert und die Anmeldung mit Apple ermöglicht, `IASAuthorizationControllerDelegate` muss `IASAuthorizationControllerPresentationContextProviding` und wie im `LoginViewController` folgenden Beispiel implementieren.

```csharp
public partial class LoginViewController : UIViewController, IASAuthorizationControllerDelegate, IASAuthorizationControllerPresentationContextProviding {
    public LoginViewController (IntPtr handle) : base (handle)
    {
    }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.

        SetupProviderLoginView ();
    }

    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        PerformExistingAccountSetupFlows ();
    }

    void SetupProviderLoginView ()
    {
        var authorizationButton = new ASAuthorizationAppleIdButton (ASAuthorizationAppleIdButtonType.Default, ASAuthorizationAppleIdButtonStyle.White);
        authorizationButton.TouchUpInside += HandleAuthorizationAppleIDButtonPress;
        loginProviderStackView.AddArrangedSubview (authorizationButton);
    }

    // Prompts the user if an existing iCloud Keychain credential or Apple ID credential is found.
    void PerformExistingAccountSetupFlows ()
    {
        // Prepare requests for both Apple ID and password providers.
        ASAuthorizationRequest [] requests = {
            new ASAuthorizationAppleIdProvider ().CreateRequest (),
            new ASAuthorizationPasswordProvider ().CreateRequest ()
        };

        // Create an authorization controller with the given requests.
        var authorizationController = new ASAuthorizationController (requests);
        authorizationController.Delegate = this;
        authorizationController.PresentationContextProvider = this;
        authorizationController.PerformRequests ();
    }

    private void HandleAuthorizationAppleIDButtonPress (object sender, EventArgs e)
    {
        var appleIdProvider = new ASAuthorizationAppleIdProvider ();
        var request = appleIdProvider.CreateRequest ();
        request.RequestedScopes = new [] { ASAuthorizationScope.Email, ASAuthorizationScope.FullName };

        var authorizationController = new ASAuthorizationController (new [] { request });
        authorizationController.Delegate = this;
        authorizationController.PresentationContextProvider = this;
        authorizationController.PerformRequests ();
    }
}
```

![Animation der Beispiel-App mit der Anmeldung bei Apple](sign-in-images/sign-in-flow.png)

Dieser Beispielcode überprüft den aktuellen Anmeldestatus in `PerformExistingAccountSetupFlows` und stellt eine Verbindung mit der aktuellen Ansicht als Delegat her. Wenn eine vorhandene icloud-Keychain-Anmelde Information oder Anmelde Informationen für die Apple-ID gefunden wird, wird der Benutzer dazu aufgefordert, diese zu verwenden.

Apple stellt `ASAuthorizationAppleIdButton`eine Schaltfläche speziell für diesen Zweck bereit. Wenn Sie berührt wird, löst die Schaltfläche den in der- `HandleAuthorizationAppleIDButtonPress`Methode behandelten Workflow aus.

## <a name="handling-authorization"></a>Verwalten der Autorisierung

`IASAuthorizationController` In implementieren Sie benutzerdefinierte Logik zum Speichern des Benutzerkontos. Im folgenden Beispiel wird das Konto des Benutzers in keychain, dem eigenen Speicherdienst von Apple, gespeichert.

```csharp
#region IASAuthorizationController Delegate

[Export ("authorizationController:didCompleteWithAuthorization:")]
public void DidComplete (ASAuthorizationController controller, ASAuthorization authorization)
{
    if (authorization.GetCredential<ASAuthorizationAppleIdCredential> () is ASAuthorizationAppleIdCredential appleIdCredential) {
        var userIdentifier = appleIdCredential.User;
        var fullName = appleIdCredential.FullName;
        var email = appleIdCredential.Email;

        // Create an account in your system.
        // For the purpose of this demo app, store the userIdentifier in the keychain.
        try {
            new KeychainItem ("com.example.apple-samplecode.juice", "userIdentifier").SaveItem (userIdentifier);
        } catch (Exception) {
            Console.WriteLine ("Unable to save userIdentifier to keychain.");
        }

        // For the purpose of this demo app, show the Apple ID credential information in the ResultViewController.
        if (!(PresentingViewController is ResultViewController viewController))
            return;

        InvokeOnMainThread (() => {
            viewController.UserIdentifierText = userIdentifier;
            viewController.GivenNameText = fullName?.GivenName ?? "";
            viewController.FamilyNameText = fullName?.FamilyName ?? "";
            viewController.EmailText = email ?? "";

            DismissViewController (true, null);
        });
    } else if (authorization.GetCredential<ASPasswordCredential> () is ASPasswordCredential passwordCredential) {
        // Sign in using an existing iCloud Keychain credential.
        var username = passwordCredential.User;
        var password = passwordCredential.Password;

        // For the purpose of this demo app, show the password credential as an alert.
        InvokeOnMainThread (() => {
            var message = $"The app has received your selected credential from the keychain. \n\n Username: {username}\n Password: {password}";
            var alertController = UIAlertController.Create ("Keychain Credential Received", message, UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("Dismiss", UIAlertActionStyle.Cancel, null));

            PresentViewController (alertController, true, null);
        });
    }
}

[Export ("authorizationController:didCompleteWithError:")]
public void DidComplete (ASAuthorizationController controller, NSError error)
{
    Console.WriteLine (error);
}

#endregion
```

## <a name="authorization-controller"></a>Autorisierungs Controller

Der letzte Teil dieser Implementierung ist der `ASAuthorizationController` , der Autorisierungs Anforderungen für den Anbieter verwaltet.

```csharp
#region IASAuthorizationControllerPresentation Context Providing

public UIWindow GetPresentationAnchor (ASAuthorizationController controller) => View.Window;

#endregion
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Anmeldung mit Apple für IOS beschrieben.

## <a name="related-links"></a>Verwandte Links

* [Anmelden mit Apple-Richtlinien](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
* [Melden Sie sich mit der Apple-Berechtigung an.][2]
* [WWDC 2019 Sitzung 706: Die Anmeldung mit Apple wird eingeführt.][3]
* [Einrichten der Anmeldung mit Apple für xamarin. Forms][4]

[1]: https://developer.apple.com/documentation/authenticationservices/adding_the_sign_in_with_apple_flow_to_your_app
[2]: https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_applesignin
[3]: https://developer.apple.com/videos/play/wwdc19/706/
[4]: ~/xamarin-forms/platform/sign-in-with-apple/setup.md
[5]: https://developer.apple.com/account/resources/identifiers/list
