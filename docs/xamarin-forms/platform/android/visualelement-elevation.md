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
ms.openlocfilehash: 48060356014dc7600518b5de555ad3f346c50c35
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128543"
---
# <a name="visualelement-elevation-on-android"></a>Visualelement-Rechte Erweiterung unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische dient zum Steuern der Erhöhung der Rechte oder der Z-Reihenfolge von visuellen Elementen für Anwendungen, die API 21 oder höher als Ziel haben. Die Höhe eines visuellen Elements bestimmt seine Zeichnungsreihenfolge, wobei visuelle Elemente mit höheren z-Werten visuelle Elemente mit niedrigeren z-Werten okding. Sie wird in XAML verwendet, indem die `VisualElement.Elevation` angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

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

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

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

Die- `Button.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die- `VisualElement.SetElevation` Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um die Rechte Erweiterung des visuellen Elements auf NULL zu setzen `float` . Außerdem kann die- `VisualElement.GetElevation` Methode verwendet werden, um den Erweiterungs Wert eines visuellen Elements abzurufen.

Das Ergebnis ist, dass die Erhöhung von visuellen Elementen gesteuert werden kann, sodass visuelle Elemente mit höheren z-Werten visuelle Elemente mit niedrigeren z-Werten okzieren. Daher wird in diesem Beispiel die zweite [`Button`](xref:Xamarin.Forms.Button) über der gerendert, [`BoxView`](xref:Xamarin.Forms.BoxView) da Sie über einen höheren Erweiterungs Wert verfügt:

![](visualelement-elevation-images/elevation.png)

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
