---
title: Ersten Einstieg in die Fingerabdruckauthentifizierung
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/17/2018
ms.openlocfilehash: 746a096f93036e63b29bc917826259f88426cead
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020284"
---
# <a name="getting-started-with-fingerprint-authentication"></a>Ersten Einstieg in die Fingerabdruckauthentifizierung

Zunächst erfahren Sie, wie Sie ein xamarin. Android-Projekt so konfigurieren, dass die Anwendung die Fingerabdruckauthentifizierung verwenden kann:

1. Aktualisieren Sie " **androidmanifest. XML** ", um die Berechtigungen für die Fingerabdruck-APIs zu deklarieren.
2. Rufen Sie einen Verweis auf den `FingerprintManager`ab.
3. Überprüfen Sie, ob das Gerät Fingerabdruck Scannen kann.

## <a name="requesting-permissions-in-the-application-manifest"></a>Anfordern von Berechtigungen im Anwendungs Manifest

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Eine Android-Anwendung muss die `USE_FINGERPRINT`-Berechtigung im Manifest anfordern. Der folgende Screenshot zeigt, wie Sie diese Berechtigung der Anwendung in Visual Studio hinzufügen:

[![Aktivieren der Verwendung von\_Fingerabdruck im Android-Manifest-Bildschirm](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Eine Android-Anwendung muss die `USE_FINGERPRINT`-Berechtigung im Manifest anfordern. Der folgende Screenshot zeigt, wie Sie diese Berechtigung der Anwendung in Visual Studio für Mac hinzufügen:

[![Aktivieren von usefinger Abdruck auf dem Android-Anwendungsbildschirm](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>Eine Instanz von fingerprintmanager wird aufgerufen

Als nächstes muss die Anwendung eine Instanz des `FingerprintManager` oder der `FingerprintManagerCompat`-Klasse erhalten. Damit eine Android-Anwendung mit älteren Versionen von Android kompatibel ist, sollte Sie die Kompatibilitäts-APIs verwenden, die im Android Support V4 nuget-Paket enthalten sind. Der folgende Code Ausschnitt veranschaulicht, wie Sie das entsprechende-Objekt vom Betriebssystem erhalten: 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

Im vorherigen Code Ausschnitt ist die `context` eine beliebige Android-`Android.Content.Context`. In der Regel handelt es sich hierbei um die Aktivität, die die Authentifizierung ausführt.

## <a name="checking-for-eligibility"></a>Überprüfen der Berechtigung

Eine Anwendung muss mehrere Prüfungen durchführen, um sicherzustellen, dass es möglich ist, die Fingerabdruckauthentifizierung zu verwenden. Insgesamt gibt es fünf Bedingungen, die von der Anwendung verwendet werden, um die Berechtigung zu prüfen:  

**API-Ebene 23** &ndash; den Fingerabdruck-APIs ist API-Ebene 23 oder höher erforderlich. Die `FingerprintManagerCompat`-Klasse packt die Überprüfung auf API-Ebene für Sie. Aus diesem Grund empfiehlt es sich, die **Android-Unterstützungs Bibliothek V4** und `FingerprintManagerCompat`zu verwenden; Dadurch wird eine dieser Überprüfungen berücksichtigt.

**Hardware** &ndash; wenn die Anwendung zum ersten Mal gestartet wird, sollte überprüft werden, ob ein Fingerabdruckscanner vorhanden ist:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```

Das **Gerät ist geschützt** , &ndash; der Benutzer das Gerät mit einer Bildschirmsperre gesichert haben muss. Wenn der Benutzer das Gerät nicht mit einer Bildschirmsperre gesichert hat und die Sicherheit für die Anwendung wichtig ist, sollte der Benutzer benachrichtigt werden, dass eine Bildschirmsperre konfiguriert werden muss. Der folgende Code Ausschnitt zeigt, wie Sie diese Voraussetzungen überprüfen:

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

Registrierte **Fingerabdrücke** &ndash; für den Benutzer muss mindestens ein Fingerabdruck beim Betriebssystem registriert sein. Diese Berechtigungsüberprüfung sollte vor jedem Authentifizierungs Versuch erfolgen:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**Berechtigungen** &ndash; die Anwendung muss vor der Verwendung der Anwendung eine Berechtigung vom Benutzer anfordern. Bei Android 5,0 und niedriger erteilt der Benutzer die Berechtigung als Bedingung für die Installation der app. Android 6,0 hat ein neues Berechtigungs Modell eingeführt, das Berechtigungen zur Laufzeit überprüft. Dieser Code Ausschnitt ist ein Beispiel für das Überprüfen von Berechtigungen für Android 6,0:

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

Wenn alle diese Bedingungen überprüft werden, sobald die Anwendung Authentifizierungs Optionen anbietet, wird sichergestellt, dass der Benutzer die beste Benutzer Leistung erhält. Änderungen oder Upgrades Ihres Geräts oder Betriebssystems können sich auf die Verfügbarkeit der Fingerabdruckauthentifizierung auswirken. Wenn Sie die Ergebnisse einer dieser Überprüfungen zwischenspeichern möchten, stellen Sie sicher, dass Sie Upgradeszenarien berücksichtigen.

Weitere Informationen zum Anfordern von Berechtigungen in Android 6,0 finden Sie im Android Guide [anfordern von Berechtigungen zur Laufzeit](https://developer.android.com/training/permissions/requesting.html).

## <a name="related-links"></a>Verwandte Links

- [Kontext](xref:Android.Content.Context)
- [Keyguardmanager](xref:Android.App.KeyguardManager)
- [Contextcompat](https://developer.android.com/reference/android/support/v4/content/ContextCompat)
- [Fingerprintmanager](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [Fingerprintmanagercompat](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Anfordern von Berechtigungen zur Laufzeit](https://developer.android.com/training/permissions/requesting.html)
