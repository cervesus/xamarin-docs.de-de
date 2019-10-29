---
title: Reagieren auf Authentifizierungsrückrufe
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/06/2017
ms.openlocfilehash: 8dc06740355bd95828e1a1bd8d9d15a2ef37e6b2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027537"
---
# <a name="responding-to-authentication-callbacks"></a>Reagieren auf Authentifizierungsrückrufe

Der Fingerabdruckscanner wird im Hintergrund in seinem eigenen Thread ausgeführt. nach Abschluss des Vorgangs werden die Ergebnisse der Überprüfung gemeldet, indem eine Methode der `FingerprintManager.AuthenticationCallback` im UI-Thread aufgerufen wird. Eine Android-Anwendung muss einen eigenen Handler bereitstellen, der diese abstrakte Klasse erweitert und dabei alle folgenden Methoden implementiert:

- **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; aufgerufen, wenn ein nicht BEHEB barer Fehler vorliegt. Es gibt nichts anderes, was eine Anwendung oder ein Benutzer tun kann, um die Situation zu beheben, außer möglicherweise wiederholen.
- **`OnAuthenticationFailed()`** &ndash; diese Methode aufgerufen wird, wenn ein Fingerabdruck erkannt, aber vom Gerät nicht erkannt wird.
- **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; aufgerufen, wenn ein BEHEB barer Fehler vorliegt, z. b. der Finger, der über dem Scanner zum schnellen überspringen geleitet wird.
- **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; wird dies aufgerufen, wenn ein Fingerabdruck erkannt wurde.

Wenn beim Aufrufen von `Authenticate`ein `CryptoObject` verwendet wurde, wird empfohlen, `Cipher.DoFinal` in `OnAuthenticationSuccessful`aufzurufen.
`DoFinal` löst eine Ausnahme aus, wenn die Verschlüsselung manipuliert oder nicht ordnungsgemäß initialisiert wurde. Dies deutet darauf hin, dass das Ergebnis des Fingerabdruckscanners außerhalb der Anwendung manipuliert wurde.

> [!NOTE]
> Es wird empfohlen, die Rückruf Klasse relativ gering zu halten und die anwendungsspezifische Logik freizugeben. Die Rückrufe sollten als "Verkehrs Cop" zwischen der Android-Anwendung und den Ergebnissen des Fingerabdruckscanners fungieren.

## <a name="a-sample-authentication-callback-handler"></a>Ein Beispiel für einen Authentifizierungs Rückruf Handler

Die folgende Klasse ist ein Beispiel für eine minimale `FingerprintManager.AuthenticationCallback` Implementierung: 

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

`OnAuthenticationSucceeded` überprüft, ob ein `Cipher` für `FingerprintManager` bereitgestellt wurde, als `Authentication` aufgerufen wurde. Wenn dies der Fall ist, wird die `DoFinal`-Methode für die Chiffre aufgerufen. Dadurch wird der `Cipher`geschlossen, und der ursprüngliche Zustand wird wieder hergestellt. Wenn ein Problem mit der Verschlüsselung aufgetreten ist, löst `DoFinal` eine Ausnahme aus, und der Authentifizierungs Versuch sollte als fehlgeschlagen angesehen werden.

Die `OnAuthenticationError` und `OnAuthenticationHelp` Rückrufe erhalten jeweils eine ganze Zahl, die angibt, worum es sich bei dem Problem handelt. Im folgenden Abschnitt werden die einzelnen möglichen Hilfe-oder Fehlercodes erläutert. Die beiden Rückrufe dienen ähnlichen Zwecken &ndash;, um die Anwendung darüber zu informieren, dass die Fingerabdruckauthentifizierung fehlgeschlagen ist. Unterschiede zwischen den unterschieden. `OnAuthenticationHelp` ist ein Benutzer BEHEB barer Fehler, wie z. b. ein zu schnelles Schwenken des Fingerabdrucks. `OnAuthenticationError` ist ein schwerer Fehler, z. b. ein beschädigter Fingerabdruckscanner.

Beachten Sie, dass `OnAuthenticationError` aufgerufen wird, wenn die Fingerabdruck Überprüfung über die `CancellationSignal.Cancel()` Meldung abgebrochen wird. Der `errMsgId`-Parameter hat den Wert 5 (`FingerprintState.ErrorCanceled`). Abhängig von den Anforderungen kann eine Implementierung der `AuthenticationCallbacks` diese Situation anders behandeln als die anderen Fehler. 

