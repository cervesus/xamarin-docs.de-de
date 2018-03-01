---
title: "Überprüfung von Fingerabdrücken"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/23/2016
ms.openlocfilehash: 678ceaf122550c6561541533405fe3500d192110
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="scanning-for-fingerprints"></a>Überprüfung von Fingerabdrücken

Nun, dass zum Vorbereiten einer Anwendung Xamarin.Android Fingerabdruckauthentifizierung verwenden gesehen, wir zum Zurückgeben der `FingerprintManager.Authenticate` -Methode, und erläutern seine Position in der Android 6.0 Fingerabdruck-Authentifizierung. Eine kurze Übersicht über den Workflow für die Fingerabdruckauthentifizierung wird in dieser Liste beschrieben:

1. Aufrufen `FingerprintManager.Authenticate`, und übergeben Sie eine `CryptoObject` und eine `FingerprintManager.AuthenticationCallback` Instanz. Die `CryptoObject` wird verwendet, um sicherzustellen, dass das Authentifizierungsergebnis Fingerabdruck nicht manipuliert wurde. 
2. Unterklasse der [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) Klasse. Eine Instanz dieser Klasse wird für bereitgestellt `FingerprintManager` Fingerabdruck wenn Authentifizierung beginnt. Wenn der Fingerabdruck Scanner abgeschlossen ist, wird es eines Rückrufmethoden für diese Klasse aufgerufen.
3. Schreiben Sie Code zum Aktualisieren der Benutzeroberflächenautomatisierungs, um den Benutzer wissen, dass das Gerät den Fingerabdruck Scanner gestartet wurde und darauf, dass eine Benutzerinteraktion wartet lassen. 
4. Klicken Sie nach Abschluss der Fingerabdruck Scanner zurückgeben Android Ergebnisse an die Anwendung durch Aufrufen einer Methode auf die `FingerprintManager.AuthenticationCallback` -Instanz, die im vorherigen Schritt bereitgestellt wurde.
5. Die Anwendung informieren Sie den Benutzer, der das Ergebnis der Fingerabdruck Authentifizierung und auf die Ergebnisse entsprechend reagieren. 

Der folgende Codeausschnitt ist ein Beispiel für eine Methode in eine Aktivität, die Überprüfung von Fingerabdrücken gestartet wird:

```csharp
protected void FingerPrintAuthenticationExample()
{
    const int flags = 0; /* always zero (0) */

    // CryptoObjectHelper is described in the previous section.
    CryptoObjectHelper cryptoHelper = new CryptoObjectHelper();    
    
    // cancellationSignal can be used to manually stop the fingerprint scanner. 
    cancellationSignal = new Android.Support.V4.OS.CancellationSignal();
    
    FingerprintManagerCompat fingerPrintManager = FingerprintManagerCompat.From(this);
    
    // AuthenticationCallback is a base class that will be covered later on in this guide.
    FingerprintManagerCompat.AuthenticationCallback authenticationCallback = new MyAuthCallbackSample(this);

    // Start the fingerprint scanner.
    fingerprintManager.Authenticate(cryptoHelper.BuildCryptoObject(), flags, cancellationSignal, authenticationCallback, null);
}
```

Betrachten Sie jede der folgenden Parameter in der `Authenticate` Methode etwas ausführlicher:

* Der erste Parameter ist ein _Crypto_ -Objekt, die der Fingerabdruck Scanner verwenden möchten, können Sie die Ergebnisse eines Scans Fingerabdruck zu authentifizieren. Dieses Objekt möglicherweise `null`, in diesem Fall muss die Anwendung Blind, dass keine Vertrauensstellung mit den Ergebnissen Fingerabdruck manipuliert wurde. Es wird empfohlen, eine `CryptoObject` instanziiert und bereitgestellt, um die `FingerprintManager` statt Null. [Erstellen eine CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) wird erläutert, im Detail wie beim Instanziieren einer `CryptoObject` basierend auf einer `Cipher`.
* Der zweite Parameter ist immer 0 (null). Die Android-Dokumentation wird diese als Gruppe von Flags, und es ist sehr wahrscheinlich für die zukünftige Verwendung reserviert. 
* Der dritte Parameter `cancellationSignal` ist ein Objekt, mit der Fingerabdruck Scanner deaktivieren und die aktuelle Anforderung abzubrechen. Dies ist ein [Android CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html), und nicht für einen Typ von .NET Framework.
* Der vierte Parameter ist obligatorisch und ist eine Klasse, Unterklassen der `AuthenticationCallback` abstrakte Klasse. Methoden in dieser Klasse wird aufgerufen, um Clients zu signalisieren bei der `FingerprintManager` abgeschlossen hat und was die Ergebnisse sind. Wie viele zur Implementierung verstehen der `AuthenticationCallback`, im behandelt wird [ist eigenen Abschnitt](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* Der fünfte Parameter ist eine optionale `Handler` Instanz. Wenn eine `Handler` Objekt angegeben ist, die `FingerprintManager` verwenden die `Looper` aus dem Objekt bei der Verarbeitung von Nachrichten von der Hardware Fingerabdruck. In der Regel eine muss nicht bieten eine `Handler`, die FingerprintManager verwendet die `Looper` aus der Anwendung.

## <a name="cancelling-a-fingerprint-scan"></a>Abbrechen einer Fingerabdruck-Überprüfung

Es ist möglicherweise erforderlich, um den Benutzer (oder die Anwendung) Fingerabdruck Vorgang abbrechen, nachdem er initiiert wurde. In diesem Fall rufen die [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) Methode für die [ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html) , bereitgestellt wurde, um `FingerprintManager.Authenticate` Wenn es um den Fingerabdruck-Scan starten aufgerufen wurde.

Nun wir gesehen haben, die `Authenticate` -Methode, sehen wir uns einige der wichtigeren Parameter im Detail. Zuerst, betrachten wir [reagieren auf Authentifizierungsrückrufen](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), die besprechen wie Unterklasse der [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), aktivieren eine Android-Anwendung, zu reagieren die Ergebnisse der Fingerabdruck Scanner.




## <a name="related-links"></a>Verwandte Links

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
