---
title: Visualelement-Rechte Erweiterung unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Element nutzen, um die Erweiterung von visualelements auf Anwendungen zu steuern, die API 21 oder höher als Ziel haben.
ms.prod: xamarin
ms.assetid: 5BFD6175-2BBD-41CD-B8F9-521B4750B708
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 243e351f29b056a6d4a567b8e39240a87f37aec2
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651883"
---
# <a name="visualelement-elevation-on-android"></a>Visualelement-Rechte Erweiterung unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische dient zum Steuern der Erhöhung der Rechte oder der Z-Reihenfolge von visuellen Elementen für Anwendungen, die API 21 oder höher als Ziel haben. Die Höhe ein visuelles Element bestimmt die Zeichnungsreihenfolge mit visuellen Elementen mit höheren Z-Werten, die occluding visuelle Elemente mit niedrigeren Z-Werte. Es ist in XAML verwendet, durch Festlegen der `VisualElement.Elevation` angefügten Eigenschaft, um eine `boolean` Wert:

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

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
