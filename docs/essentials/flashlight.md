---
title: 'Xamarin.Essentials: Taschenlampe'
description: In diesem Dokument wird die Klasse „Flashlight“ in Xamarin.Essentials beschrieben, mit der Sie das Blitzlicht der Gerätekamera aktivieren bzw. deaktivieren, um es als Taschenlampe einzusetzen.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 3f8e9e2e8608467367af6523c8b89bd8f757d491
ms.sourcegitcommit: 01f93a34b466f8d4043cef68fab9b35cd8decee6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899056"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials: Taschenlampe

Mit der Klasse **Flashlight** können Sie das Blitzlicht der Gerätekamera aktivieren bzw. deaktivieren, um es als Taschenlampe einzusetzen.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

Für den Zugriff auf die **Taschenlampen**-Funktion ist die folgende plattformspezifische Einrichtung erforderlich.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die Berechtigungen „Flashlight“ und „Camera“ sind erforderlich und müssen im Android-Projekt konfiguriert werden. Das Hinzufügen erfolgt folgendermaßen:

Öffnen Sie die Datei **AssemblyInfo.cs** im Ordner **Eigenschaften** und fügen Sie Folgendes hinzu:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

Alternativ können Sie das Android-Manifest aktualisieren:

Öffnen Sie die Datei **AndroidManifest.xml** im Ordner **Eigenschaften**, und fügen Sie Folgendes im Knoten **Manifest** hinzu.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Alternativ können Sie mit der rechten Maustaste auf das Android-Projekt klicken und die Eigenschaften des Projekts öffnen. Suchen Sie unter **Android-Manifest** den Bereich **Erforderliche Berechtigungen:**, und aktivieren Sie die Berechtigungen **FLASHLIGHT** (Taschenlampe) und **CAMERA** (Kamera). Dadurch wird die Datei **AndroidManifest.xml** automatisch aktualisiert.

Durch das Hinzufügen dieser Berechtigungen [filtert Google Play automatisch die Geräte heraus](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features), die keine spezifische Hardware aufweisen. Sie können dies umgehen, indem Sie Folgendes der Datei „AssemblyInfo.cs“ in Ihrem Android-Projekt hinzufügen:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Es ist kein zusätzliches Setup erforderlich.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Es ist kein zusätzliches Setup erforderlich.

-----

## <a name="using-flashlight"></a>Verwenden der Taschenlampe

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die Taschenlampe kann mit den Methoden `TurnOnAsync` und `TurnOffAsync` aktiviert bzw. deaktiviert werden:

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

### <a name="androidtabandroid"></a>[Android](#tab/android)

Die Klasse „Flashlight“ wurde auf Basis des Betriebssystems des Geräts optimiert.

#### <a name="api-level-23-and-higher"></a>API-Ebene 23 und höher

Auf neueren API-Ebenen wird der [TorchMode](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) (Taschenlampenmodus) verwendet, um die Blitzlichteinheit des Geräts zu aktivieren bzw. zu deaktivieren.

#### <a name="api-level-22-and-lower"></a>API-Ebene 22 und niedriger

Zum Aktivieren oder Deaktivieren des `FlashMode` (Blitzlichtmodus) der Kameraeinheit wird eine Kameraoberflächentextur erstellt. 

### <a name="iostabios"></a>[iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) wird verwendet, um den Taschenlampen- und Blitzlichtmodus des Geräts zu aktivieren bzw. zu deaktivieren.

### <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[Lamp](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp) wird verwendet, um die erste Lampe auf der Rückseite des Geräts zu erkennen und zu aktivieren bzw. zu deaktivieren.

-----

## <a name="api"></a>API

- [Flashlight-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Flashlight-API-Dokumentation](xref:Xamarin.Essentials.Flashlight)
