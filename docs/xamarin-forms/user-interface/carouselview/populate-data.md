---
title: Xamarin. Forms carouselview-Daten
description: Eine carouselview wird mit Daten aufgefüllt, indem die ItemsSource-Eigenschaft auf eine beliebige Auflistung festgelegt wird, die IEnumerable implementiert.
ms.prod: xamarin
ms.assetid: 20DB2C57-CE3A-4D91-80DC-73AE361A3CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/17/2019
ms.openlocfilehash: 7d1183bf0c741b5a7ca02b43c4edb0c640ee1ac2
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488222"
---
# <a name="xamarinforms-carouselview-data"></a>Xamarin. Forms carouselview-Daten

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) enthält die folgenden Eigenschaften, die die anzuzeigenden Daten und ihre Darstellung definieren:

- [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)vom Typ `IEnumerable`gibt die Sammlung von Elementen an, die angezeigt werden sollen, und hat den Standardwert `null`.
- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate)vom Typ [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)gibt die Vorlage an, die auf jedes Element in der Auflistung der anzuzeigenden Elemente angewendet werden soll.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert eine `ItemsUpdatingScrollMode` Eigenschaft, die das Scrollverhalten der `CarouselView` darstellt, wenn ihr neue Elemente hinzugefügt werden. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Steuern der Scrollposition, wenn neue Elemente hinzugefügt werden](scrolling.md#control-scroll-position-when-new-items-are-added).

[`CarouselView`](xref:Xamarin.Forms.CarouselView) können Daten auch inkrementell laden, wenn der Benutzer einen Bildlauf ausführt. Weitere Informationen finden Sie unter [inkrementelles Laden von Daten](#load-data-incrementally).

## <a name="populate-a-carouselview-with-data"></a>Auffüllen einer "carouselview" mit Daten

Ein [`CarouselView`](xref:Xamarin.Forms.CarouselView) wird mit Daten aufgefüllt, indem seine [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft auf eine beliebige Auflistung festgelegt wird, die `IEnumerable`implementiert. Elemente können in XAML hinzugefügt werden, indem die `ItemsSource`-Eigenschaft aus einem Zeichen folgen Array initialisiert wird:

```xaml
<CarouselView>
    <CarouselView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Baboon</x:String>
            <x:String>Capuchin Monkey</x:String>
            <x:String>Blue Monkey</x:String>
            <x:String>Squirrel Monkey</x:String>
            <x:String>Golden Lion Tamarin</x:String>
            <x:String>Howler Monkey</x:String>
            <x:String>Japanese Macaque</x:String>
        </x:Array>
    </CarouselView.ItemsSource>
</CarouselView>
```

> [!NOTE]
> Bedenken Sie, dass Sie für das Element `x:Array` ein `Type`-Attribute benötigen, das die Elementtypen im Array angibt.

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!IMPORTANT]
> Wenn die [`CarouselView`](xref:Xamarin.Forms.CarouselView) beim Hinzufügen, entfernen oder Ändern von Elementen in der zugrunde liegenden Auflistung aktualisiert werden muss, sollte die zugrunde liegende Auflistung eine `IEnumerable` Auflistung sein, die Benachrichtigungen über Eigenschafts Änderungen sendet, z. b. `ObservableCollection`.

Standardmäßig zeigt [`CarouselView`](xref:Xamarin.Forms.CarouselView) Elemente horizontal an. Die folgenden Screenshots zeigen eine `CarouselView`, die verschiedene Zeichen folgen Elemente unter IOS und Android anzeigt:

[![Screenshot von carouselview mit Textelementen unter IOS und Android](populate-data-images/text.png "Text Elemente in einer carouselview")](populate-data-images/text-large.png#lightbox "Text Elemente in einer carouselview")

Informationen zum Ändern der [`CarouselView`](xref:Xamarin.Forms.CarouselView) Ausrichtung finden Sie unter [xamarin. Forms carouselview Layout](layout.md). Informationen dazu, wie Sie die Darstellung der einzelnen Elemente im `CarouselView`definieren, finden Sie unter [Definieren der Element](#define-item-appearance)Darstellung.

### <a name="data-binding"></a>Datenbindung

[`CarouselView`](xref:Xamarin.Forms.CarouselView) können mit Daten aufgefüllt werden, indem die Datenbindung verwendet wird, um die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschaft an eine `IEnumerable` Auflistung zu binden. In XAML wird dies mit der `Binding` Markup Erweiterung erreicht:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}" />
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

In diesem Beispiel werden die [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) -Eigenschafts Daten an die `Monkeys`-Eigenschaft des verbundenen ViewModel gebunden.

> [!NOTE]
> Kompilierte Bindungen können aktiviert werden, um die Daten bindungsleistung in xamarin. Forms-Anwendungen zu verbessern. Weitere Informationen finden Sie unter [Kompilierte Bindungen](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

Weitere Informationen zur Datenbindung finden Sie unter [Xamarin.Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="define-item-appearance"></a>Element Darstellung definieren

Die Darstellung der einzelnen Elemente in der [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann definiert werden, indem die [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)festgelegt wird:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

carouselView.ItemTemplate = new DataTemplate(() =>
{
    Label nameLabel = new Label { ... };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Image image = new Image { ... };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label locationLabel = new Label { ... };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Label detailsLabel = new Label { ... };
    detailsLabel.SetBinding(Label.TextProperty, "Details");

    StackLayout stackLayout = new StackLayout
    {
        Children = { nameLabel, image, locationLabel, detailsLabel }
    };

    Frame frame = new Frame { ... };
    StackLayout rootStackLayout = new StackLayout
    {
        Children = { frame }
    };

    return rootStackLayout;
});
```

Die im [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) angegebenen Elemente definieren die Darstellung der einzelnen Elemente im `CarouselView`. Im Beispiel wird das Layout innerhalb des `DataTemplate` von einem [`StackLayout`](xref:Xamarin.Forms.StackLayout)verwaltet, und die Daten werden mit einem [`Image`](xref:Xamarin.Forms.Image) -Objekt und drei [`Label`](xref:Xamarin.Forms.Label) Objekten angezeigt, die alle an Eigenschaften der `Monkey`-Klasse gebunden sind:

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

Die folgenden Screenshots zeigen das Ergebnis der Vorlagen Darstellung der einzelnen Elemente:

[![Screenshot von carouselview, in dem jedes Element auf IOS und Android Vorlagen basiert](populate-data-images/datatemplate.png "Auf Vorlagen basierende Elemente in einer "carouselview"")](populate-data-images/datatemplate-large.png#lightbox "Auf Vorlagen basierende Elemente in einer "carouselview"")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente im [`CarouselView`](xref:Xamarin.Forms.CarouselView) kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die Eigenschaft [`CarouselView.ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) auf ein [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) Objekt festgelegt wird:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CarouselViewDemos.Controls"
             x:Class="CarouselViewDemos.Views.HorizontalLayoutDataTemplateSelectorPage">
    <ContentPage.Resources>
        <DataTemplate x:Key="AmericanMonkeyTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="OtherMonkeyTemplate">
            ...
        </DataTemplate>

        <controls:MonkeyDataTemplateSelector x:Key="MonkeySelector"
                                             AmericanMonkey="{StaticResource AmericanMonkeyTemplate}"
                                             OtherMonkey="{StaticResource OtherMonkeyTemplate}" />
    </ContentPage.Resources>

    <CarouselView ItemsSource="{Binding Monkeys}"
                  ItemTemplate="{StaticResource MonkeySelector}" />
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemTemplate = new MonkeyDataTemplateSelector { ... }
};
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Die [`ItemTemplate`](xref:Xamarin.Forms.ItemsView.ItemTemplate) -Eigenschaft wird auf ein `MonkeyDataTemplateSelector`-Objekt festgelegt. Das folgende Beispiel zeigt die `MonkeyDataTemplateSelector`-Klasse:

```csharp
public class MonkeyDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate AmericanMonkey { get; set; }
    public DataTemplate OtherMonkey { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Monkey)item).Location.Contains("America") ? AmericanMonkey : OtherMonkey;
    }
}
```

Die `MonkeyDataTemplateSelector`-Klasse definiert `AmericanMonkey` und `OtherMonkey` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften, die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` Überschreibung gibt die `AmericanMonkey` Vorlage zurück, wenn der affenname "America" enthält. Wenn der affenname nicht "America" enthält, gibt die `OnSelectTemplate` Überschreibung die `OtherMonkey` Vorlage zurück, die die Daten abgeblendet anzeigt:

[![Screenshot der Auswahl von "carouselview Runtime item Template" unter IOS und Android](populate-data-images/datatemplateselector.png "Auswahl der Lauf Zeitelement Vorlage in einer "carouselview"")](populate-data-images/datatemplateselector-large.png#lightbox "Auswahl der Lauf Zeitelement Vorlage in einer "carouselview"")

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

> [!IMPORTANT]
> Wenn Sie [`CarouselView`](xref:Xamarin.Forms.CarouselView)verwenden, legen Sie das Stamm Element ihrer [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) -Objekte nie auf einen `ViewCell`fest. Dies führt dazu, dass eine Ausnahme ausgelöst wird, da `CarouselView` über kein Konzept von Zellen verfügt.

## <a name="display-indicators"></a>Anzeigen von Indikatoren

Indikatoren, die die Anzahl der Elemente und die aktuelle Position in einem `CarouselView`darstellen, können neben dem `CarouselView`angezeigt werden. Dies kann mit dem `IndicatorView`-Steuerelement erreicht werden:

```xaml
<StackLayout>
    <CarouselView x:Name="carouselView"
                  ItemsSource="{Binding Monkeys}">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView ItemsSourceBy="carouselView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

In diesem Beispiel wird der `IndicatorView` unterhalb des `CarouselView`gerendert, mit einem Indikator für jedes Element im `CarouselView`. Die `IndicatorView` wird mit Daten aufgefüllt, indem die `ItemsSourceBy`-Eigenschaft auf das `CarouselView`-Objekt festgelegt wird. Jeder Indikator ist ein heller grauer Kreis, während der Indikator, der das aktuelle Element im `CarouselView` darstellt, dunkelgrau ist:

[![Screenshot von "carouselview" und "indikatorview" unter IOS und Android](populate-data-images/indicators.png "Sichorview-Kreise")](populate-data-images/indicators-large.png#lightbox "Sichorview-Kreise")

> [!IMPORTANT]
> Wenn Sie die `ItemsSourceBy`-Eigenschaft festlegen, wird die `IndicatorView.Position`-Eigenschaft an die `CarouselView.Position`-Eigenschaft gebunden, und die `IndicatorView.ItemsSource` Eigenschaften Bindung an die `CarouselView.ItemsSource`-Eigenschaft.

Weitere Informationen zu Indikatoren finden Sie unter [xamarin. Forms-Anzeige](~/xamarin-forms/user-interface/indicatorview.md).

## <a name="pull-to-refresh"></a>Aktualisieren durch Ziehen

[`CarouselView`](xref:Xamarin.Forms.CarouselView) unterstützt die Funktion zum Aktualisieren von Pull zum Aktualisieren durch die `RefreshView`, wodurch die zu aktualisierenden Daten durch Abrufen der Elemente aktualisiert werden können. Der `RefreshView` ist ein Container Steuerelement, das dem untergeordneten Pullvorgang zur Aktualisierungs Funktion bereitstellt, vorausgesetzt, das untergeordnete Element unterstützt scrollbaren Inhalt. Der Pull-Vorgang zum Aktualisieren wird daher für eine `CarouselView` implementiert, indem Sie als untergeordnetes Element eines `RefreshView`festgelegt wird:

```xaml
<RefreshView IsRefreshing="{Binding IsRefreshing}"
             Command="{Binding RefreshCommand}">
    <CarouselView ItemsSource="{Binding Animals}">
        ...
    </CarouselView>
</RefreshView>
```

Der entsprechende C#-Code lautet:

```csharp
RefreshView refreshView = new RefreshView();
ICommand refreshCommand = new Command(() =>
{
    // IsRefreshing is true
    // Refresh data here
    refreshView.IsRefreshing = false;
});
refreshView.Command = refreshCommand;

CarouselView carouselView = new CarouselView();
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
refreshView.Content = carouselView;
// ...
```

Wenn der Benutzer eine Aktualisierung initiiert, wird die von der `Command`-Eigenschaft definierte `ICommand` ausgeführt, wodurch die angezeigten Elemente aktualisiert werden. Eine Aktualisierungs Visualisierung wird angezeigt, während die Aktualisierung durchgeführt wird, die aus einem animierten Fortschritts Kreis besteht:

[![Screenshot von "carouselview Pull-to-refresh" unter IOS und Android](populate-data-images/pull-to-refresh.png "Carouselview Pull-to-refresh")](populate-data-images/pull-to-refresh-large.png#lightbox "Carouselview Pull-to-refresh")

Der Wert der `RefreshView.IsRefreshing`-Eigenschaft gibt den aktuellen Status des `RefreshView`an. Wenn eine Aktualisierung durch den Benutzer ausgelöst wird, wird diese Eigenschaft automatisch zu `true`übergehen. Nachdem die Aktualisierung abgeschlossen ist, sollten Sie die-Eigenschaft auf `false`zurücksetzen.

Weitere Informationen zu `RefreshView`finden Sie unter [xamarin. Forms erfrischendes View](~/xamarin-forms/user-interface/refreshview.md).

## <a name="load-data-incrementally"></a>Inkrementelles Laden von Daten

[`CarouselView`](xref:Xamarin.Forms.CarouselView) unterstützt das inkrementelle Laden von Daten, wenn Benutzer durch Elemente scrollen. Dies ermöglicht Szenarios, wie z. b. das asynchrone Laden einer Datenseite aus einem Webdienst, während der Benutzer einen Bildlauf ausführt. Außerdem ist der Punkt, an dem mehr Daten geladen werden, so konfigurierbar, dass Benutzern kein leerer Bereich angezeigt wird oder der Bildlauf angehalten wird.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert die folgenden Eigenschaften, um das inkrementelle Laden von Daten zu steuern:

- `RemainingItemsThreshold`vom Typ `int`den Schwellenwert für Elemente, die noch nicht in der Liste angezeigt werden, bei der das `RemainingItemsThresholdReached` Ereignis ausgelöst wird.
- `RemainingItemsThresholdReachedCommand`vom Typ `ICommand`, der ausgeführt wird, wenn die `RemainingItemsThreshold` erreicht wird.
- `RemainingItemsThresholdReachedCommandParameter` vom Typ `object`: der Parameter, der an `RemainingItemsThresholdReachedCommand` übergeben wird.

[`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert auch ein `RemainingItemsThresholdReached` Ereignis, das ausgelöst wird, wenn der `CarouselView` einen Rollup hat, dass `RemainingItemsThreshold` Elemente nicht angezeigt wurden. Dieses Ereignis kann behandelt werden, um weitere Elemente zu laden. Wenn das `RemainingItemsThresholdReached`-Ereignis ausgelöst wird, wird außerdem der `RemainingItemsThresholdReachedCommand` ausgeführt, sodass inkrementelles Laden von Daten in einem ViewModel erfolgen kann.

Der Standardwert der `RemainingItemsThreshold`-Eigenschaft ist-1, was darauf hinweist, dass das `RemainingItemsThresholdReached` Ereignis nie ausgelöst wird. Wenn der-Eigenschafts Wert 0 ist, wird das `RemainingItemsThresholdReached` Ereignis ausgelöst, wenn das letzte Element in der [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) angezeigt wird. Bei Werten, die größer als 0 sind, wird das `RemainingItemsThresholdReached` Ereignis ausgelöst, wenn die `ItemsSource` die Anzahl der Elemente enthält, für die noch kein Rollup durchgeführt wurde.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) überprüft die `RemainingItemsThreshold`-Eigenschaft, sodass Ihr Wert immer größer oder gleich-1 ist.

Das folgende XAML-Beispiel zeigt eine [`CarouselView`](xref:Xamarin.Forms.CarouselView) , mit der Daten inkrementell geladen werden:

```xaml
<CarouselView ItemsSource="{Binding Animals}"
              RemainingItemsThreshold="2"
              RemainingItemsThresholdReached="OnCarouselViewRemainingItemsThresholdReached"
              RemainingItemsThresholdReachedCommand="{Binding LoadMoreDataCommand}">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    RemainingItemsThreshold = 2
};
carouselView.RemainingItemsThresholdReached += OnCollectionViewRemainingItemsThresholdReached;
carouselView.SetBinding(ItemsView.ItemsSourceProperty, "Animals");
```

In diesem Codebeispiel wird das `RemainingItemsThresholdReached`-Ereignis ausgelöst, wenn für zwei Elemente noch kein Rollup durchgeführt wurde, und in der Antwort wird der `OnCollectionViewRemainingItemsThresholdReached`-Ereignishandler ausgeführt:

```csharp
void OnCollectionViewRemainingItemsThresholdReached(object sender, EventArgs e)
{
    // Retrieve more data here and add it to the CollectionView's ItemsSource collection.
}
```

> [!NOTE]
> Daten können auch inkrementell geladen werden, indem die `RemainingItemsThresholdReachedCommand` an eine `ICommand` Implementierung in ViewModel gebunden wird.

## <a name="related-links"></a>Verwandte Links

- [Carouselview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Xamarin. Forms-Anzeige Ansicht](~/xamarin-forms/user-interface/indicatorview.md)
- [Xamarin. Forms-Ansicht "erfrischend"](~/xamarin-forms/user-interface/refreshview.md)
- [Xamarin. Forms-Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin. Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
