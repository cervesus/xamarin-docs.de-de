---
title: Starten Sie die native Karten-app vonXamarin.Forms
description: Die native Maps-APP auf jeder Plattform kann von einer- Xamarin.Forms Anwendung durch die Start Programm Klasse gestartet werden Xamarin.Essentials .
ms.prod: xamarin
ms.assetid: 5CF7CD67-3F20-4D80-B99E-D35A5FD1019A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/30/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c135d5dd02bba5102f5a93132f079526c84865d5
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84129336"
---
# <a name="launch-the-native-map-app-from-xamarinforms"></a>Starten Sie die native Karten-app vonXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Die native Map-App auf jeder Plattform kann von einer- Xamarin.Forms Anwendung durch die-Klasse gestartet werden Xamarin.Essentials `Launcher` . Diese Klasse ermöglicht es einer Anwendung, eine andere APP über das benutzerdefinierte URI-Schema zu öffnen. Die Start Programmfunktion kann mit der- `OpenAsync` Methode aufgerufen werden, wobei ein-oder-Argument übergeben wird, `string` `Uri` das das zu öffnende benutzerdefinierte URL-Schema darstellt. Weitere Informationen zu Xamarin.Essentials finden Sie unter [Xamarin.Essentials](~/essentials/index.md?context=xamarin/xamarin-forms) .

> [!NOTE]
> Eine Alternative zur Verwendung der- Xamarin.Essentials `Launcher` Klasse ist die Verwendung der- `Map` Klasse. Weitere Informationen finden Sie unter [ Xamarin.Essentials : Map](~/essentials/maps.md?context=xamarin/xamarin-forms).

Die Maps-APP auf jeder Plattform verwendet ein eindeutiges benutzerdefiniertes URI-Schema. Weitere Informationen zum Zuordnungs-URI-Schema unter IOS finden Sie unter [Karten Verknüpfungen](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html) auf Developer.Apple.com. Weitere Informationen zum Maps-URI-Schema unter Android finden Sie unter [Maps Developer Guide](https://developer.android.com/guide/components/intents-common.html#Maps) und [Google Maps Intents for Android](https://developers.google.com/maps/documentation/urls/android-intents) on Developers.Android.com. Weitere Informationen zum Maps-URI-Schema für die universelle Windows-Plattform (UWP) finden Sie unter [Starten der Windows Maps-APP](/windows/uwp/launch-resume/launch-maps-app).

## <a name="launch-the-map-app-at-a-specific-location"></a>Starten der Karten-app an einem bestimmten Speicherort

Ein Speicherort in der nativen Maps-APP kann geöffnet werden, indem dem benutzerdefinierten URI-Schema für jede Karten-app geeignete Abfrage Parameter hinzugefügt werden:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    // https://developer.apple.com/library/ios/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html
    await Launcher.OpenAsync("http://maps.apple.com/?q=394+Pacific+Ave+San+Francisco+CA");
}
else if (Device.RuntimePlatform == Device.Android)
{
    // open the maps app directly
    await Launcher.OpenAsync("geo:0,0?q=394+Pacific+Ave+San+Francisco+CA");
}
else if (Device.RuntimePlatform == Device.UWP)
{
    await Launcher.OpenAsync("bingmaps:?where=394 Pacific Ave San Francisco CA");
}
```

Dieser Beispielcode führt dazu, dass die native Map-App auf jeder Plattform gestartet wird, wobei die Karte auf eine PIN ausgerichtet ist, die den angegebenen Speicherort darstellt:

[![Screenshot der nativen Karten-app unter IOS und Android](native-map-app-images/location.png "Native Map-App")](native-map-app-images/location-large.png#lightbox "Native Map-App")

## <a name="launch-the-map-app-with-directions"></a>Starten Sie die Map-App mit Anweisungen.

Die native Maps-APP kann mit der Anzeige von Anweisungen gestartet werden, indem dem benutzerdefinierten URI-Schema für jede Karten-app geeignete Abfrage Parameter hinzugefügt werden:

```csharp
if (Device.RuntimePlatform == Device.iOS)
{
    // https://developer.apple.com/library/ios/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html
    await Launcher.OpenAsync("http://maps.apple.com/?daddr=San+Francisco,+CA&saddr=cupertino");
}
else if (Device.RuntimePlatform == Device.Android)
{
    // opens the 'task chooser' so the user can pick Maps, Chrome or other mapping app
    await Launcher.OpenAsync("http://maps.google.com/?daddr=San+Francisco,+CA&saddr=Mountain+View");
}
else if (Device.RuntimePlatform == Device.UWP)
{
    await Launcher.OpenAsync("bingmaps:?rtp=adr.394 Pacific Ave San Francisco CA~adr.One Microsoft Way Redmond WA 98052");
}
```

Dieser Beispielcode führt dazu, dass die native Map-App auf jeder Plattform gestartet wird, wobei die Karte auf eine Route zwischen den angegebenen Speicherorten zentriert ist:

[![Screenshot der nativen Karten-app-Route unter IOS und Android](native-map-app-images/directions.png "Nativer Karten-app-Directions")](native-map-app-images/directions-large.png#lightbox "Nativer Karten-app-Directions")

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.Essentials](~/essentials/index.md?context=xamarin/xamarin-forms)
- [Karten Verknüpfungen](https://developer.apple.com/library/archive/featuredarticles/iPhoneURLScheme_Reference/MapLinks/MapLinks.html)
- [Entwicklerhandbuch zu Maps](https://developer.android.com/guide/components/intents-common.html#Maps)
- [Google Maps Intents für Android](https://developers.google.com/maps/documentation/)
- [Starten der Windows-Karten-App](/windows/uwp/launch-resume/launch-maps-app)
