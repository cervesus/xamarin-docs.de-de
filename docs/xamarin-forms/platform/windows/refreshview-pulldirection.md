---
title: Aktuativenansichts Richtung unter Windows
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die Windows-plattformspezifische verwenden, mit der die Pull-Richtung einer aktualisierbaren Ansicht geändert werden kann.
ms.prod: xamarin
ms.assetid: 407A862B-281E-4384-9696-C0655830B84D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2019
ms.openlocfilehash: cf2ab38bed7b45a48fcf0b5f86add49c0d4cc21f
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697650"
---
# <a name="refreshview-pull-direction-on-windows"></a>Aktuativenansichts Richtung unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese universelle Windows-Plattform plattformspezifisch ermöglicht, dass die Pull-Richtung eines `RefreshView` geändert werden kann, um der Ausrichtung des Bild lauffähigen Steuer Elements zu entsprechen, das Daten anzeigt. Sie wird in XAML verwendet, indem die `RefreshView.RefreshPullDirection` bindbare-Eigenschaft auf einen Wert der `RefreshPullDirection`-Enumeration festgelegt wird:

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

Alternativ kann Sie von C# der Verwendung der flüssigen API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
refreshView.On<Windows>().SetRefreshPullDirection(RefreshPullDirection.LeftToRight);
```

Die `RefreshView.On<Windows>`-Methode gibt an, dass diese plattformspezifische nur auf der universelle Windows-Plattform ausgeführt wird. Die `RefreshView.SetRefreshPullDirection`-Methode im [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um die Pull-Richtung der `RefreshView` festzulegen, wobei die `RefreshPullDirection`-Enumeration vier mögliche Werte bereitstellt:

- `LeftToRight` gibt an, dass ein Pull von links nach rechts eine Aktualisierung initiiert.
- `TopToBottom` gibt an, dass ein Pull von oben nach unten eine Aktualisierung initiiert und die standardmäßige Pull-Richtung eines `RefreshView` ist.
- `RightToLeft` gibt an, dass ein Pull von rechts nach links eine Aktualisierung initiiert.
- `BottomToTop` gibt an, dass ein Pull von unten nach oben eine Aktualisierung initiiert.

Außerdem kann die `GetRefreshPullDirection`-Methode verwendet werden, um die aktuelle `RefreshPullDirection` der `RefreshView` zurückzugeben.

Das Ergebnis ist, dass ein angegebenes `RefreshPullDirection` auf den `RefreshView` angewendet wird, um die Pull Direction so festzulegen, dass Sie der Ausrichtung des scrollbaren Steuer Elements entspricht, das Daten anzeigt. Der folgende Screenshot zeigt eine `RefreshView` mit einer `LeftToRight` Pull-Richtung:

[![Screenshot einer aktuerfrischenden Ansicht mit einer von links nach rechts gerichteten Pull-Richtung auf UWP](refreshview-pulldirection-images/refreshview-pulldirection.png "Aktuativenansicht mit der Richtung von links nach rechts")](refreshview-pulldirection-images/refreshview-pulldirection-large.png#lightbox "Aktuativenansicht mit der Richtung von links nach rechts")

> [!NOTE]
> Wenn Sie die Pull-Richtung ändern, wird die Anfangsposition des Fortschritts Kreises automatisch so gedreht, dass der Pfeil an der entsprechenden Position für die Pull-Richtung beginnt.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
