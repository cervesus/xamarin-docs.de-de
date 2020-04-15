---
title: Reagieren auf Authentifizierungsrückrufe
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: 8dc06740355bd95828e1a1bd8d9d15a2ef37e6b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027537"
---
# <a name="responding-to-authentication-callbacks"></a>Reagieren auf Authentifizierungsrückrufe

Der Fingerabdruckscanner wird im Hintergrund in einem eigenen Thread ausgeführt. Nach Abschluss des Vorgangs werden die Ergebnisse der Überprüfung gemeldet, indem eine Methode von `FingerprintManager.AuthenticationCallback` im UI-Thread aufgerufen wird. Eine Android-Anwendung muss einen eigenen Handler bereitstellen, der diese abstrakte Klasse erweitert und dabei alle folgenden Methoden implementiert:

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Wird aufgerufen, wenn ein nicht behebbarer Fehler vorliegt. Außer möglicherweise durch einen erneuten Versuch kann die Situation von der Anwendung oder dem Benutzer nicht korrigiert werden.
- **`OnAuthenticationFailed()`** &ndash; Diese Methode wird aufgerufen, wenn ein Fingerabdruck erkannt, aber vom Gerät nicht zugeordnet werden kann.
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Wird aufgerufen, wenn ein behebbarer Fehler vorliegt, zum Beispiel, wenn der Finger zu schnell über den Scanner gleitet.
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; Wird aufgerufen, wenn ein Fingerabdruck erkannt wurde.

Wenn beim Aufrufen von `Authenticate` ein `CryptoObject`-Element verwendet wurde, wird empfohlen, `Cipher.DoFinal` in `OnAuthenticationSuccessful` aufzurufen.
`DoFinal` löst eine Ausnahme aus, wenn die Verschlüsselung geändert oder fehlerhaft initialisiert wurde, was darauf hinweist, dass das Ergebnis des Fingerabdruckscanners möglicherweise außerhalb der Anwendung manipuliert wurde.

> [!NOTE]
> Es wird empfohlen, die Rückrufklasse relativ einfach und frei von anwendungsspezifischer Logik zu halten. Die Rückrufe sollten als „Kontrollinstanz“ zwischen der Android-Anwendung und den Ergebnissen des Fingerabdruckscanners fungieren.

## <a name="a-sample-authentication-callback-handler"></a>Ein Beispiel für einen Authentifizierungsrückrufhandler

Die folgende Klasse ist ein Beispiel für eine minimale `FingerprintManager.AuthenticationCallback`-Implementierung: 

```csharp
class MyAuthCallbackSample : FingerprintManagerCompat.AuthenticationCallback
{
    // Can be any byte array, keep unique to application.
    static readonly byte[] SECRET_BYTES = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // The TAG can be any string, this one is for demonstration.
    static readonly string TAG = "X:" + typeof (SimpleAuthCallbacks).Name;

    public MyAuthCallbackSample()
    {
    }

    public override void OnAuthenticationSucceeded(FingerprintManagerCompat.AuthenticationResult result)
    {
        if (result.CryptoObject.Cipher != null) 
        {
            try
            {
                // Calling DoFinal on the Cipher ensures that the encryption worked.
                byte[] doFinalResult = result.CryptoObject.Cipher.DoFinal(SECRET_BYTES);
    
                // No errors occurred, trust the results.              
            }
            catch (BadPaddingException bpe)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + bpe);
            }
            catch (IllegalBlockSizeException ibse)
            {
                // Can't really trust the results.
                Log.Error(TAG, "Failed to encrypt the data with the generated key." + ibse);
            }
        }
        else
        {
            // No cipher used, assume that everything went well and trust the results.
        }
    }

    public override void OnAuthenticationError(int errMsgId, ICharSequence errString)
    {
        // Report the error to the user. Note that if the user canceled the scan,
        // this method will be called and the errMsgId will be FingerprintState.ErrorCanceled.
    }

    public override void OnAuthenticationFailed()
    {
        // Tell the user that the fingerprint was not recognized.
    }

    public override void OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)
    {
        // Notify the user that the scan failed and display the provided hint.
    }
}
```

`OnAuthenticationSucceeded` prüft, ob beim Aufruf von `Authentication` `Cipher` für `FingerprintManager` bereitgestellt wurde. Ist dies der Fall, wird die `DoFinal`-Methode für die Verschlüsselung aufgerufen. Damit wird `Cipher` geschlossen und der ursprüngliche Zustand wiederhergestellt. Wenn es ein Problem mit der Verschlüsselung gab, dann löst `DoFinal` eine Ausnahme aus, und der Authentifizierungsversuch gilt als gescheitert.

Die Rückrufe `OnAuthenticationError` und `OnAuthenticationHelp` erhalten jeweils eine ganze Zahl, die auf die Ursache des Problems hinweist. Im folgenden Abschnitt werden die einzelnen Hilfs- bzw. Fehlercodes erläutert. Der Zweck der beiden Rückrufe ist ähnlich: Sie sollen die Anwendung darüber informieren, dass die Fingerabdruckauthentifizierung fehlgeschlagen ist. Sie unterscheiden sich im Schweregrad des Problems. `OnAuthenticationHelp` ist ein vom Benutzer behebbarer Fehler, wie z.B. ein zu schnelles Gleiten des Fingers über den Sensor; `OnAuthenticationError` ist ein schwerwiegenderer Fehler, wie z.B. ein beschädigter Fingerabdruckscanner.

