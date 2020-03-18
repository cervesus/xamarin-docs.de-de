---
title: Überprüfung auf Fingerabdrücke
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/23/2016
ms.openlocfilehash: 61edd0e4b532f18a8fc28502e5bb990703068776
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027496"
---
# <a name="scanning-for-fingerprints"></a>Überprüfung auf Fingerabdrücke

Nachdem Sie nun gesehen haben, wie eine Xamarin.Android-Anwendung für die Verwendung der Fingerabdruckauthentifizierung vorbereitet wird, kehren wir zur `FingerprintManager.Authenticate`-Methode zurück und beschreiben ihre Aufgabe bei der Android 6.0-Fingerabdruckauthentifizierung. Eine kurze Übersicht über den Workflow für die Fingerabdruckauthentifizierung wird in dieser Liste beschrieben:

1. Rufen Sie `FingerprintManager.Authenticate` auf, und übergeben Sie ein `CryptoObject` und eine `FingerprintManager.AuthenticationCallback`-Instanz. Das `CryptoObject` wird verwendet, um sicherzustellen, dass das Ergebnis der Fingerabdruckauthentifizierung nicht manipuliert wurde. 
2. Erstellen Sie die [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)-Klasse als Unterklasse. Eine Instanz dieser Klasse wird für `FingerprintManager` bereitgestellt, wenn die Fingerabdruckauthentifizierung gestartet wird. Wenn die Fingerabdrucküberprüfung abgeschlossen ist, wird eine der Rückrufmethoden für diese Klasse aufgerufen.
3. Schreiben Sie Code, um die Benutzeroberfläche zu aktualisieren, damit der Benutzer weiß, dass das Gerät die Fingerabdrucküberprüfung gestartet hat und auf eine Benutzerinteraktion wartet. 
4. Wenn die Fingerabdrucküberprüfung abgeschlossen ist, gibt Android Ergebnisse an die Anwendung zurück, indem eine Methode für die `FingerprintManager.AuthenticationCallback`-Instanz aufgerufen wird, die im vorherigen Schritt bereitgestellt wurde.
5. Die Anwendung informiert den Benutzer über die Ergebnisse der Fingerabdruckauthentifizierung und reagiert entsprechend auf die Ergebnisse. 

Der folgende Codeausschnitt ist ein Beispiel für eine Methode in einer Aktivität, die mit der Überprüfung auf Fingerabdrücke beginnt:

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Im folgenden werden die einzelnen Parameter in der `Authenticate`-Methode ausführlicher erläutert:

- Der erste Parameter ist ein _crypto_-Objekt, das die Fingerabdrucküberprüfung verwendet, um die Ergebnisse einer Fingerabdrucküberprüfung zu authentifizieren. Dieses Objekt kann `null` sein. in diesem Fall muss die Anwendung blind darauf vertrauen, dass die Fingerabdruckergebnisse nicht manipuliert wurden. Es wird empfohlen, ein `CryptoObject` zu instanziieren und für `FingerprintManager` anstelle von NULL bereitzustellen. Das Instanziieren eines `CryptoObject` basierend auf einem `Cipher`-Element wird unter [Erstellen eines CryptoObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) ausführlich erläutert.
- Der zweite Parameter ist immer NULL. Die Android-Dokumentation identifiziert dies als Satz von Flags, die höchstwahrscheinlich für zukünftige Verwendung reserviert sind. 
- Der dritte Parameter (`cancellationSignal`) ist ein Objekt, das zum Deaktivieren der Fingerabdrucküberprüfung und zum Abbrechen der aktuellen Anforderung verwendet wird. Dabei handelt es sich um eine [Android-CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html) und nicht um einen Typ aus .NET Framework.
- Der vierte Parameter ist obligatorisch und eine Klasse, die die abstrakte Klasse `AuthenticationCallback` als Unterklasse verwendet. Methoden für diese Klasse werden aufgerufen, um Clients zu signalisieren, dass `FingerprintManager` abgeschlossen wurde und welche Ergebnisse vorliegen. Da bei der Implementierung von `AuthenticationCallback` viel zu beachten ist, wird sie in [einem eigenen Abschnitt](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md) behandelt.
- Der fünfte Parameter ist eine optionale `Handler`-Instanz. Wenn ein `Handler`-Objekt bereitgestellt wird, verwendet `FingerprintManager` den `Looper` aus diesem Objekt, wenn die Nachrichten von der Fingerabdruckhardware verarbeitet werden. In der Regel ist eine nicht erforderlich, einen `Handler` bereitzustellen, da der FingerprintManager den `Looper` aus der Anwendung verwendet.

## <a name="cancelling-a-fingerprint-scan"></a>Abbrechen einer Fingerabdrucküberprüfung

Möglicherweise ist es erforderlich, dass der Benutzer (oder die Anwendung) die Fingerabdrucküberprüfung abbricht, nachdem sie initiiert wurde. Rufen Sie in diesem Fall die [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled())-Methode für das [`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html) auf, das für `FingerprintManager.Authenticate` bereitgestellt wurde, als die Fingerabdrucküberprüfung durch den Aufruf gestartet wurde.

Nachdem wir nun die `Authenticate`-Methode kennengelernt haben, betrachten wir einige der wichtigeren Parameter ausführlicher. Zunächst sehen wir uns [Antworten auf Authentifizierungsrückrufe](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md) an. Dabei wird erläutert, wie [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) als Unterklasse verwendet wird und einer Android-Anwendung ermöglicht, auf die von der Fingerabdrucküberprüfung bereitgestellten Ergebnisse zu reagieren.

## <a name="related-links"></a>Verwandte Links

- [CancellationSignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
