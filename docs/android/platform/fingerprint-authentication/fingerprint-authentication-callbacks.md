---
title: "Reagieren auf Authentifizierungsrückrufen"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/06/2017
ms.openlocfilehash: 37d288cd75f232c8674aece085a78a83ce12d720
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="responding-to-authentication-callbacks"></a>Reagieren auf Authentifizierungsrückrufen

Der Fingerabdruck Scanner, die in einem eigenen Thread im Hintergrund ausgeführt, und abschließend wird er meldet die Ergebnisse der Überprüfung durch Aufrufen einer Methode des `FingerprintManager.AuthenticationCallback` im UI-Thread. Eine Android-Anwendung muss einen eigenen Handler bereitstellen, der erweitert diese abstrakte Klasse, implementieren die folgenden Methoden:

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Wird aufgerufen, wenn ein nicht behebbarer Fehler vorliegt. Es ist keine weitere Aktion, die eine Anwendung oder ein Benutzer ausführen kann, um das Problem mit der Ausnahme möglicherweise erneut zu beheben.
* **`OnAuthenticationFailed()`** &ndash; Diese Methode wird aufgerufen, wenn ein Fingerabdruck wurde erkannt, aber nicht vom Gerät erkannt.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Wird aufgerufen, wenn ein behebbarer Fehler, z. B. den Finger wird zum schnellen über der Scanner Magnetstreifenkarte vorhanden ist.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; Wird aufgerufen, wenn ein Fingerabdruck erkannt wurde.

Wenn eine `CryptoObject` verwendet wurde, beim Aufrufen von `Authenticate`, es wird empfohlen, rufen Sie `Cipher.DoFinal` in `OnAuthenticationSuccessful`.
`DoFinal` löst eine Ausnahme aus, wenn das Verschlüsselungsverfahren zu manipulieren oder nicht ordnungsgemäß initialisiert wurde, gibt an, dass das Ergebnis des Scanners Fingerabdruck außerhalb der Anwendung manipuliert wurden möglicherweise.


> [!NOTE]
> Es wird empfohlen, die Rückruf-Klasse relativ gemäßigte Gewichtung und frei von anwendungsspezifische Logik. Die Rückrufe sollten als eine "Datenverkehr kopieren" zwischen der Android-Anwendung und die Ergebnisse vom Fingerabdruck Scanner fungieren.

## <a name="a-sample-authentication-callback-handler"></a>Ein Beispiel Rückruf Authentifizierungshandler

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

`OnAuthenticationSucceeded` überprüft, ob eine `Cipher` bereitgestellt wurde, um `FingerprintManager` Wenn `Authentication` aufgerufen wurde. Wenn dies der Fall ist, die `DoFinal` das Verschlüsselungsverfahren Methode aufgerufen wird. Dies schließt die `Cipher`, auf den ursprünglichen Zustand wiederherstellen. Wenn es dann ein Problem mit der Verschlüsselung wurde `DoFinal` löst eine Ausnahme aus, und der Authentifizierungsversuch angesehen wird als fehlgeschlagen betrachtet.

Die `OnAuthenticationError` und `OnAuthenticationHelp` Rückrufe jedes empfangen eine ganze Zahl, der angibt, was das Problem auftrat. Im folgende Abschnitt wird erläutert, alle möglichen Hilfe oder Fehlercodes. Die zwei Rückrufe für ähnliche Zwecke verwendet &ndash; an die Anwendung darüber zu informieren, Fingerabdruckauthentifizierung fehlgeschlagen ist. Der Schweregrad werden unterschieden. `OnAuthenticationHelp` ist ein Benutzer behebbarer Fehler, z. B. den Fingerabdruck zu schnell Streifen. `OnAuthenticationError` mehr ein schwerer Fehler, z. B. ein beschädigter Fingerabdruck Scanner ist.

Beachten Sie, dass `OnAuthenticationError` wird aufgerufen, wenn die Überprüfung Fingerabdruck, über abgebrochen wird die `CancellationSignal.Cancel()` Nachricht. Die `errMsgId` Parameter hat den Wert 5 (`FingerprintState.ErrorCanceled`). Je nach den Anforderungen, die eine Implementierung der `AuthenticationCallbacks` dieser Situation möglicherweise anders als die anderen Fehler behandelt. 

