---
title: Große Seitentitel unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung verwenden, die den Seitentitel in der Navigationsleiste einer navigationpage als großen Titel anzeigt.
ms.prod: xamarin
ms.assetid: 45FD9145-8319-452C-9AE6-624431A4D43C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 88761472c4beae3f40f2ca6852eea4db8276c4c9
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930680"
---
# <a name="large-page-titles-on-ios"></a>Große Seitentitel unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische wird verwendet, um den Seitentitel als großen Titel auf der Navigationsleiste eines [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) für Geräte mit IOS 11 oder höher anzuzeigen. Ein großer Titel wird linksbündig ausgerichtet und verwendet eine größere Schriftart und wechselt zu einem Standard Titel, wenn der Benutzer mit dem Scrollen von Inhalten beginnt, sodass der Bildschirm tatsächlich verwendet wird. Bei der quer Ausrichtung wird der Titel jedoch in den Mittelpunkt der Navigationsleiste zurückkehren, um das Inhalts Layout zu optimieren. Sie wird in XAML verwendet, indem die `NavigationPage.PrefersLargeTitles` angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

Die- `NavigationPage.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Mit der- `NavigationPage.SetPrefersLargeTitle` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird gesteuert, ob große Titel aktiviert sind.

Wenn große Titel im aktiviert sind [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) , werden auf allen Seiten des Navigations Stapels große Titel angezeigt. Dieses Verhalten kann auf Seiten überschrieben werden, indem die `Page.LargeTitleDisplay` angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird `LargeTitleDisplayMode` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Alternativ kann das Seiten Verhalten aus c# mithilfe der flüssigen API überschrieben werden:

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

Die- `Page.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `Page.SetLargeTitleDisplay` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace steuert das Verhalten des großen Titels auf dem [`Page`](xref:Xamarin.Forms.Page) , wobei die- `LargeTitleDisplayMode` Enumeration drei mögliche Werte bereitstellt:

- `Always`– erzwingen, dass die Navigationsleiste und der Schrift Grad das große Format verwenden.
- `Automatic`– Verwenden Sie denselben Stil (groß oder klein) wie das vorherige Element im Navigations Stapel.
- `Never`– erzwingen Sie die Verwendung der regulären Navigationsleiste mit kleinem Format.

Außerdem kann die- `SetLargeTitleDisplay` Methode zum Umschalten der Enumerationswerte verwendet werden, indem die- `LargeTitleDisplay` Methode aufgerufen wird, die den aktuellen zurückgibt `LargeTitleDisplayMode` :

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

Das Ergebnis ist, dass ein `LargeTitleDisplayMode` angegebenes auf das angewendet wird [`Page`](xref:Xamarin.Forms.Page) , das das Verhalten des großen Titels steuert:

![Weichzeichnereffekt plattformspezifisch](page-large-title-images/large-title.png)

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
