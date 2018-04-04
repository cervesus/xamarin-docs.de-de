---
title: Erste Schritte mit Fingerabdruckauthentifizierung
ms.prod: xamarin
ms.assetid: 7BACCB36-8E3E-4E5D-B8EF-56A639839FD2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 70d27ef3d7518619a246c25aac128b2fd1ed70c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-fingerprint-authentication"></a>Erste Schritte mit Fingerabdruckauthentifizierung

Um zu beginnen wir zunächst wird beschrieben, wie ein Xamarin.Android-Projekt so konfigurieren, dass die Anwendung Fingerabdruckauthentifizierung verwenden kann:

1. Update **AndroidManifest.xml** um die Berechtigungen zu deklarieren, die den Fingerabdruck-APIs erfordern.
2. Rufen Sie einen Verweis auf die `FingerprintManager`.
3. Überprüfen Sie, dass das Gerät kann Fingerabdruck zu scannen.

## <a name="requesting-permissions-in-the-application-manifest"></a>Anfordernde von Berechtigungen in der Anwendung Manifest

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Eine Android-Anwendung anfordern muss die `USE_FINGERPRINT` -Berechtigung für das Manifest. Der folgende Screenshot zeigt, wie diese Berechtigung für die Anwendung in Visual Studio 2015 hinzuzufügen:

[![Aktivieren der Verwendung\_FINGERABDRUCK in der Android-Manifest-Bildschirm](get-started-images/fingerprint-01-vs.png)](get-started-images/fingerprint-01-vs.png#lightbox) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Eine Android-Anwendung anfordern muss die `USE_FINGERPRINT` -Berechtigung für das Manifest. Der folgende Screenshot zeigt, wie die Anwendung in Visual Studio für Mac durch diese Berechtigung hinzugefügt wird:

[![Aktivieren UseFingerprint auf Android-Anwendung](get-started-images/fingerprint-01-xs.png)](get-started-images/fingerprint-01-xs.png#lightbox) 

-----

## <a name="getting-an-instance-of-the-fingerprintmanager"></a>Abrufen einer Instanz von der FingerprintManager

Als Nächstes muss die Anwendung ruft eine Instanz von der `FingerprintManager` oder `FingerprintManagerCompat` Klasse. Mit älteren Versionen von Android kompatibel ist, sollte eine Android-Anwendung verwenden Sie die API-Kompatibilität in der Android-Unterstützung v4-NuGet-Paket gefunden. Der folgende Codeausschnitt veranschaulicht, wie das entsprechende Objekt aus dem Betriebssystem abrufen: 

```csharp
// Using the Android Support Library v4
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);

// Using API level 23:
FingerprintManager fingerprintManager = context.GetSystemService(Context.FingerprintService) as FingerprintManager;
```  

Im vorherigen Codeausschnitt die `context` stellt alle Android `Android.Content.Context`. In der Regel ist dies der Aktivität, die die Authentifizierung durchführt.

## <a name="checking-for-eligibility"></a>Für die Berechtigung überprüfen

Eine Anwendung durchzuführenden verschiedene Überprüfungen aus, um sicherzustellen, dass es möglich ist, Fingerabdruckauthentifizierung verwenden. Insgesamt stehen Ihnen fünf Bedingungen, der mit die Anwendung für die Berechtigung:  
 

**API-Ebene 23** &ndash; der Fingerabdruck-APIs erfordern die API-Ebene 23 oder höher. Die `FingerprintManagerCompat` Klasse wird die Überprüfung der API für Sie zu umschließen. Aus diesem Grund wird empfohlen, um eine verwenden die **Android Unterstützungsbibliothek v4** und `FingerprintManagerCompat`; dabei wird berücksichtigt, die auf das Verzeichnis der Überprüfungen.

**Hardware** &ndash; Wenn die Anwendung zum ersten Mal gestartet wird, sollten Sie das Vorhandensein eines Scanners Fingerabdruck überprüfen:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.IsHardwareDetected)
{
    // Code omitted
}
```
    
**Gerät wird gesichert** &ndash; muss der Benutzer das Gerät mit einem Bildschirmsperre gesichert haben. Wenn der Benutzer das Gerät mit einem Bildschirmsperre nicht gesichert hat und Sicherheit für die Anwendung von Bedeutung ist, sollte der Benutzer benachrichtigt, dass eine bildschirmsperren konfiguriert werden muss. Der folgende Codeausschnitt zeigt, wie diese Pre-Requiste überprüfen:

```csharp
KeyguardManager keyguardManager = (KeyguardManager) GetSystemService(KeyguardService);
if (!keyguardManager.IsKeyguardSecure)
{
}
```

**Registriert die Fingerabdrücke** &ndash; der Benutzer muss über mindestens ein Fingerabdruck mit dem Betriebssystem registriert haben. Diese berechtigungsüberprüfung sollte vor jedem Authentifizierungsversuch auftreten:

```csharp
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
if (!fingerprintManager.HasEnrolledFingerprints)
{
    // Can't use fingerprint authentication - notify the user that they need to
    // enroll at least one fingerprint with the device.
}
```

**Berechtigungen** &ndash; die Anwendung muss eine Berechtigung anfordern, vom Benutzer vor der Verwendung der Anwendung. Für Android 5.0 und untere erhält der Benutzer die Berechtigung als Bedingung für die Installation der app. Android 6.0 eingeführt, ein neues Berechtigungsmodell, das Berechtigungen zur Laufzeit überprüft wird. Dieser Codeausschnitt ist ein Beispiel zum Überprüfen von Berechtigungen für Android 6.0:

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
    // http://developer.android.com/training/permissions/requesting.html
}
```

Eine Anwendung sollte Bedingungen 3, 4 und 5 jedes Mal überprüfen umgebungsänderungen er Fingerabdruckauthentifizierung verwenden. Die ersten beiden Bedingungen können überprüft werden, die ersten Mal eine Anwendung ausgeführt wird, auf einem Gerät und die Ergebnisse gespeichert (in freigegebene Voreinstellungen z. B.).

Weitere Informationen zum Anfrageberechtigungen in Android 6.0 finden Sie in der Android-Handbuch [Anfordern von Berechtigungen zur Laufzeit](http://developer.android.com/training/permissions/requesting.html).



## <a name="related-links"></a>Verwandte Links

- [Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [ContextCompat](https://developer.xamarin.com/api/type/Android.Support.V4.Content.ContextCompat/)
- [KeyguardManager](https://developer.xamarin.com/api/type/Android.App.KeyguardManager/)
- [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)
- [FingerprintManagerCompat](http://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)
- [Anfordern von Berechtigungen zur Laufzeit](http://developer.android.com/training/permissions/requesting.html)
