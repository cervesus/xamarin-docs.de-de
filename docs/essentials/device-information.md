---
title: 'Xamarin.Essentials: Geräteinformationen'
description: Dieses Dokument beschreibt die DeviceInfo-Klasse in Xamarin.Essentials, bietet Informationen über das Gerät, dass die Anwendung ausgeführt wird.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 18fe081372cc190e5ead2045f36d63652f8702c3
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353801"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: Geräteinformationen

![Vorabversionen von NuGet](~/media/shared/pre-release.png)

Die **DeviceInfo** Klasse enthält Informationen über das Gerät auf die Anwendung ausgeführt wird.

## <a name="using-deviceinfo"></a>DeviceInfo verwenden

Fügen Sie einen Verweis auf Xamarin.Essentials in Ihrer Klasse hinzu:

```csharp
using Xamarin.Essentials;
```

Die folgende Informationen werden über die API verfügbar gemacht:

```csharp
// Device Model (SMG-950U, iPhone10,6)
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

`DeviceInfo.Platform` eine Konstante Zeichenfolge, die für das Betriebssystem ordnet entspricht. Die Werte können überprüft werden, mit der `Platforms` Klasse:

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – nicht unterstützt

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Ausdrücke](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` korreliert eine Konstante Zeichenfolge, die den Typ des Geräts die Anwendung zugeordnet, ausgeführt wird. Die Werte können überprüft werden, mit der `Idioms` Klasse:

- **DeviceInfo.Idioms.Phone** – Telefon
- **DeviceInfo.Idioms.Tablet** -Tablet
- **DeviceInfo.Idioms.Desktop** – Desktop
- **DeviceInfo.Idioms.TV** -TV
- **DeviceInfo.Idioms.Unsupported** – nicht unterstützt

## <a name="device-type"></a>Gerätetyp

`DeviceInfo.DeviceType` korreliert eine Enumeration, um festzustellen, ob die Anwendung auf einem physischen oder virtuellen Gerät ausgeführt wird. Ein virtuelles Gerät ist einem Simulator oder Emulator.

## <a name="platform-implementation-specifics"></a>Implementierung von Plattformeigenschaften

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS macht eine API für Entwickler, um den Namen des bestimmte iOS-Geräts abzurufen. Stattdessen eine Hardware-ID wird zurückgegeben, wie z. B. _iPhone10, 6_ , die auf dem iPhone X verweist. Eine Zuordnung von diesen Identifers werden nicht von Apple bereitgestellt, jedoch finden Sie unter [iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (eine nicht-Official-Source-Quelle).

--------------

## <a name="api"></a>API

- [DeviceInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo-API-Dokumentation](xref:Xamarin.Essentials.DeviceInfo)
