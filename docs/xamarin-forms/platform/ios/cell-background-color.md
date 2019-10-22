---
title: Zellen Hintergrundfarbe unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung verwenden, die die Standard Hintergrundfarbe von Zellen unter IOS festlegt.
ms.prod: xamarin
ms.assetid: 2A3FDACF-5AE2-40DE-8488-6FE41733712F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 24276dce97e4935ba41d7012cf6a9aa8fa2658a8
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68651375"
---
# <a name="cell-background-color-on-ios"></a>Zellen Hintergrundfarbe unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen wird die Standard Hintergrundfarbe [`Cell`](xref:Xamarin.Forms.Cell) Instanzen festgelegt. Sie wird in XAML verwendet, indem die `Cell.DefaultBackgroundColor` bindbare Eigenschaft auf einen [`Color`](xref:Xamarin.Forms.Color)festgelegt wird:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  IsGroupingEnabled="true">
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <ViewCell ios:Cell.DefaultBackgroundColor="Teal">
                        <Label Margin="10,10"
                               Text="{Binding Key}"
                               FontAttributes="Bold" />
                    </ViewCell>
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie von C# der Verwendung der flüssigen API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

Die `ListView.On<iOS>`-Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die `Cell.SetDefaultBackgroundColor`-Methode im [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace legt die Hintergrundfarbe der Zelle auf eine angegebene [`Color`](xref:Xamarin.Forms.Color)fest. Außerdem kann die `Cell.DefaultBackgroundColor`-Methode verwendet werden, um die aktuelle Zellen Hintergrundfarbe abzurufen.

Das Ergebnis ist, dass die Hintergrundfarbe in einer [`Cell`](xref:Xamarin.Forms.Cell) auf eine bestimmte [`Color`](xref:Xamarin.Forms.Color)festgelegt werden kann:

[![Screenshot der Header Zellen der Teal-Gruppe unter IOS](cell-background-color-images/group-header-cell-color.png "ListView mit Header Zellen der Gruppe "Blaugrün"")](cell-background-color-images/group-header-cell-color-large.png#lightbox "ListView mit Header Zellen der Gruppe "Blaugrün"")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
