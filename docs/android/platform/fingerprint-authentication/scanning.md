---
title: Suchen nach Fingerabdrücken
ms.prod: xamarin
ms.assetid: 1CDDC096-77E0-47B3-BE0B-8953E2DDACD3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/23/2016
ms.openlocfilehash: 61edd0e4b532f18a8fc28502e5bb990703068776
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027496"
---
# <a name="scanning-for-fingerprints"></a>Suchen nach Fingerabdrücken

Nachdem Sie nun gesehen haben, wie eine xamarin. Android-Anwendung für die Verwendung der Fingerabdruckauthentifizierung vorbereitet wird, kehren wir zur `FingerprintManager.Authenticate`-Methode zurück und besprechen ihre Stelle in der Android 6,0-Fingerabdruckauthentifizierung. Eine kurze Übersicht über den Workflow für die Fingerabdruckauthentifizierung wird in dieser Liste beschrieben:

1. Rufen Sie `FingerprintManager.Authenticate`auf, und übergeben Sie eine `CryptoObject` und eine `FingerprintManager.AuthenticationCallback` Instanz. Der `CryptoObject` wird verwendet, um sicherzustellen, dass das Ergebnis der Fingerabdruckauthentifizierung nicht manipuliert wurde. 
2. Unterklasse der [fingerprintmanager. authenticationcallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html) -Klasse. Eine Instanz dieser Klasse wird für `FingerprintManager` bereitgestellt, wenn die Fingerabdruckauthentifizierung gestartet wird. Wenn der Fingerabdruckscanner abgeschlossen ist, wird eine der Rückruf Methoden für diese Klasse aufgerufen.
3. Schreiben Sie Code, um die Benutzeroberfläche zu aktualisieren, damit der Benutzer weiß, dass das Gerät den Fingerabdruckscanner gestartet hat und auf eine Benutzerinteraktion wartet. 
4. Wenn der Fingerabdruckscanner abgeschlossen ist, gibt Android Ergebnisse an die Anwendung zurück, indem eine Methode für die `FingerprintManager.AuthenticationCallback`-Instanz aufgerufen wird, die im vorherigen Schritt bereitgestellt wurde.
5. Die Anwendung informiert den Benutzer über die Ergebnisse der Fingerabdruckauthentifizierung und reagiert entsprechend auf die Ergebnisse. 

Der folgende Code Ausschnitt ist ein Beispiel für eine Methode in einer Aktivität, die mit der Überprüfung auf Fingerabdrücke beginnt:

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

- Der erste Parameter ist ein _Kryptografieobjekt_ , mit dem der Fingerabdruckscanner die Ergebnisse eines Fingerabdruck Scans authentifizieren kann. Dieses Objekt kann `null`werden. in diesem Fall muss die Anwendung blind darauf vertrauen, dass die Fingerabdruck Ergebnisse nicht manipuliert wurden. Es wird empfohlen, eine `CryptoObject` zu instanziiert und für den `FingerprintManager` anstelle von NULL bereitzustellen. Beim [Erstellen eines kryptobject](~/android/platform/fingerprint-authentication/creating-a-cryptoobject.md) wird ausführlich erläutert, wie eine `CryptoObject` auf der Grundlage einer `Cipher`instanziiert wird.
- Der zweite Parameter ist immer 0 (null). Die Android-Dokumentation identifiziert dies als Satz von Flags und ist höchstwahrscheinlich für die zukünftige Verwendung reserviert. 
- Der dritte Parameter, `cancellationSignal` ist ein Objekt, das zum Ausschalten des Fingerabdruckscanners und Abbrechen der aktuellen Anforderung verwendet wird. Dabei handelt es sich um ein [Android cancellationsignal](https://developer.android.com/reference/android/os/CancellationSignal.html)und nicht um einen Typ aus .NET Framework.
- Der vierte Parameter ist obligatorisch und eine Klasse, die die `AuthenticationCallback` abstrakten Klasse unterteilt. Methoden für diese Klasse werden aufgerufen, um Clients zu signalisieren, wenn die `FingerprintManager` abgeschlossen ist und was die Ergebnisse sind. Da die Implementierung des `AuthenticationCallback`viel zu verstehen ist, wird es im [eigenen Abschnitt](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)behandelt.
- Der fünfte Parameter ist eine optionale `Handler` Instanz. Wenn ein `Handler` Objekt bereitgestellt wird, verwendet die `FingerprintManager` den `Looper` dieses Objekts, wenn die Nachrichten von der Fingerabdruck Hardware verarbeitet werden. In der Regel ist eine `Handler`nicht erforderlich, da der fingerprintmanager die `Looper` der Anwendung verwendet.

## <a name="cancelling-a-fingerprint-scan"></a>Abbrechen eines Fingerabdruck Scans

Möglicherweise ist es erforderlich, dass der Benutzer (oder die Anwendung) die Fingerabdruck Überprüfung nach dem initiieren abbricht. Rufen Sie in diesem Fall die [`IsCancelled`](https://developer.android.com/reference/android/os/CancellationSignal.html#isCanceled()) -Methode für die [`CancellationSignal`](https://developer.android.com/reference/android/os/CancellationSignal.html) auf, die `FingerprintManager.Authenticate`, als Sie aufgerufen wurde, um den Fingerabdruck Scan zu starten.

Nachdem wir nun die `Authenticate`-Methode kennengelernt haben, betrachten wir einige der wichtigeren Parameter ausführlicher. Zuerst wird die [Reaktion auf Authentifizierungs Rückrufe](~/android/platform/fingerprint-authentication/fingerprint-authentication-callbacks.md)erläutert, in denen erläutert wird, wie die [fingerprintmanager. authenticationcallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)-Klasse unterteilt werden kann, sodass eine Android-Anwendung auf die vom Fingerabdruckscanner bereitgestellten Ergebnisse reagieren kann.

## <a name="related-links"></a>Verwandte Links

- [Cancellationsignal](https://developer.android.com/reference/android/os/CancellationSignal.html)
- [Fingerprintmanager. authenticationcallback](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.AuthenticationCallback.html)
- [Fingerprintmanager. cryptoobject](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.CryptoObject.html)
- [Fingerprintmanagercompat. cryptoobject](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.CryptoObject.html)
- [Fingerprintmanager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [Fingerprintmanagercompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