`OnAuthenticationFailed` wird aufgerufen, wenn der Fingerabdruck wurde erfolgreich überprüft, aber alle Fingerabdruck beim Gerät angemeldet stimmte nicht überein. 

## <a name="help-codes-and-error-message-ids"></a>Hilfe-Codes und Fehler Nachrichten-Ids 

Eine Liste und Beschreibung des Fehlercodes und Hilfe Codes finden Sie in der [Android SDK-Dokumentation](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) für die FingerprintManager-Klasse. Xamarin.Android stellt diese Werte mit den `Android.Hardware.Fingerprints.FingerprintState` Enum:


-   **`AcquiredGood`** &ndash; (Wert 0) Das Image erworben wurde gut.


-   **`AcquiredImagerDirty`** &ndash; (Wert 3) Der Fingerabdruck Image war aufgrund von potenziellen oder erkannte Schmutz auf der Sensor zu viel Rauschen verursacht. Beispielsweise ist es sinnvoll, diese nach mehreren zurückgeben `AcquiredInsufficient` oder die tatsächliche Erkennung von Schmutz auf der Sensor (fixierten Pixel, Adressbereiche usw.). Der Benutzer wird erwartet, Maßnahmen zum Bereinigen des Sensors, wenn dies zurückgegeben wird.


-   **`AcquiredInsufficient`** &ndash; (Wert 2) Das Bild Fingerabdruck zu viel Rauschen verursacht aufgrund einer erkannten Bedingung (d. h. trockenen Design) oder ein möglicherweise dirty Sensor verarbeitet wurde (siehe `AcquiredImagerDirty`.



-   **`AcquiredPartial`** &ndash; (Wert 1) Nur eine teilweise Fingerabdruck Image wurde erkannt. Während der Anmeldung der Benutzer auf herrscht zum Lösen dieses Problems, z. B. der Fall sein sollte informiert werden &ldquo;Sensor, drücken Sie fest.&rdquo;



-   **`AcquiredTooFast`** &ndash; (der Wert 5) Der Fingerabdruck Image wurde wegen der schnellen Bewegung unvollständig. Während für lineare Array Sensoren größtenteils geeignet, könnte dies auch vorkommen, wenn der Finger während der Übernahme verschoben wurde. Der Benutzer sollte aufgefordert, verschieben Sie den Finger langsamer (linear), oder lassen Sie den Finger auf dem Sensor länger.




-   **`AcquiredToSlow`** &ndash; (Wert 4) Der Fingerabdruck Image war aufgrund fehlender während des Verschiebens nicht lesbar. Dies ist die am besten geeigneten für lineare Array Sensoren, die eine Bewegung Wischen erfordern.



-   **`ErrorCanceled`** &ndash; (der Wert 5) Der Vorgang wurde abgebrochen, da fingerabdrucksensors nicht verfügbar ist. Dies kann z. B. vorkommen, wenn der Benutzer gewechselt ist, das Gerät gesperrt ist oder einen anderen ausstehenden Vorgang wird verhindert, dass diese aktiviert oder deaktiviert.



-   **`ErrorHwUnavailable`** &ndash; (Wert 1) Die Hardware ist nicht verfügbar. Versuchen Sie es später noch einmal.




-   **`ErrorLockout`** &ndash; (Wert 7) Der Vorgang wurde abgebrochen, da die API aufgrund zu viele Anmeldeversuche gesperrt ist.




-   **`ErrorNoSpace`** &ndash; (Wert 4) Status "Fehler" für Vorgänge wie das Enrollment zurückgegeben; der Vorgang kann nicht abgeschlossen werden, da nicht genügend Speicherplatz zum Abschließen des Vorgangs noch vorhanden ist.



-   **`ErrorTimeout`** &ndash; (Wert 3) Status "Fehler" zurückgegeben, wenn die aktuelle Anforderung zu lang ausgeführt wurde. Dies soll verhindern, dass Programme fingerabdrucksensors unbegrenzt wartet. Das Timeout ist Plattform und Sensor-spezifische, aber in der Regel ungefähr 30 Sekunden.



-   **`ErrorUnableToProcess`** &ndash; (Wert 2) Status "Fehler" zurückgegeben, wenn der Sensor das aktuelle Bild zu verarbeiten konnte.



## <a name="related-links"></a>Verwandte Links

- [Cipher](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