`OnAuthenticationFailed` wird aufgerufen, wenn der Fingerabdruck erfolgreich gescannt wurde, aber keinem mit dem Gerät registrierten Fingerabdruck entsprach. 

## <a name="help-codes-and-error-message-ids"></a>Hilfe Codes und Fehlermeldungs-IDs 

Eine Liste und Beschreibung der Fehlercodes und Hilfe Codes finden Sie in der Android SDK- [Dokumentation](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) für die fingerprintmanager-Klasse. Xamarin. Android stellt diese Werte mit der `Android.Hardware.Fingerprints.FingerprintState` Enumeration dar:

- **`AcquiredGood`** &ndash; (Wert 0) war das abgerufene Image gut.

- **`AcquiredImagerDirty`** &ndash; (Wert 3) war das Fingerabdruck Bild aufgrund von verdächtigem oder erkanntem Schmutz auf dem Sensor zu laut. Beispielsweise ist es sinnvoll, dies nach mehreren `AcquiredInsufficient` oder der tatsächlichen Erkennung von Schmutz auf dem Sensor (bei hängen von Pixel, swaths usw.) zurückzugeben. Es wird erwartet, dass der Benutzer Maßnahmen zum Bereinigen des Sensors übernimmt, wenn dieser zurückgegeben wird.

- **`AcquiredInsufficient`** &ndash; (Wert 2) war das Fingerabdruck Bild aufgrund einer erkannten Bedingung (d. h. der trockenen Skin) oder eines möglicherweise Dirty Sensors zu stark zu veralten (siehe `AcquiredImagerDirty`.

- **`AcquiredPartial`** &ndash; (Wert 1) wurde nur ein partielles Fingerabdruck Image erkannt. Während der Registrierung sollte der Benutzer informiert werden, was zum Beheben dieses Problems geschehen muss, z. b. &ldquo;, um auf den Sensor zu klicken.&rdquo;

- **`AcquiredTooFast`** &ndash; (Wert 5) war das Fingerabdruck Bild aufgrund von schnell Bewegung unvollständig. Dies ist in erster Linie für lineare Array Sensoren geeignet, kann jedoch auch auftreten, wenn der Finger während des Erwerbs verschoben wurde. Der Benutzer sollte aufgefordert werden, den Finger langsamer (linear) zu bewegen oder den Finger länger auf dem Sensor zu belassen.

- **`AcquiredToSlow`** &ndash; (Wert 4) war das Fingerabdruck Bild aufgrund mangelnder Bewegung nicht lesbar. Dies ist am besten für lineare Array Sensoren geeignet, die eine Schwenkbewegung erfordern.

- **`ErrorCanceled`** &ndash; (Wert 5) der Vorgang wurde abgebrochen, da der Fingerabdrucksensor nicht verfügbar ist. Dies kann z. b. der Fall sein, wenn der Benutzer gewechselt wird, das Gerät gesperrt ist, oder ein anderer ausstehender Vorgang Dies verhindert oder deaktiviert.

- **`ErrorHwUnavailable`** &ndash; (Wert 1) ist die Hardware nicht verfügbar. Versuchen Sie es später erneut.

- **`ErrorLockout`** &ndash; (Wert 7) wurde der Vorgang abgebrochen, da die API aufgrund zu vieler Versuche gesperrt ist.

- **`ErrorNoSpace`** &ndash; (Wert 4) Fehlerzustand, der für Vorgänge wie die Registrierung zurückgegeben wurde. der Vorgang kann nicht abgeschlossen werden, da nicht genügend Speicherplatz vorhanden ist, um den Vorgang abzuschließen.

- **`ErrorTimeout`** &ndash; (Value 3) Fehlerzustand, der zurückgegeben wird, wenn die aktuelle Anforderung zu lang ausgeführt wurde. Dies soll verhindern, dass Programme unbegrenzt auf den Fingerabdrucksensor warten. Das Timeout ist Platt Form-und Sensor spezifisch, liegt jedoch im Allgemeinen bei ungefähr 30 Sekunden.

- der Fehlerstatus **`ErrorUnableToProcess`** &ndash; (Wert 2), der zurückgegeben wird, wenn der Sensor das aktuelle Bild nicht verarbeiten konnte.

## <a name="related-links"></a>Verwandte Links

- [Verschlüsselungs](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
