---
title: Fingerabdruckauthentifizierung
description: "Dieses Handbuch erläutert, wie Fingerabdruck Authentication, eingeführt in Android 6.0 zu einer Anwendung Xamarin.Android hinzugefügt wird."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 79f5f81e11f62359c3b951500d4ab5cbd63fb507
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="fingerprint-authentication"></a>Fingerabdruckauthentifizierung

_Dieses Handbuch erläutert, wie Fingerabdruck Authentication, eingeführt in Android 6.0 zu einer Anwendung Xamarin.Android hinzugefügt wird._


## <a name="fingerprint-authentication-overview"></a>Fingerabdruck Authentifizierung (Übersicht)

Der Eingang der Fingerabdruckscanner auf Android-Geräte bietet Anwendungen eine Alternative zum herkömmlichen Benutzername/Kennwort-Methode der Benutzerauthentifizierung. Die Verwendung von Fingerabdrücken zum Authentifizieren eines Benutzers ermöglicht es einer Anwendung, um Sicherheit zu integrieren, die weniger intrusiv als Benutzername und Kennwort ist.

Die APIs FingerprintManager Zielgeräte mit einem Fingerabdruck Scanner und API-Ebene 23 (Android 6.0) ausgeführt werden oder höher. Die APIs befinden sich in der `Android.Hardware.Fingerprints` Namespace. Die Android-Unterstützungsbibliothek v4 enthält den Fingerabdruck APIs vorgesehen für ältere Versionen von Android-Versionen. Informationen zur Kompatibilität APIs befinden sich unter der `Android.Support.v4.Hardware.Fingerprint` -Namespace über verteilt werden die [Xamarin.Android.Support.v4 NuGet-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).

Die [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (und seine Entsprechung Unterstützungsbibliothek [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) ist die primäre Klasse für die Verwendung des Fingerabdruck Hardware scannen. Diese Klasse ist eine Android-SDK-Wrapper um die Ebene Systemdienst, die Interaktionen mit der Hardware selbst verwaltet. Er ist verantwortlich für das Starten des Fingerabdruck Scanners und als Reaktion auf Feedback aus der Scanner. Diese Klasse verfügt über eine recht einfach Schnittstelle mit maximal drei Mitglieder:

* **`Authenticate`** &ndash; Diese Methode wird den Hardware-Scanner zu initialisieren und starten Sie den Dienst im Hintergrund, warten, bis des Benutzers ihren Fingerabdruck zu scannen.
* **`EnrolledFingerprints`** &ndash; Diese Eigenschaft zurück `true` , wenn der Benutzer eine oder mehrere Fingerabdrücke mit dem Gerät registriert wurde.
* **`HardwareDetected`** &ndash; Diese Eigenschaft wird verwendet, um festzustellen, ob das Gerät Fingerabdruck zu scannen unterstützt.

Die `FingerprintManager.Authenticate` Methode wird von einer Android-Anwendung zum Starten der Scanner Fingerabdruck verwendet. Der folgende Codeausschnitt zeigt ein Beispiel dazu, wie sie mit dem Kompatibilitätsgrad Unterstützungsbibliothek APIs aufzurufen:

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

Dieses Handbuch wird beschrieben, wie mithilfe der `FingerprintManager` APIs den Zugriff auf eine Android-Anwendung mit Fingerabdruckauthentifizierung zu verbessern. Es wird beschrieben, wie instanziieren und erstellen Sie eine `CryptoObject` zum Sichern der Ergebnisse vom Fingerabdruck Scanner. Untersucht, wie eine Anwendung Unterklasse sollte `FingerprintManager.AuthenticationCallback` und vom Fingerabdruck Scanner auf Feedback reagieren. Schließlich wir sehen wie Sie einen Fingerabdruck auf einem Android-Gerät oder -Emulator zu registrieren und wie Sie **Adb** limitieren Fingerabdruck zu simulieren.

## <a name="requirements"></a>Anforderungen

Fingerabdruckauthentifizierung erfordert Android 6.0 (API-Ebene 23) oder höher und ein Gerät mit einem Fingerabdruck Scanner. 

Ein Fingerabdruck muss bereits mit dem Gerät für jeden Benutzer registriert werden, die authentifiziert werden. Dies umfasst das Einrichten einer Bildschirmsperre, die Kennwörter, PIN, Wischen Muster oder gesichtserkennung verwendet. Es ist möglich, einige der Fingerabdruck Authentifizierungsfunktionen in einem Android-Emulator zu simulieren.  Weitere Informationen zu diesen beiden Themen finden Sie unter der [registrieren einen Fingerabdruck](enrolling-fingerprint.md) Abschnitt. 






## <a name="related-links"></a>Verwandte Links

- [Fingerabdruck Handbuch Beispiel-App](https://developer.xamarin.com/samples/monodroid/FingerprintGuide/)
- [Fingerabdruck Dialogfeld-Beispiel](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [Anfordern von Berechtigungen zur Laufzeit](http://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](http://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Fingerabdruck und Zahlungen-API (Video)](https://youtu.be/VOn7VrTRlA4)
