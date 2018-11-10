---
title: 'Xamarin.Essentials: Geräteinformationen'
description: In diesem Dokument wird die Klasse „DeviceInfo“ in Xamarin.Essentials beschrieben, die Informationen zu dem Gerät bereitstellt, auf dem die Anwendung ausgeführt wird.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 530b04446703d78452357b2c9f9089e59ebf6e6c
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674812"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: Geräteinformationen

![NuGet-Vorabrelease](~/media/shared/pre-release.png)

Die Klasse **DeviceInfo** stellt Informationen zu dem Gerät bereit, auf dem die Anwendung ausgeführt wird.

## <a name="get-started"></a>Erste Schritte

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-deviceinfo"></a>Verwenden der Geräteinformationen

Fügen Sie Ihrer Klasse einen Verweis auf Xamarin.Essentials hinzu:

```csharp
using Xamarin.Essentials;
```

Die folgenden Informationen werden über die API verfügbar gemacht:

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

`DeviceInfo.Platform` entspricht einer konstanten Zeichenfolge, die dem Betriebssystem zugeordnet ist. Die Werte können mit der Klasse `Platforms` überprüft werden:

- **DeviceInfo.Platforms.iOS** – iOS
- **DeviceInfo.Platforms.Android** – Android
- **DeviceInfo.Platforms.UWP** – UWP
- **DeviceInfo.Platforms.Unsupported** – Nicht unterstützt

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Idioms](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` entspricht einer konstanten Zeichenfolge, die dem Typ des Geräts zugeordnet ist, auf dem die Anwendung ausgeführt wird. Die Werte können mit der Klasse `Idioms` überprüft werden:

- **DeviceInfo.Idioms.Phone** – Telefon
- **DeviceInfo.Idioms.Tablet** – Tablet
- **DeviceInfo.Idioms.Desktop** – Desktop
- **DeviceInfo.Idioms.TV** – TV
- **DeviceInfo.Platforms.Unsupported** – Nicht unterstützt

## <a name="device-type"></a>Gerätetyp

`DeviceInfo.DeviceType` entspricht einer Enumeration, um festzustellen, ob die Anwendung auf einem physischen oder virtuellen Gerät ausgeführt wird. Ein virtuelles Gerät ist ein Simulator oder Emulator.

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="iostabios"></a>[iOS](#tab/ios)

iOS macht für Entwickler keine API verfügbar, um den Namen des bestimmten iOS-Geräts abzurufen. Stattdessen wird eine Hardware-ID (z. B. _iPhone10,6_) zurückgegeben, die auf das iPhone X verweist. Eine Zuordnung dieser Kennungen wird von Apple nicht bereitgestellt, kann jedoch in [The iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) (eine nicht offizielle Quellcode-Quelle) gefunden werden.

--------------

## <a name="api"></a>API

- [DeviceInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo-API-Dokumentation](xref:Xamarin.Essentials.DeviceInfo)
