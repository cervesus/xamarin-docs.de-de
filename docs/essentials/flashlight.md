---
title: 'Xamarin.Essentials: Taschenlampe'
description: Dieses Dokument beschreibt die Taschenlampe-Klasse in Xamarin.Essentials, mit der Möglichkeit zum Aktivieren oder deaktivieren Sie das Gerät die Kamera flash in eine Taschenlampe umzuwandeln.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 8c471f64c14a2e41693c450e02f89e7ac845d060
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353359"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials: Taschenlampe

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **Taschenlampe** -Klasse verfügt über die Möglichkeit, aktivieren oder deaktivieren Sie das Gerät die Kamera flash in eine Taschenlampe umzuwandeln.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Taschenlampe** Funktionalität die folgende Plattform Einrichtung erforderlich ist.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die Taschenlampe und Kamera Berechtigungen sind erforderlich und müssen in der Android-Projekt konfiguriert werden. Dies kann auf folgende Weise hinzugefügt werden:

Öffnen der **"AssemblyInfo.cs"** -Datei unter dem **Eigenschaften** Ordner und hinzufügen:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

ODER Android-Manifest aktualisieren:

Öffnen der **"androidmanifest.xml"** -Datei unter dem **Eigenschaften** Ordner und fügen Sie Folgendes in der die **manifest** Knoten.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Oder klicken Sie mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** finden Sie die **erforderliche Berechtigungen:** Bereichs- und überprüfen Sie die **TASCHENLAMPE** und **KAMERA** Berechtigungen. Dadurch wird automatisch aktualisiert. die **"androidmanifest.xml"** Datei.

Durch diese Berechtigungen [Google Play automatisch Geräte herausgefiltert](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) ohne spezielle Hardware. Sie können dies umgehen, indem Sie Folgendes der Datei "AssemblyInfo.cs" in Ihrem Android-Projekt hinzufügen:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Ohne zusätzliche Einrichtung erforderlich.

-----

## <a name="using-flashlight"></a>Verwenden von Taschenlampe

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die Taschenlampe aktiviert werden kann oder über die `TurnOnAsync` und `TurnOffAsync` Methoden:

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

## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

### <a name="androidtabandroid"></a>[Android](#tab/android)

Die Klasse Taschenlampe wurde optimiert, je nach Betriebssystem des Geräts.

#### <a name="api-level-23-and-higher"></a>API-Ebene 23 oder höher

Auf neueren API-Ebenen [Torch Modus](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) aktivieren oder Deaktivieren der Flash-Einheit des Geräts verwendet werden.

#### <a name="api-level-22-and-lower"></a>API-Ebene 22 und niedriger

Eine Kamera Oberfläche Textur erstellt, um das Aktivieren oder Deaktivieren der `FlashMode` der Kamera Einheit. 

### <a name="iostabios"></a>[iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) wird verwendet, um ein-und Ausschalten der Torch und Flash-Modus des Geräts.

### <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[Lamp](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) verwendet, um den ersten Lamp auf der Rückseite des Geräts aktivieren oder deaktivieren zu erkennen.

-----

## <a name="api"></a>API

- [Taschenlampe-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Taschenlampe-API-Dokumentation](xref:Xamarin.Essentials.Flashlight)
