---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 46a1b4d00b9eea276b9a3b3d5bffbdac3d31e0ef
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136577"
---
# <a name="refreshview-pull-direction-on-windows"></a>RefreshView-Pullrichtung unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese universelle Windows-Plattform plattformspezifisch ermöglicht, dass die Pull-Richtung einer `RefreshView` geändert werden muss, damit Sie der Ausrichtung des Bild lauffähigen Steuer Elements entspricht, das Daten anzeigt. Sie wird in XAML verwendet, indem die `RefreshView.RefreshPullDirection` bindbare Eigenschaft auf einen Wert der- `RefreshPullDirection` Enumeration festgelegt wird:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <RefreshView windows:RefreshView.RefreshPullDirection="LeftToRight"
                 IsRefreshing="{Binding IsRefreshing}"
                 Command="{Binding RefreshCommand}">
        <ScrollView>
            ...
        </ScrollView>
    </RefreshView>
 </ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
refreshView.On<Windows>().SetRefreshPullDirection(RefreshPullDirection.LeftToRight);
```

Die- `RefreshView.On<Windows>` Methode gibt an, dass diese plattformspezifische nur auf der universelle Windows-Plattform ausgeführt wird. Die- `RefreshView.SetRefreshPullDirection` Methode im- [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace wird verwendet, um die Pull-Richtung des festzulegen `RefreshView` , wobei die- `RefreshPullDirection` Enumeration vier mögliche Werte bereitstellt:

- `LeftToRight`Gibt an, dass ein Pull von links nach rechts eine Aktualisierung initiiert.
- `TopToBottom`Gibt an, dass ein Pull von oben nach unten eine Aktualisierung initiiert und die standardmäßige Pull-Richtung eines ist `RefreshView` .
- `RightToLeft`Gibt an, dass ein Pull von rechts nach links eine Aktualisierung initiiert.
- `BottomToTop`Gibt an, dass ein Pull von unten nach oben eine Aktualisierung initiiert.

Außerdem kann die- `GetRefreshPullDirection` Methode verwendet werden, um den aktuellen `RefreshPullDirection` des zurückzugeben `RefreshView` .

Das Ergebnis ist, dass ein `RefreshPullDirection` angegebenes auf das-Element angewendet wird `RefreshView` , um die Pull Direction so festzulegen, dass Sie mit der Ausrichtung des scrollbaren Steuer Elements, das Daten anzeigt Der folgende Screenshot zeigt eine `RefreshView` mit einer `LeftToRight` Pull-Richtung:

[![Screenshot einer aktuerfrischenden Ansicht mit einer von links nach rechts gerichteten Pull-Richtung auf UWP](refreshview-pulldirection-images/refreshview-pulldirection.png "Aktuativenansicht mit der Richtung von links nach rechts")](refreshview-pulldirection-images/refreshview-pulldirection-large.png#lightbox "Aktuativenansicht mit der Richtung von links nach rechts")

> [!NOTE]
> Wenn Sie die Pull-Richtung ändern, wird die Anfangsposition des Fortschritts Kreises automatisch so gedreht, dass der Pfeil an der entsprechenden Position für die Pull-Richtung beginnt.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
