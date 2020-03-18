---
title: Erste Schritte mit der Fingerabdruckauthentifizierung
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/17/2018
ms.openlocfilehash: 746a096f93036e63b29bc917826259f88426cead
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020284"
---
# <a name="getting-started-with-fingerprint-authentication"></a>Erste Schritte mit der Fingerabdruckauthentifizierung

Zunächst erfahren Sie, wie Sie ein Xamarin.Android-Projekt so konfigurieren, dass die Anwendung Fingerabdruckauthentifizierung verwenden kann:

1. Aktualisieren Sie **AndroidManifest.xml**, um die Berechtigungen zu deklarieren, die die Fingerabdruck-APIs benötigen.
2. Rufen Sie einen Verweis auf `FingerprintManager` ab.
3. Überprüfen Sie, ob das Gerät eine Fingerabdrucküberprüfung ausführen kann.

## <a name="requesting-permissions-in-the-application-manifest"></a>Anfordern von Berechtigungen im Anwendungsmanifest

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Eine Android-Anwendung muss die `USE_FINGERPRINT`-Berechtigung im Manifest anfordern. Der folgende Screenshot zeigt, wie Sie der Anwendung diese Berechtigung in Visual Studio hinzufügen:

[![Bildschirm: Aktivieren der Verwendung von USE\_FINGERPRINT im Android-Manifest](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Eine Android-Anwendung muss die `USE_FINGERPRINT`-Berechtigung im Manifest anfordern. Der folgende Screenshot zeigt, wie Sie der Anwendung diese Berechtigung in Visual Studio für Mac hinzufügen:

[![Bildschirm: Aktivieren von UseFingerprint in der Android-Anwendung](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>Abrufen einer Instanz von FingerprintManager

Nun muss die Anwendung eine Instanz von `FingerprintManager` oder der `FingerprintManagerCompat`-Klasse abrufen. Damit eine Android-Anwendung mit älteren Versionen von Android kompatibel ist, sollte sie die Kompatibilitäts-APIs verwenden, die im Android Support v4 NuGet-Paket enthalten sind. Der folgende Codeausschnitt veranschaulicht, wie das entsprechende Objekt aus dem Betriebssystem abgerufen wird: 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

Im vorherigen Codeausschnitt ist der `context` ein beliebiger Android-`Android.Content.Context`. In der Regel handelt es sich hierbei um die Aktivität, die die Authentifizierung ausführt.

## <a name="checking-for-eligibility"></a>Überprüfen der Berechtigung

Eine Anwendung muss mehrere Prüfungen durchführen, um sicherzustellen, dass es möglich ist, Fingerabdruckauthentifizierung zu verwenden. Insgesamt gibt es fünf Bedingungen, die von der Anwendung verwendet werden, um die Berechtigung zu prüfen:  

**API-Ebene 23** &ndash; Die Fingerabdruck-APIs erfordern API-Ebene 23 oder höher. Die `FingerprintManagerCompat`-Klasse bindet die Überprüfung der API-Ebene für Sie ein. Aus diesem Grund empfiehlt es sich, die **Android-Supportbibliothek v4** und `FingerprintManagerCompat` zu verwenden, um diese Überprüfungen zu berücksichtigen.

**Hardware** &ndash; Wenn die Anwendung zum ersten Mal gestartet wird, sollte sie das Vorhandensein eines Fingerabdruckscanners überprüfen:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

**Gerät ist gesichert** &ndash; Der Benutzer muss das Gerät mit einer Bildschirmsperre gesichert haben. Wenn der Benutzer das Gerät nicht mit einer Bildschirmsperre gesichert hat und Sicherheit für die Anwendung wichtig ist, sollte der Benutzer benachrichtigt werden, dass eine Bildschirmsperre konfiguriert werden muss. Der folgende Codeausschnitt veranschaulicht die Überprüfung dieser Voraussetzung:

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**Registrierte Fingerabdrücke** &ndash; Der Benutzer muss über mindestens einen Fingerabdruck verfügen, der beim Betriebssystem registriert ist. Diese Berechtigungsüberprüfung sollte vor jedem Authentifizierungsversuch erfolgen:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**Berechtigungen** &ndash; Die Anwendung muss vor der Verwendung der Anwendung die Berechtigung vom Benutzer anfordern. Bei Android 5.0 oder niedriger erteilt der Benutzer die Berechtigung als Bedingung für die Installation der App. Mit Android 6.0 wurde ein neues Berechtigungsmodell eingeführt, das Berechtigungen zur Laufzeit überprüft. Dieser Codeausschnitt ist ein Beispiel für das Überprüfen von Berechtigungen unter Android 6.0:

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

Wenn alle diese Bedingungen immer überprüft werden, sobald die Anwendung Authentifizierungsoptionen anbietet, wird die beste Benutzererfahrung sichergestellt. Änderungen oder Upgrades des Geräts oder Betriebssystems können sich auf die Verfügbarkeit von Fingerabdruckauthentifizierung auswirken. Wenn Sie die Ergebnisse einer dieser Überprüfungen zwischenspeichern möchten, stellen Sie sicher, dass Sie Upgradeszenarien berücksichtigen.

Weitere Informationen zum Anfordern von Berechtigungen in Android 6.0 finden Sie im Android-Leitfaden [Anfordern von Berechtigungen zur Laufzeit](https://developer.android.com/training/permissions/requesting.html).

## <a name="related-links"></a>Verwandte Links

- [Context](xref:Android.Content.Context)
- [KeyguardManager](xref:Android.App.KeyguardManager)
- [ContextCompat](https://developer.android.com/reference/android/support/v4/content/ContextCompat)
- [FingerprintManager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Anfordern von Berechtigungen zur Laufzeit](https://developer.android.com/training/permissions/requesting.html)
