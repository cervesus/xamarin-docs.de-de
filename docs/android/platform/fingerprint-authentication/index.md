---
title: Fingerabdruckauthentifizierung
description: Dieser Leitfaden erläutert, wie Sie die Authentifizierung per Fingerabdruck, eingeführt in Android 6.0 zu einer Xamarin.Android-Anwendung hinzufügen.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 70e12abdf61a6a0bfb36d281bcaa6214199e567d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023469"
---
# <a name="fingerprint-authentication"></a>Fingerabdruckauthentifizierung

_Dieser Leitfaden erläutert, wie Sie die Authentifizierung per Fingerabdruck, eingeführt in Android 6.0 zu einer Xamarin.Android-Anwendung hinzufügen._


## <a name="fingerprint-authentication-overview"></a>Übersicht über die Authentifizierung per Fingerabdruck

Der Eingang Fingerabdruckscanner auf Android-Geräten bietet Anwendungen eine Alternative zu der herkömmlichen Benutzername/Kennwort-Methode der Benutzerauthentifizierung. Die Verwendung von Fingerabdrücken zum Authentifizieren eines Benutzers ermöglicht es einer Anwendung, die Sicherheit zu integrieren, die weniger intrusiv als Benutzername und Kennwort.

Die FingerprintManager-APIs mit einem Fingerabdruck-Scanner-Zielgeräte und API-Ebene 23 (Android 6.0) ausgeführt werden oder höher. Die APIs befinden sich die `Android.Hardware.Fingerprints` Namespace. Die Bibliothek für Android-Unterstützung v4 enthält den Fingerabdruck-APIs vorgesehen, die für ältere Versionen von Android-Versionen. Die Kompatibilität APIs finden Sie in der `Android.Support.v4.Hardware.Fingerprint` -Namespace über verteilt werden die [Xamarin.Android.Support.v4 NuGet-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).

Die [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (und sein Gegenstück Unterstützungsbibliothek [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) ist die primäre Klasse für die Verwendung von der Hardware mittels fingerabdruckscan. Diese Klasse ist ein Android SDK-Wrapper für die Ebene Systemdienst, die Interaktionen mit der Hardware selbst verwaltet. Er ist verantwortlich für das Starten des fingerabdruckscanners und für die Reaktion auf Feedback von der Überprüfung. Diese Klasse verfügt über eine Schnittstelle ziemlich einfach, mit nur drei Member:

* **`Authenticate`** &ndash; Diese Methode wird die Überprüfung von Hardware zu initialisieren und starten Sie den Dienst im Hintergrund, warten, bis des Benutzers ihren Fingerabdruck scannen.
* **`EnrolledFingerprints`** &ndash; Diese Eigenschaft gibt `true` , wenn der Benutzer eine oder mehrere Fingerabdrücke mit dem Gerät registriert wurde.
* **`HardwareDetected`** &ndash; Diese Eigenschaft wird verwendet, um festzustellen, ob das Gerät per Fingerabdruck zu scannen unterstützt.

Die `FingerprintManager.Authenticate` Methode wird von einer Android-Anwendung verwendet, um des fingerabdruckscanners zu starten. Der folgende Codeausschnitt ist ein Beispiel, wie Sie mit der Unterstützung der Kompatibilität von APIs aufrufen:

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

Dieser Anleitung erfahren Sie, wie mit der `FingerprintManager` APIs, um eine Android-Anwendung mit der Authentifizierung per Fingerabdruck zu verbessern. Es wird beschrieben, wie Sie instanziieren und erstellen Sie eine `CryptoObject` um die Ergebnisse des fingerabdruckscanners zu schützen. Wie eine Anwendung die Unterklasse sollte untersucht `FingerprintManager.AuthenticationCallback` und reagieren auf Feedback des fingerabdruckscanners. Zum Schluss sehen, dass zum Registrieren eines Fingerabdrucks auf einem Android-Gerät oder Emulator und Verwendung **Adb** um eine Überprüfung per Fingerabdruck zu simulieren.

## <a name="requirements"></a>Anforderungen

Authentifizierung per Fingerabdruck muss Android 6.0 (API-Ebene 23) oder höher und ein Gerät mit einem Fingerabdruck-Scanner. 

Ein Fingerabdruck muss bereits mit dem Gerät für die einzelnen Benutzer registriert werden, der authentifiziert werden. Dies umfasst das Einrichten einer Bildschirmsperre, die Kennwörter, PIN, wischbewegungen Muster oder gesichtserkennung verwendet. Es ist möglich, einige der Authentifizierungsfunktionen per Fingerabdruck im Android-Emulator zu simulieren.  Weitere Informationen zu diesen zwei Themen finden Sie unter den [Registrieren eines Fingerabdrucks](enrolling-fingerprint.md) Abschnitt. 






## <a name="related-links"></a>Verwandte Links

- [Fingerabdruck-Guide-Beispiel-App](https://developer.xamarin.com/samples/monodroid/FingerprintGuide/)
- [Beispiel der Fingerprint-Dialogfeld](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Anfordern von Berechtigungen zur Laufzeit](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Fingerabdruck und Zahlungen-API (Video)](https://youtu.be/VOn7VrTRlA4)
