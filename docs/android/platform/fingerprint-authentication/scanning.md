---
title: Überprüfung auf Fingerabdrücke
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/23/2016
ms.openlocfilehash: ed7f31b011c32b25f431801e47c570900589e131
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106324"
---
# <a name="scanning-for-fingerprints"></a>Überprüfung auf Fingerabdrücke

Nun, wir vorbereiten eine Xamarin.Android-Anwendung zur Verwendung von Authentifizierung per Fingerabdruck gesehen haben, kehren wir zurück zum die `FingerprintManager.Authenticate` -Methode, und Erläutern Sie seine Position in der Android 6.0-Authentifizierung per Fingerabdruck. Eine kurze Übersicht über den Workflow für die Authentifizierung per Fingerabdruck wird in dieser Liste beschrieben:

1. Rufen Sie `FingerprintManager.Authenticate`, und übergeben Sie einen `CryptoObject` und `FingerprintManager.AuthenticationCallback` Instanz. Die `CryptoObject` verwendet, um sicherzustellen, dass das Ergebnis der Authentifizierung per Fingerabdruck nicht manipuliert wurde. 
2. Unterklasse der [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) Klasse. Eine Instanz dieser Klasse bereitgestellt `FingerprintManager` Wenn Fingerabdruck Authentifizierung gestartet wird. Nach Abschluss des fingerabdruckscanners wird einer der Rückrufmethoden für diese Klasse aufgerufen.
3. Schreiben Sie Code, um die Benutzeroberfläche, um den Benutzer informieren, dass das Gerät des fingerabdruckscanners gestartet wurde und darauf, dass eine Benutzerinteraktion wartet zu aktualisieren. 
4. Nach Abschluss des fingerabdruckscanners Android wird geben Ergebnisse zurück an die Anwendung durch Aufrufen einer Methode auf die `FingerprintManager.AuthenticationCallback` -Instanz, die im vorherigen Schritt bereitgestellt wurde.
5. Die Anwendung informiert den Benutzer über das Ergebnis der Authentifizierung per Fingerabdruck und die Ergebnisse als geeigneten reagieren. 

Der folgende Codeausschnitt ist ein Beispiel für eine Methode in einer Aktivität, die Überprüfung auf Fingerabdrücke gestartet wird:

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

Betrachten wir jeden dieser Parameter in der `Authenticate` Methode etwas ausführlicher:

* Der erste Parameter ist ein _Crypto_ Objekt, mit der fingerabdruckscanners wird können die Ergebnisse der Überprüfung per Fingerabdruck zu authentifizieren. Dieses Objekt möglicherweise `null`, die Ergebnisse der Fingerabdruck manipuliert hat die Anwendung muss in diesem Fall, dass es keine Blind zu vertrauen. Es wird empfohlen, eine `CryptoObject` instanziiert und bereitgestellt, um die `FingerprintManager` statt Null. [Erstellen eine CryptObject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) erläutern, im Detail wie Instanziieren einer `CryptoObject` basierend auf einer `Cipher`.
* Der zweite Parameter ist immer 0 (null). Die Android-Dokumentation bezeichnet dies als Satz von Flags und ist in den meisten Fällen für die zukünftige Verwendung reserviert. 
* Der dritte Parameter `cancellationSignal` ist ein Objekt, das zum Deaktivieren des fingerabdruckscanners und brechen Sie die aktuelle Anforderung. Dies ist ein [Android CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html), und nicht für einen Typ von .NET Framework.
* Der vierte Parameter ist obligatorisch und ist eine Klasse, die Unterklassen der `AuthenticationCallback` abstrakte Klasse. Methoden in dieser Klasse aufgerufen, um Clients zu signalisieren bei der `FingerprintManager` wurde und was die Ergebnisse sind. Wie viele zur Implementierung verstehen der `AuthenticationCallback`, im behandelt wird [ist eigenen Abschnitt](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md).
* Der fünfte Parameter ist eine optionale `Handler` Instanz. Wenn eine `Handler` Objekt angegeben ist, die `FingerprintManager` verwendet die `Looper` aus dem Objekt, bei der Verarbeitung von Nachrichten aus der Fingerprint-Hardware. In der Regel eine muss nicht zu einem `Handler`, die FingerprintManager verwenden die `Looper` aus der Anwendung.

## <a name="cancelling-a-fingerprint-scan"></a>Eine Überprüfung der Fingerabdruck wird abgebrochen.

Es kann erforderlich sein, für den Benutzer (oder der Anwendung), Fingerabdruck diesen Vorgang abbrechen, nachdem es initiiert wurde. In diesem Fall rufen die [ `IsCancelled` ](http://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) Methode für die [ `CancellationSignal` ](http://developer.android.com/reference/android/os/CancellationSignal.html) , demonstriert die `FingerprintManager.Authenticate` Wenn es um den Fingerabdruck-Scan starten aufgerufen wurde.

Nun, wir gesehen haben die `Authenticate` -Methode, sehen wir uns einige der wichtigere Parameter noch ausführlicher. Zunächst betrachten wir [reagieren auf Authentifizierungsrückrufe](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md), besprechen die Unterklasse wie die [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html), aktivieren eine Android-Anwendung, um zu reagieren die Ergebnisse des fingerabdruckscanners.




## <a name="related-links"></a>Verwandte Links

- [CancellationSignal](http://developer.android.com/reference/android/os/CancellationSignal.html)
- [FingerprintManager.AuthenticationCallback](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [FingerprintManager.CryptoObject](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [FingerprintManagerCompat.CryptoObject](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
