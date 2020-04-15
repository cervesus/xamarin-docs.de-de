---
title: 'Xamarin.Essentials: Geräteinformationen'
description: In diesem Dokument wird die Klasse „DeviceInfo“ in Xamarin.Essentials beschrieben, die Informationen zu dem Gerät bereitstellt, auf dem die Anwendung ausgeführt wird.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: 1cab4ea8ea3f98def4830e101783db1554efa69c
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "78295416"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: Geräteinformationen

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

## <a name="platforms"></a>Plattformen

[`DeviceInfo.Platform`](xref:Xamarin.Essentials.DeviceInfo.Platform) korreliert mit einer konstanten Zeichenfolge, die dem Betriebssystem zugeordnet ist. Die Werte können mit der `DevicePlatform`-Struktur überprüft werden:

- **DevicePlatform.iOS**: iOS
- **DevicePlatform.Android**: Android
- **DevicePlatform.UWP**: UWP
- **DevicePlatform.Unknown**: Unbekannt

## <a name="idioms"></a>Idioms

[`DeviceInfo.Idiom`](xref:Xamarin.Essentials.DeviceInfo.Idiom) korreliert mit einer konstanten Zeichenfolge, die dem Typ des Geräts zugeordnet ist, auf dem die Anwendung ausgeführt wird. Die Werte können mit der `DeviceIdiom`-Struktur überprüft werden:

- **DeviceIdiom.Phone**: Mobiltelefon
- **DeviceIdiom.Tablet**: Tablet
- **DeviceIdiom.Desktop**: Desktop
- **DeviceIdiom.TV**: TV
- **DeviceIdiom.Watch**: Überwachungselement
- **DeviceIdiom.Unknown**: Unbekannt

## <a name="device-type"></a>Gerätetyp

`DeviceInfo.DeviceType` entspricht einer Enumeration, um festzustellen, ob die Anwendung auf einem physischen oder virtuellen Gerät ausgeführt wird. Ein virtuelles Gerät ist ein Simulator oder Emulator.

## <a name="platform-implementation-specifics"></a>Besonderheiten bei der plattformspezifischen Implementierung

# <a name="ios"></a>[iOS](#tab/ios)

iOS macht für Entwickler keine API verfügbar, um das Modell des konkreten iOS-Geräts abzurufen. Stattdessen wird eine Hardware-ID (z. B. _iPhone10.6_) zurückgegeben, die auf das iPhone X verweist. Eine Zuordnung dieser Kennungen wird von Apple nicht bereitgestellt, kann jedoch mithilfe der folgenden (nicht offiziellen) Quellen gefunden werden: [The iPhone Wiki](https://www.theiphonewiki.com/wiki/Models) und [Get iOS Model](https://github.com/dannycabrera/Get-iOS-Model).

--------------

## <a name="api"></a>API

- [DeviceInfo-Quellcode](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [DeviceInfo-API-Dokumentation](xref:Xamarin.Essentials.DeviceInfo)
