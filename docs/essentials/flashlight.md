---
title: 'Xamarin.Essentials: Taschenlampe'
description: Dieses Dokument beschreibt die Klasse Taschenlampe in Xamarin.Essentials, besitzt die Möglichkeit, aktivieren oder Deaktivieren des Geräts Kamera flash, um ihn in eine Taschenlampe zu aktivieren.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: a5c559653bff38c692f0b1d881d5d8f4cac3d383
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782423"
---
# <a name="xamarinessentials-flashlight"></a>Xamarin.Essentials: Taschenlampe

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **Taschenlampe** -Klasse verfügt über die Möglichkeit, aktivieren oder Deaktivieren des Geräts Kamera flash, um ihn in eine Taschenlampe zu aktivieren.

## <a name="getting-started"></a>Erste Schritte

Für den Zugriff auf die **Taschenlampe** Funktionen die folgenden spezifischen Plattform-Setup ist erforderlich.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Die Taschenlampe und Kamera Berechtigungen sind erforderlich und müssen in der Android-Projekt konfiguriert werden. Dies kann auf folgende Weise hinzugefügt werden:

Öffnen der **AssemblyInfo.cs** Datei unter dem **Eigenschaften** Ordner und hinzufügen:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

ODER Android-Manifest aktualisieren:

Öffnen der **AndroidManifest.xml** Datei unter dem **Eigenschaften** Ordner und fügen Sie die folgenden innerhalb eines der **manifest** Knoten.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Oder klicken Sie mit der rechten Maustaste auf das Android-Projekt, und öffnen Sie die Eigenschaften des Projekts. Unter **Android-Manifest** Suchen der **die erforderlichen Berechtigungen verfügen:** Bereichs- und überprüfen Sie die **TASCHENLAMPE** und **KAMERA** Berechtigungen. Dadurch wird automatisch aktualisiert die **AndroidManifest.xml** Datei.

Durch Hinzufügen der folgenden Berechtigungen [Google Play automatisch herausgefiltert Geräte](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) ohne spezielle Hardware. Sie können dies umgehen, indem Sie Folgendes in die Datei AssemblyInfo.cs in Ihrem Android-Projekt hinzufügen:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Ohne zusätzliche Einrichtung erforderlich.

# <a name="uwptabuwp"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp)

Ohne zusätzliche Einrichtung erforderlich.

-----

## <a name="using-flashlight"></a>Verwenden von Taschenlampe

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Leuchten aktiviert werden kann oder über die `TurnOnAsync` und `TurnOffAsync` Methoden:

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

## <a name="platform-implementation-specifics"></a>Plattform Implementierungsspezifika

### <a name="androidtabandroid-specifics"></a>[Android](#tab/android-specifics)

Die Klasse Taschenlampe wurde Optmized je nach Betriebssystem des Geräts.

#### <a name="api-level-23-and-higher"></a>API-Ebene 23 und höher

Auf neueren API-Ebenen [Fackel Modus](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) zum Aktivieren oder Deaktivieren der Flash-Einheit des Geräts verwendet werden.

#### <a name="api-level-22-and-lower"></a>API-Ebene 22 und niedriger

Eine Kamera-Oberfläche Textur erstellt, um Aktivieren / Deaktivieren der `FlashMode` der Kamera-Einheit. 

### <a name="iostabios-specifics"></a>[iOS](#tab/ios-specifics)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) dient zum Aktivieren und Deaktivieren der Fackel und Flash Modus des Geräts.

### <a name="uwptabuwp-specifics"></a>[UNIVERSELLE WINDOWS-PLATTFORM](#tab/uwp-specifics)

[Lamp](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) verwendet, um die erste Lamp auf der Rückseite des Geräts zu aktivieren oder deaktivieren zu erkennen.

-----

## <a name="api"></a>API

- [Taschenlampe-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Taschenlampe API-Dokumentation](xref:Xamarin.Essentials.Flashlight)
