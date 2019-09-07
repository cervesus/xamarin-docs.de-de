---
title: Fingerabdruckauthentifizierung
description: In diesem Handbuch wird erläutert, wie Sie die Fingerabdruckauthentifizierung, die in Android 6,0 eingeführt wurde, in eine xamarin. Android-Anwendung einfügen.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: a58242e89033d6cd2652495f9466379f63f498f0
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761237"
---
# <a name="fingerprint-authentication"></a>Fingerabdruckauthentifizierung

_In diesem Handbuch wird erläutert, wie Sie die Fingerabdruckauthentifizierung, die in Android 6,0 eingeführt wurde, in eine xamarin. Android-Anwendung einfügen._

## <a name="fingerprint-authentication-overview"></a>Übersicht über Fingerabdruckauthentifizierung

Die Ankunft von Fingerabdruckscannern auf Android-Geräten bietet Anwendungen eine Alternative zur herkömmlichen Benutzernamen-/Kennwort-Methode der Benutzerauthentifizierung. Die Verwendung von Fingerabdrücken zum Authentifizieren eines Benutzers ermöglicht einer Anwendung die Einbindung von Sicherheit, die weniger stark aufdringlich ist als ein Benutzername und ein Kennwort.

Die fingerprintmanager-APIs richten sich an Geräte mit einem Fingerabdruckscanner und führen die API-Ebene 23 (Android 6,0) oder höher aus. Die APIs finden Sie im `Android.Hardware.Fingerprints` -Namespace. Die Android-Unterstützungs Bibliothek V4 stellt Versionen der Fingerabdruck-APIs bereit, die für ältere Android-Versionen vorgesehen sind. Die Kompatibilitäts-APIs befinden sich `Android.Support.v4.Hardware.Fingerprint` im-Namespace, werden über das [xamarin. Android. Support. v4-nuget-Paket](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)verteilt.

Der [fingerprintmanager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (und das zugehörige Unterstützungs Bibliotheks Pendant, [fingerprintmanagercompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) ist die primäre Klasse für die Verwendung der Fingerabdruck-Scan Hardware. Bei dieser Klasse handelt es sich um einen Android SDK Wrapper um den systemebenendienst, der Interaktionen mit der Hardware selbst verwaltet. Er ist verantwortlich für das Starten des Fingerabdruckscanners und das reagieren auf Feedback des Scanners. Diese Klasse verfügt über eine relativ unkomplizierte Schnittstelle mit nur drei Membern:

- **`Authenticate`** &ndash; Mit dieser Methode wird die Hardware Überprüfung initialisiert und der Dienst im Hintergrund gestartet, und es wird darauf gewartet, dass der Benutzer den Fingerabdruck scannt.
- **`EnrolledFingerprints`** Diese Eigenschaft gibt zurück `true` , wenn der Benutzer mindestens einen Fingerabdruck beim Gerät registriert hat. &ndash;
- **`HardwareDetected`** &ndash; Diese Eigenschaft wird verwendet, um zu bestimmen, ob das Gerät Fingerabdruck Scans unterstützt.

Die `FingerprintManager.Authenticate` -Methode wird von einer Android-Anwendung verwendet, um den Fingerabdruckscanner zu starten. Der folgende Code Ausschnitt ist ein Beispiel dafür, wie Sie ihn mit den Kompatibilitäts-APIs der Unterstützungs Bibliothek aufrufen:

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

In dieser Anleitung wird erläutert, wie Sie `FingerprintManager` die APIs verwenden, um eine Android-Anwendung mit Fingerabdruckauthentifizierung zu verbessern. Es wird erläutert, wie eine `CryptoObject` instanziiert und erstellt wird, um die Ergebnisse des Fingerabdruckscanners zu sichern. Wir untersuchen, wie eine Anwendung die unter `FingerprintManager.AuthenticationCallback` Klasse Unternehmen und Feedback vom Fingerabdruckscanner beantworten kann. Abschließend erfahren Sie, wie Sie einen Fingerabdruck auf einem Android-Gerät oder-Emulator registrieren und mit **ADB** einen Fingerabdruck Scan simulieren.

## <a name="requirements"></a>Anforderungen

Die Fingerabdruckauthentifizierung erfordert Android 6,0 (API-Ebene 23) oder höher und ein Gerät mit einem Fingerabdruckscanner. 

Für jeden Benutzer, der authentifiziert werden soll, muss bereits ein Fingerabdruck für das Gerät registriert sein. Dies umfasst das Einrichten einer Bildschirmsperre, die ein Kennwort, eine PIN, ein wischen-Muster oder eine Gesichtserkennung verwendet. Es ist möglich, einige der Fingerabdruck Authentifizierungsfunktionen in einem Android-Emulator zu simulieren.  Weitere Informationen zu diesen beiden Themen finden Sie im Abschnitt zum [Anmelden eines Fingerabdrucks](enrolling-fingerprint.md) . 

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App für Fingerabdruck Handbuch](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fingerprintguide)
- [Beispiel für Fingerabdruck](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [Anfordern von Berechtigungen zur Laufzeit](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](xref:Android.Content.Context)
- [Fingerabdruck-und Zahlungs-API (Video)](https://youtu.be/VOn7VrTRlA4)
