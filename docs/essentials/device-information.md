---
title: Xamarin.Essentials-Geräteinformationen
description: Die DeviceInfo-Klasse enthält Informationen über das Gerät die Anwendung ausgeführt wird.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: d5220b09808bda2deb2ef391b5fbe2a1332d0e28
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials-Geräteinformationen

![Vorabversion NuGet](~/media/shared/pre-release.png)

Die **DeviceInfo** Klasse enthält Informationen über das Gerät die Anwendung ausgeführt wird.

## <a name="using-deviceinfo"></a>DeviceInfo verwenden

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die folgende Informationen werden über die API verfügbar gemacht:

```csharp
// Device Model (SMG-950U)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## <a name="platformsxrefxamarinessentialsdeviceinfoplatforms"></a>[Plattformen](xref:Xamarin.Essentials.DeviceInfo.Platforms)

`DeviceInfo.Platform` Um eine Konstante Zeichenfolge, die für das Betriebssystem ordnet korreliert. Die Werte können überprüft werden, mit der `Platforms` Klasse:

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – nicht unterstützte

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idiome](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` korreliert eine Konstante Zeichenfolge, die die Anwendung den Typ des Geräts zugeordnet ausgeführt wird. Die Werte können überprüft werden, mit der `Idioms` Klasse:

- **DeviceInfo.Idioms.Phone** – Phone
- **DeviceInfo.Idioms.Tablet** – Tablet
- **DeviceInfo.Idioms.Desktop** – Desktops
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Idioms.Unsupported** – nicht unterstützte

## <a name="device-type"></a>Gerätetyp

`DeviceInfo.DeviceType` korreliert eine Enumeration, um festzustellen, ob die Anwendung ausführen auf einem physischen oder virtuellen Gerät ist. Ein virtuelles Gerät ist einem Simulator oder Emulator.

## <a name="api"></a>API

- [DeviceInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Essentials/DeviceInfo)
- [DeviceInfo-API-Dokumentation](xref:Xamarin.Essentials.DeviceInfo)
