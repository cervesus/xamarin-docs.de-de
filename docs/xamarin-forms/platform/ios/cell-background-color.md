---
Title: "Cell Background Color on IOS" Description: "Platform-Besonderheiten ermöglicht Ihnen die Nutzung von Funktionen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, mit der die Standard Hintergrundfarbe von Zellen unter IOS festgelegt wird.
ms. Prod: xamarin ms. assetid: 2a3f DACF -5ae2-40DE-8488-6fe41733712f ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 10/24/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="cell-background-color-on-ios"></a>Zellen Hintergrundfarbe unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen wird die Standard Hintergrundfarbe der-Instanzen festgelegt [`Cell`](xref:Xamarin.Forms.Cell) . Sie wird in XAML verwendet, indem die `Cell.DefaultBackgroundColor` bindbare Eigenschaft auf festgelegt wird [`Color`](xref:Xamarin.Forms.Color) :

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

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var viewCell = new ViewCell { View = ... };
viewCell.On<iOS>().SetDefaultBackgroundColor(Color.Teal);
```

Die- `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `Cell.SetDefaultBackgroundColor` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace legt die Hintergrundfarbe der Zelle auf eine angegebene fest [`Color`](xref:Xamarin.Forms.Color) . Außerdem kann die- `Cell.DefaultBackgroundColor` Methode zum Abrufen der aktuellen Zellen Hintergrundfarbe verwendet werden.

Das Ergebnis ist, dass die Hintergrundfarbe in einem [`Cell`](xref:Xamarin.Forms.Cell) auf einen bestimmten festgelegt werden kann [`Color`](xref:Xamarin.Forms.Color) :

[![Screenshot der Header Zellen der Teal-Gruppe unter IOS](cell-background-color-images/group-header-cell-color.png "ListView mit Header Zellen der Gruppe "Blaugrün"")](cell-background-color-images/group-header-cell-color-large.png#lightbox "ListView mit Header Zellen der Gruppe "Blaugrün"")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
