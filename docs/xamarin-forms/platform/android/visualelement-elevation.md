---
title: VisualElement Erhöhung der Rechte unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die Android-Plattform-spezifische genutzt, die steuert, die Erhöhung des VisualElements für Anwendungen, die als API 21 oder höher Ziel.
ms.prod: xamarin
ms.assetid: 5BFD6175-2BBD-41CD-B8F9-521B4750B708
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: c7cc6b560ea91dca89c468271b89d1dcedfcda53
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65927027"
---
# <a name="visualelement-elevation-on-android"></a>VisualElement Erhöhung der Rechte unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Dieses Android-Plattform-spezifische wird verwendet, um die Erhöhung der Rechte oder Z-Reihenfolge der visuellen Elemente auf Anwendungen steuern, die auf API 21 oder höher. Die Höhe ein visuelles Element bestimmt die Zeichnungsreihenfolge mit visuellen Elementen mit höheren Z-Werten, die occluding visuelle Elemente mit niedrigeren Z-Werte. Es ist in XAML verwendet, durch Festlegen der `VisualElement.Elevation` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

Die `Button.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die `VisualElement.SetElevation` Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um die Höhe des visuellen Elements auf eine auf NULL festlegbare festgelegt `float`. Darüber hinaus die `VisualElement.GetElevation` Methode kann verwendet werden, um die Erhöhung der Rechte Wert ein visuelles Element abgerufen werden soll.

Das Ergebnis ist, dass die Höhe der visuellen Elemente so, dass visuelle Elemente mit höheren Z-Werten visuelle Elemente mit niedrigeren Z-Werten verdeckt gesteuert werden kann. Aus diesem Grund in diesem Beispiel wird die zweite [ `Button` ](xref:Xamarin.Forms.Button) gerendert wird, über die [ `BoxView` ](xref:Xamarin.Forms.BoxView) da sie einen höheren Wert für die Erhöhung der Rechte verfügt:

![](visualelement-elevation-images/elevation.png)

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
