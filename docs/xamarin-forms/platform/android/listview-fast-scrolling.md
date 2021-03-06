---
title: ListView schnelles Scrollen unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Datei nutzen, die das schnelle Scrollen durch Daten in einem ListView ermöglicht.
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e52ee0a9b61a654ae048fd0fb4a14c402c932524
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929831"
---
# <a name="listview-fast-scrolling-on-android"></a>ListView schnelles Scrollen unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische wird verwendet, um einen schnellen Bildlauf durch Daten in einem zu ermöglichen [`ListView`](xref:Xamarin.Forms.ListView) . Sie wird in XAML verwendet, indem die `ListView.IsFastScrollEnabled` angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

Die- `ListView.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die- `ListView.SetIsFastScrollEnabled` Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um einen schnellen Bildlauf durch Daten in einem zu ermöglichen [`ListView`](xref:Xamarin.Forms.ListView) . Außerdem kann die- `SetIsFastScrollEnabled` Methode verwendet werden, um den schnellen Bildlauf durch Aufrufen der- `IsFastScrollEnabled` Methode zu wechseln, um zurückzugeben, ob ein schnelles Scrollen aktiviert ist:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Das Ergebnis ist, dass der schnelle Bildlauf durch Daten in einem [`ListView`](xref:Xamarin.Forms.ListView) aktiviert werden kann, wodurch die Größe des Bild Lauf Zieh Punkts geändert wird:

[![ListView-fastscroll-plattformspezifisch](listview-fast-scrolling-images/fastscroll.png)](listview-fast-scrolling-images/fastscroll-large.png#lightbox "ListView-fastscroll-plattformspezifisch")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
