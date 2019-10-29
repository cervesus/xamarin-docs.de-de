---
title: Standort des Benutzers funktioniert in iOS 8 nicht
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9BE92C99-C9C5-427E-ADE4-789DF258BACE
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 548ea296ca1bc48ec2b5de7918a49b5bc1a55c3f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031041"
---
# <a name="user-location-not-working-in-ios-8"></a>Standort des Benutzers funktioniert in iOS 8 nicht

In einem Text-Editor: Öffnen Sie die Datei "Info. plist", und fügen Sie Folgendes hinzu:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>You are about to use location!</string>

<key>NSLocationAlwaysUsageDescription</key>
<string>This will be called if location is used behind the scenes</string>
```

Und innerhalb der MainViewController.cs müssen Sie Folgendes aufzurufen:

```csharp
iPhoneLocationManager.RequestWhenInUseAuthorization ();
```

FERN

```cs
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    iPhoneLocationManager.RequestWhenInUseAuthorization ();
    iPhoneLocationManager.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) => {
        UpdateLocation (mainScreen, e.Locations [e.Locations.Length - 1]);
        };
} else {
    // this won't be called on iOS 6 (deprecated)
    iPhoneLocationManager.UpdatedLocation += (object sender, CLLocationUpdatedEventArgs e) => {
        UpdateLocation (mainScreen, e.NewLocation);
        };
}
```
