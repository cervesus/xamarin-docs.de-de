---
title: Erste Schritte mit Authentifizierung per Fingerabdruck
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/17/2018
ms.openlocfilehash: 3082dfcd6d0ffbc6404a89a10819e60b57b9c61c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023810"
---
# <a name="getting-started-with-fingerprint-authentication"></a>Erste Schritte mit Authentifizierung per Fingerabdruck

Zunächst lassen Sie uns zunächst wird beschrieben, wie ein Xamarin.Android-Projekt so konfigurieren, dass die Anwendung die Authentifizierung per Fingerabdruck zu verwenden ist:

1. Update **"androidmanifest.xml"** , deklarieren Sie die Berechtigungen, die den Fingerabdruck-APIs erfordern.
2. Rufen Sie einen Verweis auf die `FingerprintManager`.
3. Überprüfen Sie, dass das Gerät kann per Fingerabdruck zu scannen.

## <a name="requesting-permissions-in-the-application-manifest"></a>Anfordernde von Berechtigungen in der Anwendung Manifest

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Eine Android-Anwendung muss Anfordern der `USE_FINGERPRINT` -Berechtigung für das Manifest. Der folgende Screenshot zeigt, wie Sie diese Berechtigung für die Anwendung in Visual Studio hinzufügen:

[![Aktivieren der Verwendung\_FINGERABDRUCK auf dem Android-Manifest](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Eine Android-Anwendung muss Anfordern der `USE_FINGERPRINT` -Berechtigung für das Manifest. Der folgende Screenshot zeigt, wie Sie diese Berechtigung für die Anwendung in Visual Studio für Mac hinzufügen:

[![Aktivieren der UseFingerprint auf dem Android-Anwendung](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>Abrufen einer Instanz der dem FingerprintManager

Als Nächstes muss die Anwendung ruft eine Instanz von der `FingerprintManager` oder `FingerprintManagerCompat` Klasse. Mit älteren Versionen von Android-Versionen kompatibel ist, sollten eine Android-Anwendung verwenden, die API-Kompatibilität in der Android-Unterstützung v4-NuGet-Paket gefunden. Der folgende Codeausschnitt zeigt, wie das entsprechende Objekt aus dem Betriebssystem abgerufen wird: 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

Im vorherigen Codeausschnitt der `context` stellt alle Android `Android.Content.Context`. In der Regel ist dies die Aktivität, die die Authentifizierung durchführt.

## <a name="checking-for-eligibility"></a>Berechtigung prüfen

Eine Anwendung muss mehrere Überprüfungen, um sicherzustellen, dass es möglich ist, verwenden Sie die Authentifizierung per Fingerabdruck durchführen. Es gibt fünf Bestimmungen, die die Anwendung zum Überprüfen verwendet, für die Berechtigung, insgesamt:  

**API-Ebene 23** &ndash; der Fingerprint-APIs erfordern die API-Ebene 23 oder höher. Die `FingerprintManagerCompat` -Klasse wird die Überprüfung der API für Sie zu umschließen. Aus diesem Grund wird empfohlen, verwenden Sie die **Android Support-Bibliothek v4** und `FingerprintManagerCompat`; dies für den Endpunkt der Überprüfungen berücksichtigt wird.

**Hardware** &ndash; , wenn die Anwendung zum ersten Mal wird gestartet, sollten Sie das Vorhandensein einer fingerabdruckscanners überprüfen:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**Gerät wird gesichert** &ndash; muss der Benutzer das Gerät mit einem Bildschirmsperre gesichert haben. Wenn der Benutzer das Gerät mit einem bildschirmsperren nicht gesichert hat und die Sicherheit wichtig für die Anwendung ist, sollte der Benutzer benachrichtigt werden, dass eine bildschirmsperren konfiguriert werden muss. Der folgende Codeausschnitt zeigt, wie Sie diese vor – da dies zu überprüfen:

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**Registrierte Fingerabdrücke** &ndash; der Benutzer muss über mindestens einen Fingerabdruck, die mit dem Betriebssystem registriert haben. Diese Überprüfung der Berechtigung muss vor jedem Authentifizierungsversuch auftreten:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**Berechtigungen** &ndash; die Anwendung muss Berechtigung vom Benutzer anfordern, bevor Sie mit der Anwendung. Für Android 5.0 und niedriger erhält der Benutzer die Berechtigung als Bedingung für die app zu installieren. Android 6.0 eingeführt, ein neues Berechtigungsmodell, das Berechtigungen zur Laufzeit überprüft. Dieser Codeausschnitt ist ein Beispiel zum Überprüfen von Berechtigungen auf Android 6.0:

```csharp
// The context is typically a reference to the current activity.
Android.Content.PM.Permission permissionResult = ContextCompat.CheckSelfPermission(context, Manifest.Permission.UseFingerprint);
if (permissionResult == Android.Content.PM.Permission.Granted)
{
    // Permission granted - go ahead and start the fingerprint scanner.
}
else
{
    // No permission. Go and ask for permissions and don't start the scanner. See
    // https://developer.android.com/training/permissions/requesting.html
}
```

Überprüfen alle diese Bedingungen jedes Mal Authentifizierungsoptionen sicherzustellen, dass der Benutzer die bestmögliche benutzererfahrung erhält die Anwendung bietet. Änderungen oder Upgrades für ihre Gerät oder Betriebssystem beeinträchtigt die Verfügbarkeit der Authentifizierung per Fingerabdruck. Wenn Sie die Ergebnisse von dieser Überprüfungen zwischenspeichern möchten, sicher für Aktualisierungsszenarien Formfaktoren zur Verfügung.

Weitere Informationen dazu, wie Berechtigungen anfordern, in Android 6.0 finden Sie in der Android-Anleitung [Anfordern von Berechtigungen zur Laufzeit](https://developer.android.com/training/permissions/requesting.html).

## <a name="related-links"></a>Verwandte Links

- [Kontext](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Anfordern von Berechtigungen zur Laufzeit](https://developer.android.com/training/permissions/requesting.html)
