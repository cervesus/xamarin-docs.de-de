---
title: Fingerabdruckauthentifizierung
description: In diesem Leitfaden wird erläutert, wie Sie Fingerabdruckauthentifizierung, die in Android 6.0 eingeführt wurde, in eine Xamarin.Android-Anwendung einfügen.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 4a4b6ee7a123683a9d5a140c46c0b3542767ffa3
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027525"
---
# <a name="fingerprint-authentication"></a>Fingerabdruckauthentifizierung

_In diesem Leitfaden wird erläutert, wie Sie Fingerabdruckauthentifizierung, die in Android 6.0 eingeführt wurde, in eine Xamarin.Android-Anwendung einfügen._

## <a name="fingerprint-authentication-overview"></a>Fingerabdruckauthentifizierung: Übersicht

Die Einführung von Fingerabdruckscannern auf Android-Geräten bietet Anwendungen eine Alternative zur herkömmlichen Methode der Benutzerauthentifizierung über den Benutzernamen und ein Kennwort. Die Verwendung von Fingerabdrücken zum Authentifizieren eines Benutzers ermöglicht einer Anwendung die Einbindung von Sicherheit, die weniger aufdringlich ist als ein Benutzername und ein Kennwort.

Die FingerprintManager-APIs sind für Geräte mit einem Fingerabdruckscanner vorgesehen, die die API-Ebene 23 (Android 6.0) oder höher ausführen. Die APIs befinden sich im `Android.Hardware.Fingerprints`-Namespace. Die Android-Unterstützungsbibliothek v4 stellt auch Versionen der Fingerabdruck-APIs bereit, die für ältere Android-Versionen vorgesehen sind. Die Kompatibilitäts-APIs befinden sich im `Android.Support.v4.Hardware.Fingerprint`-Namespace und werden über das [NuGet-Paket Xamarin.Android.Support.v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) verteilt.

[FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (und das Unterstützungsbibliothekspendant [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) ist die primäre Klasse für die Verwendung der Fingerabdruck-Überprüfungshardware. Bei dieser Klasse handelt es sich um einen Android SDK-Wrapper um den Dienst auf Systemebene, der Interaktionen mit der Hardware selbst verwaltet. Er ist verantwortlich für das Starten des Fingerabdruckscanners und das Reagieren auf Feedback vom Scanner. Diese Klasse verfügt über eine relativ unkomplizierte Schnittstelle mit nur drei Membern:

- **`Authenticate`** &ndash; Mit dieser Methode wird der Hardwarescanner initialisiert und der Dienst im Hintergrund gestartet, und es wird darauf gewartet, dass der Benutzer seinen Fingerabdruck scannt.
- **`EnrolledFingerprints`** &ndash; Diese Eigenschaft gibt `true` zurück, wenn der Benutzer mindestens einen Fingerabdruck beim Gerät registriert hat.
- **`HardwareDetected`** &ndash; Diese Eigenschaft wird verwendet, um zu bestimmen, ob das Gerät Fingerabdrucküberprüfungen unterstützt.

Die `FingerprintManager.Authenticate`-Methode wird von einer Android-Anwendung verwendet, um den Fingerabdruckscanner zu starten. Der folgende Codeausschnitt ist ein Beispiel dafür, wie Sie ihn mit den Kompatibilitäts-APIs der Unterstützungsbibliothek aufrufen:

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

In diesem Leitfaden wird erläutert, wie Sie die `FingerprintManager`-APIs verwenden, um eine Android-Anwendung durch Fingerabdruckauthentifizierung zu optimieren. Es wird erläutert, wie ein `CryptoObject` instanziiert und erstellt wird, um die Ergebnisse des Fingerabdruckscanners zu sichern. Wir untersuchen, wie eine Anwendung `FingerprintManager.AuthenticationCallback`-Unterklassen erstellt und auf Feedback des Fingerabdruckscanners reagiert. Abschließend erfahren Sie, wie Sie einen Fingerabdruck auf einem Android-Gerät oder einem Emulator registrieren und **ADB** verwenden, um eine Fingerabdrucküberprüfung zu simulieren.

## <a name="requirements"></a>Anforderungen

Die Fingerabdruckauthentifizierung erfordert Android 6.0 (API-Ebene 23) oder höher und ein Gerät mit einem Fingerabdruckscanner. 

Für jeden Benutzer, der authentifiziert werden soll, muss bereits ein Fingerabdruck für das Gerät registriert sein. Dies umfasst das Einrichten einer Bildschirmsperre, die ein Kennwort, eine PIN, ein Wischmuster oder Gesichtserkennung verwendet. Es ist möglich, einige der Fingerabdruck-Authentifizierungsfunktionen in einem Android-Emulator zu simulieren.  Weitere Informationen zu diesen beiden Themen finden Sie im Abschnitt [Registrieren eines Fingerabdrucks](enrolling-fingerprint.md). 

## <a name="related-links"></a>Verwandte Links

- [Beispiel-App für Fingerabdruckleitfaden](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fingerprintguide)
- [Beispiel für Fingerabdruck-Dialogfeld](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [Anfordern von Berechtigungen zur Laufzeit](https://developer.android.com/training/permissions/requesting.html)
- [android.hardware.fingerprint](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [android.support.v4.hardware.fingerprint](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android.Content.Context](xref:Android.Content.Context)
- [Fingerabdruck- und Zahlungs-API (Video)](https://youtu.be/VOn7VrTRlA4)
