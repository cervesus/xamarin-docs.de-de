---
Title: "viewcell Context Actions on Android" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Modell verwenden, das viewcell-Kontext Aktionen im Legacy Modus ermöglicht. "
ms. Prod: xamarin ms. assetid: 8bd71b10-5037-443f-9975-B941CE393E5A ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 09/24/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="viewcell-context-actions-on-android"></a>ViewCell-Kontextaktionen unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Xamarin.FormsWenn ein [`ViewCell`](xref:Xamarin.Forms.ViewCell) in einer Android-Anwendung in einer Android-Anwendung 4,3 standardmäßig Kontext Aktionen für jedes Element in einem definiert [`ListView`](xref:Xamarin.Forms.ListView) , wird standardmäßig das Kontext Aktionsmenü aktualisiert, wenn das ausgewählte Element in `ListView` geändert wird. In früheren Versionen von Xamarin.Forms wurde das Menü Kontext Aktionen jedoch nicht aktualisiert, und dieses Verhalten wird als `ViewCell` Legacy Modus bezeichnet. Dieser Legacy Modus kann zu einem falschen Verhalten führen, wenn ein ein `ListView` verwendet [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) , um seine `ItemTemplate` von Objekten festzulegen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die unterschiedliche Kontext Aktionen definieren.

Mit dieser Android-plattformspezifischen Option [`ViewCell`](xref:Xamarin.Forms.ViewCell) wird der Legacy Modus für Kontext Aktionen aus Gründen der Abwärtskompatibilität aktiviert, sodass das Kontextmenü Menü nicht aktualisiert wird, wenn das ausgewählte Element in einer [`ListView`](xref:Xamarin.Forms.ListView) geändert wird. Sie wird in XAML verwendet, indem die `ViewCell.IsContextActionsLegacyModeEnabled` bindbare Eigenschaft auf festgelegt wird `true` :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ItemsSource="{Binding Items}">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell android:ViewCell.IsContextActionsLegacyModeEnabled="true">
                        <ViewCell.ContextActions>
                            <MenuItem Text="{Binding Item1Text}" />
                            <MenuItem Text="{Binding Item2Text}" />
                        </ViewCell.ContextActions>
                        <Label Text="{Binding Text}" />
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

viewCell.On<Android>().SetIsContextActionsLegacyModeEnabled(true);
```

Die- `ViewCell.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Mit der- `ViewCell.SetIsContextActionsLegacyModeEnabled` Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird der [`ViewCell`](xref:Xamarin.Forms.ViewCell) Legacy Modus für Kontext Aktionen aktiviert, sodass das Kontextmenü Menü nicht aktualisiert wird, wenn das ausgewählte Element in einer geändert wird [`ListView`](xref:Xamarin.Forms.ListView) . Außerdem kann die- `ViewCell.GetIsContextActionsLegacyModeEnabled` Methode verwendet werden, um zurückzugeben, ob der Legacy Modus für Kontext Aktionen aktiviert ist.

Die folgenden Screenshots zeigen den [`ViewCell`](xref:Xamarin.Forms.ViewCell) Kontext von aktivierten Kontext Aktionen:

![Screenshot des Legacy Modus "viewcell" unter Android](viewcell-context-actions-images/legacy-mode-enabled.png "Viewcell-Legacy Modus aktiviert")

In diesem Modus sind die angezeigten Kontext Aktionsmenü Elemente für Zelle 1 und Zelle 2 identisch, obwohl verschiedene Kontextmenü Elemente für Zelle 2 definiert sind.

Die folgenden Screenshots zeigen [`ViewCell`](xref:Xamarin.Forms.ViewCell) , dass der Legacy Modus für Kontext Aktionen deaktiviert ist. Dies ist das Standard Xamarin.Forms Verhalten:

![Screenshot des Legacy Modus "viewcell" deaktiviert unter Android](viewcell-context-actions-images/legacy-mode-disabled.png "Viewcell-Legacy Modus deaktiviert")

In diesem Modus werden die richtigen Kontext Aktionsmenü Elemente für Zelle 1 und Zelle 2 angezeigt.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
