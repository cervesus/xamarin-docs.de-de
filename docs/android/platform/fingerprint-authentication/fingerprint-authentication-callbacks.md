---
title: Reagieren auf Authentifizierungsrückrufe
ms.prod: xamarin
ms.assetid: 6533AFC9-1A1C-4897-A154-4D4ECFE27761
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/06/2017
ms.openlocfilehash: c720a30a59eea8f1ed74033da8d1c045a1fb9109
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666865"
---
# <a name="responding-to-authentication-callbacks"></a>Reagieren auf Authentifizierungsrückrufe

Des fingerabdruckscanners in einem eigenen Thread im Hintergrund ausgeführt, und nach Abschluss Systemtabellennamen und meldet die Ergebnisse der Überprüfung durch Aufrufen einer Methode des `FingerprintManager.AuthenticationCallback` im UI-Thread. Eine Android-Anwendung muss seinen eigenen Handler angeben, der erweitert diese abstrakte Klasse, die alle der folgenden Methoden implementieren:

* **`OnAuthenticationError(int errorCode, ICharSequence errString)`** &ndash; Wird aufgerufen, wenn ein nicht behebbarer Fehler auftritt. Es ist nichts anderes, wie eine Anwendung oder Benutzer kann Beheben des Problems mit der Ausnahme möglicherweise erneut versuchen.
* **`OnAuthenticationFailed()`** &ndash; Diese Methode wird aufgerufen, wenn ein Fingerabdruck wurde erkannt, aber nicht vom Gerät erkannt.
* **`OnAuthenticationHelp(int helpMsgId, ICharSequence helpString)`** &ndash; Wird aufgerufen, wenn es ein behebbarer Fehler, z. B. den Finger Touchscreen-zu schnell über die Überprüfung.
* **`OnAuthenticationSucceeded(FingerprintManagerCompati.AuthenticationResult result)`** &ndash; Wird aufgerufen, wenn ein Fingerabdruck erkannt wurde.

Wenn eine `CryptoObject` verwendet wurde, beim Aufrufen von `Authenticate`, es wird empfohlen, rufen Sie `Cipher.DoFinal` in `OnAuthenticationSuccessful`.
`DoFinal` wird eine Ausnahme ausgelöst, wenn das Verschlüsselungsverfahren manipuliert oder nicht ordnungsgemäß initialisiert wurde, gibt an, dass das Ergebnis des fingerabdruckscanners außerhalb der Anwendung manipuliert sein kann.


> [!NOTE]
> Es wird empfohlen, die für die Rückruf-Klasse relativ geringe Gewichtung und frei von anwendungsspezifische Logik. Die Rückrufe sollten als eine "Datenverkehrspolizist" zwischen der Android-Anwendung und die Ergebnisse von des fingerabdruckscanners fungieren.

## <a name="a-sample-authentication-callback-handler"></a>Einen Rückrufhandler für Beispiel-Authentifizierung

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

`OnAuthenticationSucceeded` überprüft, ob eine `Cipher` demonstriert die `FingerprintManager` beim `Authentication` aufgerufen wurde. Wenn dies der Fall ist, die `DoFinal` Methode für das Verschlüsselungsverfahren aufgerufen wird. Dies schließt die `Cipher`, auf den ursprünglichen Zustand wiederherstellen. Wenn gab es ein Problem mit der Verschlüsselung, klicken Sie dann `DoFinal` löst eine Ausnahme aus, und der Authentifizierungsversuch fehlgeschlagen angesehen werden.

Die `OnAuthenticationError` und `OnAuthenticationHelp` Rückrufe, die jede empfangen, eine ganze Zahl, der angibt, was das Problem war. Im folgende Abschnitt wird erläutert, alle möglichen Hilfe oder Fehlercodes. Die zwei Rückrufe dienen sehr ähnlichen Zwecken &ndash; auf die Anwendung zu informieren, dass die Fingerabdruckauthentifizierung ist fehlgeschlagen. Wie sie sich unterscheiden, ist der Schweregrad. `OnAuthenticationHelp` ist ein Benutzer behebbarer Fehler, wie z. B. das Wischen von des Fingerabdrucks nicht so schnell. `OnAuthenticationError` mehr ein schwerer Fehler, z. B. einen beschädigten fingerabdruckscanners ist.

Beachten Sie, dass `OnAuthenticationError` wird aufgerufen, wenn die Überprüfung per Fingerabdruck, über abgebrochen wird die `CancellationSignal.Cancel()` Nachricht. Die `errMsgId` Parameter hat den Wert 5 (`FingerprintState.ErrorCanceled`). Je nach den Anforderungen, die eine Implementierung der `AuthenticationCallbacks` dies möglicherweise anders als die anderen Fehler behandelt. 