Beachten Sie, dass `OnAuthenticationError` aufgerufen wird, wenn die Überprüfung des Fingerabdrucks mit der Meldung `CancellationSignal.Cancel()` abgebrochen wird. Der Parameter `errMsgId` hat dann den Wert 5 (`FingerprintState.ErrorCanceled`). Je nach den Anforderungen wird durch die Implementierung von `AuthenticationCallbacks` diese Situation anders behandelt als die anderen Fehler. 

`OnAuthenticationFailed` wird aufgerufen, wenn der Fingerabdruck erfolgreich gescannt wurde, aber nicht mit einem im Gerät registrierten Fingerabdruck übereinstimmt. 

## <a name="help-codes-and-error-message-ids"></a>Hilfscodes und Fehlermeldungs-IDs 

Eine Liste und Beschreibung der Fehler- und Hilfscodes finden Sie in der [Dokumentation zum Android SDK](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) für die FingerprintManager-Klasse. Xamarin.Android stellt diese Werte mit der Enumeration `Android.Hardware.Fingerprints.FingerprintState` dar:

- **`AcquiredGood`** &ndash; (Wert 0) Das abgerufene Bild war gut.

- **`AcquiredImagerDirty`** &ndash; (Wert 3) Das Fingerabdruckbild war aufgrund von vermuteter oder festgestellter Verschmutzung des Sensors zu verrauscht. Es ist z.B. sinnvoll, dieses nach mehrfacher `AcquiredInsufficient`-Erfassung oder tatsächlicher Erkennung einer Verschmutzung des Sensors (festsitzende Partikel, Schlieren usw.) zurückzugeben. Der Benutzer muss Maßnahmen zur Reinigung des Sensors ergreifen, wenn dieser Wert zurückgegeben wird.

- **`AcquiredInsufficient`** &ndash; (Wert 2) Das Fingerabdruckbild war aufgrund eines erkannten Zustands (d.h. trockener Haut) oder eines möglicherweise verschmutzten Sensors zu verrauscht, um es zu verarbeiten (siehe `AcquiredImagerDirty`).

- **`AcquiredPartial`** &ndash; (Wert 1) Es wurde nur ein Teil des Fingerabdruckbilds erkannt. Während der Registrierung sollte der Benutzer darüber informiert werden, wie dieses Problem zu lösen ist, z.B. &ldquo;Drücken Sie fest auf den Sensor&rdquo;.

- **`AcquiredTooFast`** &ndash; (Wert 5) Das Fingerabdruckbild war aufgrund einer zu schnellen Bewegung unvollständig. Dies trifft zwar hauptsächlich auf lineare Arraysensoren zu, könnte aber auch auftreten, wenn der Finger während der Erfassung bewegt wird. Der Benutzer sollte aufgefordert werden, den Finger langsamer (linear) zu bewegen oder den Finger länger auf dem Sensor zu lassen.

- **`AcquiredToSlow`** &ndash; (Wert 4) Das Fingerabdruckbild konnte aufgrund von mangelnder Bewegung nicht gelesen werden. Dies trifft vor allem auf lineare Arraysensoren zu, die eine Wischbewegung erfordern.

- **`ErrorCanceled`** &ndash; (Wert 5) Der Vorgang wurde abgebrochen, weil der Fingerabdrucksensor nicht verfügbar ist. Dies kann z.B. der Fall sein, wenn der Benutzer wechselt, das Gerät gesperrt ist oder ein anderer ausstehender Vorgang das Scannen verhindert oder deaktiviert.

- **`ErrorHwUnavailable`** &ndash; (Wert 1) Die Hardware ist nicht verfügbar. Versuchen Sie es später noch einmal.

- **`ErrorLockout`** &ndash; (Wert 7) Der Vorgang wurde abgebrochen, da die API aufgrund zu vieler Versuche gesperrt ist.

- **`ErrorNoSpace`** &ndash; (Wert 4) Fehlerstatus, der für Vorgänge wie die Registrierung zurückgegeben wird; der Vorgang kann nicht abgeschlossen werden, da nicht genügend Speicherplatz für den Abschluss des Vorgangs zur Verfügung steht.

- **`ErrorTimeout`** &ndash; (Wert 3) Fehlerstatus, der zurückgegeben wird, wenn die aktuelle Anforderung zu lange läuft. Damit soll verhindert werden, dass Programme unbegrenzt auf den Fingerabdrucksensor warten. Die Zeitüberschreitung ist plattform- und sensorspezifisch, beträgt aber in der Regel etwa 30 Sekunden.

- **`ErrorUnableToProcess`** &ndash; (Wert 2) Fehlerstatus, der zurückgegeben wird, wenn der Sensor das aktuelle Bild nicht verarbeiten konnte.

## <a name="related-links"></a>Verwandte Links

- [Verschlüsselung](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
