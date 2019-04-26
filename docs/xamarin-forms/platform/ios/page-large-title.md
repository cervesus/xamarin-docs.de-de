---
title: Große Seitentitel unter iOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die plattformspezifischen iOS nutzen, der den Seitentitel als große Titel auf der Navigationsleiste auf einer "NavigationPage" angezeigt wird.
ms.prod: xamarin
ms.assetid: 45FD9145-8319-452C-9AE6-624431A4D43C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: ad1c7f94467c6f32b9108ab7f6abe85d31679d3a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60952121"
---
# <a name="large-page-titles-on-ios"></a>Große Seitentitel unter iOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Dieses iOS-Plattform-spezifische wird verwendet, um den Seitentitel als große Titel auf der Navigationsleiste des Anzeigen einer [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), für Geräte, iOS 11 oder höher verwenden. Ein große Titel wird links ausgerichtet und wird von einer größeren Schrift und geht in einen standard-Titel, wie der Benutzer beginnt, Scrollen von Inhalt, sodass Bildschirmfläche effizient verwendet wird. Allerdings wird im Querformat, der Titel in der Mitte des der Navigationsleiste Inhaltslayout optimieren zurück. Es ist in XAML verwendet, durch Festlegen der `NavigationPage.PrefersLargeTitles` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

Die `NavigationPage.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `NavigationPage.SetPrefersLargeTitle` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace steuert, ob große Titel aktiviert sind.

Vorausgesetzt, dass große Titel aktiviert sind, auf die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), alle Seiten im Navigationsstapel werden große Titel angezeigt. Dieses Verhalten kann auf Seiten überschrieben werden, durch Festlegen der `Page.LargeTitleDisplay` angefügte Eigenschaft auf den Wert der `LargeTitleDisplayMode` Enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Alternativ kann das Seitenverhalten in C# mithilfe der fluent-API überschrieben werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

Die `Page.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Page.SetLargeTitleDisplay` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace, steuert das Verhalten der große Titel auf der [ `Page` ](xref:Xamarin.Forms.Page), mit der `LargeTitleDisplayMode` Enumeration, die Bereitstellung von drei möglichen Werte:

- `Always` – erzwingen, dass die Navigationsleiste und die Schriftart Größe der große-Format verwendet.
- `Automatic` – Verwenden Sie das gleiche Format (Groß oder klein) als das vorherige Element im Navigationsstapel.
- `Never` – wird die Verwendung der regulären, kleine Format Navigationsleiste erzwungen.

Darüber hinaus die `SetLargeTitleDisplay` Methode kann verwendet werden, zum Umschalten der Enumerationswerte durch Aufrufen der `LargeTitleDisplay` Methode, die die aktuelle zurückgibt `LargeTitleDisplayMode`:

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

Das Ergebnis ist, die einem angegebenen `LargeTitleDisplayMode` gilt, an die [ `Page` ](xref:Xamarin.Forms.Page), welche Steuerelemente das große Titel Verhalten:

![](page-large-title-images/large-title.png "Blur-Effekt plattformspezifische")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