`OnAuthenticationFailed` wird aufgerufen, wenn der Fingerabdruck wurde erfolgreich überprüft entsprach jedoch keine Fingerabdruck, der mit dem Gerät registriert. 

## <a name="help-codes-and-error-message-ids"></a>Help-Codes und Fehler Meldungs-Ids 

Eine Liste und Beschreibung der Fehlercodes und der Help-Codes finden Sie unter den [Android SDK-Dokumentation](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html#FINGERPRINT_ACQUIRED_GOOD) für die FingerprintManager-Klasse. Xamarin.Android stellt diese Werte mit den `Android.Hardware.Fingerprints.FingerprintState` Enumeration:


-   **`AcquiredGood`** &ndash; (Wert 0) Das Bild, das abgerufen wurde gut.


-   **`AcquiredImagerDirty`** &ndash; (Wert 3) Der Fingerabdruck-Image wurde aufgrund eines vermuteten oder erkannt wurde, auf dem Sensor zu viel Rauschen verursacht. Beispielsweise ist es sinnvoll, dies nach mehreren zurück `AcquiredInsufficient` oder die tatsächliche Erkennung von wurde auf dem Sensor (fixierten Pixel, Adressbereiche usw.). Auszuführende Aktion, die den Sensor zu bereinigen, wenn dieser zurückgegeben wird, wird der Benutzer erwartet.


-   **`AcquiredInsufficient`** &ndash; (Wert 2) Das Image per Fingerabdruck zu viel Rauschen verursacht, verarbeiten Sie aufgrund einer Bedingung aufgetreten (d. h. dry Skin) oder einem Sensor möglicherweise geändert wurde (siehe `AcquiredImagerDirty`.



-   **`AcquiredPartial`** &ndash; (Wert 1) Nur eine partielle Fingerabdruck-Image wurde erkannt. Während der Registrierung sollte der Benutzer informiert werden in die auftreten, die zum Beheben dieses Problems, z. B. &ldquo;drücken Sie fest, auf dem Sensor.&rdquo;



-   **`AcquiredTooFast`** &ndash; (der Wert 5) Der Fingerabdruck Image war unvollständig, da kurze Bewegungsdaten. Zwar hauptsächlich für lineare Array Sensoren geeignet, kann dies auch geschehen, wenn der Finger während der Übernahme verschoben wurde. Sollte der Benutzer gefragt werden, verschieben Sie den Finger langsamer (lineare), oder übernehmen Sie den Finger auf dem Sensor mehr.




-   **`AcquiredToSlow`** &ndash; (Wert 4) Der Fingerabdruck-Image wurde aufgrund einer fehlenden während der Übertragung nicht lesbar. Dies ist am besten geeigneten für lineare Array Sensoren, die eine streifbewegung Bewegung zu erfordern.



-   **`ErrorCanceled`** &ndash; (der Wert 5) Der Vorgang wurde abgebrochen, da fingerabdrucksensors nicht verfügbar ist. Dies kann z. B. bei der Benutzer wechselt, das Gerät gesperrt ist oder einem anderen ausstehenden Vorgang verhindert, dass diese aktiviert oder deaktiviert vorkommen.



-   **`ErrorHwUnavailable`** &ndash; (Wert 1) Die Hardware ist nicht verfügbar. Versuchen Sie es später noch mal.




-   **`ErrorLockout`** &ndash; (Wert 7) Der Vorgang wurde abgebrochen, da die API aufgrund von zu viele Anmeldeversuche gesperrt ist.




-   **`ErrorNoSpace`** &ndash; (Wert 4) Status "Fehler", die für Vorgänge wie die Registrierung zurückgegeben; der Vorgang kann nicht abgeschlossen werden, da nicht genügend Speicher zum Abschließen des Vorgangs noch vorhanden ist.



-   **`ErrorTimeout`** &ndash; (Wert 3) Der Fehlerstatus zurückgegeben, wenn die aktuelle Anforderung zu lang ausgeführt wurde. Dies soll verhindern, dass Programme fingerabdrucksensors auf unbestimmte Zeit warten. Das Timeout ist Plattform und das Sensor-spezifische, aber es ist in der Regel ungefähr 30 Sekunden.



-   **`ErrorUnableToProcess`** &ndash; (Wert 2) Der Fehlerstatus zurückgegeben, wenn der Sensor konnte nicht das aktuelle Bild verarbeitet wurde.



## <a name="related-links"></a>Verwandte Links

- [Verschlüsselungsstärke](https://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [AuthenticationCallback](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.AuthenticationCallback.html)
