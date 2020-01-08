---
title: Fingereingabe-ID und Face ID mit xamarin. IOS
description: Dieses Dokument enthält eine allgemeine Beschreibung der biometrischen Authentifizierung in ios.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: profexorgeek
ms.author: jusjohns
ms.date: 12/16/2019
ms.openlocfilehash: 744a07343b9da87f196c0664f57b7d950844ab04
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490427"
---
# <a name="use-touch-id-and-face-id-with-xamarinios"></a>Verwenden von Fingereingabe-ID und Face ID mit xamarin. IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-faceidsample/)

IOS unterstützt zwei biometrische Authentifizierungssysteme:

1. **Touchid** verwendet einen Fingerabdrucksensor unter der Start Schaltfläche.
1. Die **Gesichts-ID** verwendet Front-seitige Kamerasensoren, um Benutzer mit einem Gesichts Scan zu authentifizieren.

Die Fingereingabe-ID wurde in ios 7 mit der Face ID in ios 11 eingeführt.

Diese Authentifizierungssysteme basieren auf einem hardwarebasierten Sicherheits Prozessor, der als " _Secure Enclave_" bezeichnet wird. Die sichere Enclave ist für das Verschlüsseln mathematischer Darstellungen von Gesichts-und Fingerabdruckdaten und das Authentifizieren von Benutzern mit diesen Informationen verantwortlich. Laut Apple bleiben Gesichts-und Fingerabdruckdaten nicht auf dem Gerät und werden nicht in der icloud gesichert. Apps interagieren mit der sicheren Enclave über die _lokale Authentifizierungs_ -API und können keine Gesichts-oder Fingerabdruckdaten abrufen oder direkt auf die sichere Enclave zugreifen.

Die Fingereingabe-ID und die Face ID können von apps verwendet werden, um einen Benutzer zu authentifizieren, bevor er Zugriff auf geschützte Inhalte bietet.

## <a name="local-authentication-context"></a>Lokaler Authentifizierungs Kontext

Die biometrische Authentifizierung unter IOS basiert auf einem _lokalen Authentifizierungs Kontext_ Objekt, bei dem es sich um eine Instanz der `LAContext`-Klasse handelt. Die `LAContext`-Klasse ermöglicht Folgendes:

- Überprüfen Sie die Verfügbarkeit biometrischer Hardware.
- Authentifizierungs Richtlinien auswerten.
- Zugriffs Steuerungen auswerten.
- Anpassen und Anzeigen von Authentifizierungs Aufforderungen.
- Wieder verwenden oder ungültig machen eines Authentifizierungs Zustands.
- Anmelde Informationen verwalten.

## <a name="detect-available-authentication-methods"></a>Verfügbare Authentifizierungsmethoden erkennen

Das Beispiel Projekt enthält eine `AuthenticationView`, die von einem `AuthenticationViewController`unterstützt wird. Diese Klasse überschreibt die `ViewWillAppear`-Methode, um verfügbare Authentifizierungsmethoden zu ermitteln:

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    public override void ViewWillAppear(bool animated)
    {
        base.ViewWillAppear(animated);
        unAuthenticatedLabel.Text = "";
    
        var context = new LAContext();
        var buttonText = "";

        // Is login with biometrics possible?
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out var authError1))
        {
            // has Touch ID or Face ID
            if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
            {
                context.LocalizedReason = "Authorize for access to secrets"; // iOS 11
                BiometryType = context.BiometryType == LABiometryType.TouchId ? "Touch ID" : "Face ID";
                buttonText = $"Login with {BiometryType}";
            }
            // No FaceID before iOS 11
            else
            {
                buttonText = $"Login with Touch ID";
            }
        }

        // Is pin login possible?
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out var authError2))
        {
            buttonText = $"Login"; // with device PIN
            BiometryType = "Device PIN";
        }

        // Local authentication not possible
        else
        {
            // Application might choose to implement a custom username/password
            buttonText = "Use unsecured";
            BiometryType = "none";
        }
        AuthenticateButton.SetTitle(buttonText, UIControlState.Normal);
    }
}
```

Die `ViewWillAppear`-Methode wird aufgerufen, wenn die Benutzeroberfläche dem Benutzer angezeigt wird. Diese Methode definiert eine neue Instanz von `LAContext` und verwendet die `CanEvaluatePolicy`-Methode, um zu bestimmen, ob die biometrische Authentifizierung aktiviert ist. Wenn dies der Fall ist, wird die System Version überprüft und `BiometryType` Enumeration festgelegt, welche biometrischen Optionen verfügbar sind.

Wenn die biometrische Authentifizierung nicht aktiviert ist, versucht die APP, auf die PIN-Authentifizierung zurückzugreifen. Wenn weder die biometrische noch die PIN-Authentifizierung verfügbar ist, hat der Besitzer des Geräts Sicherheitsfeatures nicht aktiviert, und Inhalte können nicht über die lokale Authentifizierung gesichert werden.

## <a name="authenticate-a-user"></a>Authentifizieren eines Benutzers

Die `AuthenticationViewController` im Beispiel Projekt enthält eine `AuthenticateMe`-Methode, die für die Authentifizierung des Benutzers zuständig ist:

```csharp
partial class AuthenticationViewController: UIViewController
{
    // ...
    string BiometryType = "";

    partial void AuthenticateMe(UIButton sender)
    {
        var context = new LAContext();
        NSError AuthError;
        var localizedReason = new NSString("To access secrets");
    
        // Because LocalAuthentication APIs have been extended over time,
        // you must check iOS version before setting some properties
        context.LocalizedFallbackTitle = "Fallback";
    
        if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
        {
            context.LocalizedCancelTitle = "Cancel";
        }
        if (UIDevice.CurrentDevice.CheckSystemVersion(11, 0))
        {
            context.LocalizedReason = "Authorize for access to secrets";
            BiometryType = context.BiometryType == LABiometryType.TouchId ? "TouchID" : "FaceID";
        }
    
        // Check if biometric authentication is possible
        if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = $"{BiometryType} Authentication Failed";
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, localizedReason, replyHandler);
        }

        // Fall back to PIN authentication
        else if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, out AuthError))
        {
            replyHandler = new LAContextReplyHandler((success, error) =>
            {
                // This affects UI and must be run on the main thread
                this.InvokeOnMainThread(() =>
                {
                    if (success)
                    {
                        PerformSegue("AuthenticationSegue", this);
                    }
                    else
                    {
                        unAuthenticatedLabel.Text = "Device PIN Authentication Failed";
                        AuthenticateButton.Hidden = true;
                    }
                });
    
            });
            context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthentication, localizedReason, replyHandler);
        }

        // User hasn't configured any authentication: show dialog with options
        else
        {
            unAuthenticatedLabel.Text = "No device auth configured";
            var okCancelAlertController = UIAlertController.Create("No authentication", "This device does't have authentication configured.", UIAlertControllerStyle.Alert);
            okCancelAlertController.AddAction(UIAlertAction.Create("Use unsecured", UIAlertActionStyle.Default, alert => PerformSegue("AuthenticationSegue", this)));
            okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine("Cancel was clicked")));
            PresentViewController(okCancelAlertController, true, null);
        }
    } 
}
```

Die `AuthenticateMe`-Methode wird aufgerufen, wenn der Benutzer auf eine **Anmelde** Schaltfläche tippt. Ein neues `LAContext` Objekt wird instanziiert, und die Geräteversion wird geprüft, um zu bestimmen, welche Eigenschaften im lokalen Authentifizierungs Kontext festgelegt werden sollen.

Die `CanEvaluatePolicy`-Methode wird aufgerufen, um zu überprüfen, ob die biometrische Authentifizierung aktiviert ist. greifen Sie nach Möglichkeit auf PIN-Authentifizierung zurück, und stellen Sie schließlich einen ungesicherten Modus bereit, wenn keine Authentifizierung verfügbar ist Wenn eine Authentifizierungsmethode verfügbar ist, wird die `EvaluatePolicy`-Methode verwendet, um die Benutzeroberfläche anzuzeigen und den Authentifizierungsprozess abzuschließen.

Das Beispiel Projekt enthält mockdaten und eine Ansicht, um die Daten anzuzeigen, wenn die Authentifizierung erfolgreich ist.

## <a name="related-links"></a>Verwandte Links

- [Beispiel für lokale Authentifizierung mit Fingereingabe-ID oder Gesichts-ID](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-faceidsample/)
- [Informationen über die Berührungs-ID](https://support.apple.com/en-us/HT204587) auf Support.Apple.com
- [Informationen über die Gesichts-ID](https://support.apple.com/en-us/HT208108) auf Support.Apple.com
