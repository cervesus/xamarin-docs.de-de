---
Title: "bindbare Layouts in Xamarin.Forms " Description: "bindbare Layouts ermöglichen layoutklassen das Generieren ihres Inhalts durch Binden an eine Auflistung von Elementen, mit der Option, die Darstellung der einzelnen Elemente mit einem DataTemplate festzulegen."
ms. Prod: xamarin ms. assetid: 824c3319-20A0-42d0-8632-cdecd98349c3 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 03/09/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="bindable-layouts-in-xamarinforms"></a>Bindbare Layouts inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)

Bindbare Layouts ermöglichen es allen layoutklassen, die von der-Klasse abgeleitet sind, den [`Layout<T>`](xref:Xamarin.Forms.Layout`1) Inhalt durch Binden an eine Auflistung von Elementen zu generieren. die Möglichkeit, die Darstellung der einzelnen Elemente mit einem festzulegen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) . Bindbare Layouts werden von der- `BindableLayout` Klasse bereitgestellt, die die folgenden angefügten Eigenschaften verfügbar macht:

- `ItemsSource`– Gibt die Sammlung von `IEnumerable` Elementen an, die vom Layout angezeigt werden sollen.
- `ItemTemplate`– Gibt das [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) an, das auf jedes Element in der Auflistung von Elementen angewendet werden soll, die vom Layout angezeigt werden.
- `ItemTemplateSelector`– Gibt das [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) an, das zum Auswählen eines [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit verwendet wird.

> [!NOTE]
> Die `ItemTemplate` -Eigenschaft hat Vorrang, wenn die `ItemTemplate` -Eigenschaft und die-Eigenschaft `ItemTemplateSelector` festgelegt sind.

Außerdem macht die- `BindableLayout` Klasse die folgenden bindbaren Eigenschaften verfügbar:

- `EmptyView`– Gibt die- `string` oder-Ansicht an, die angezeigt wird, wenn die- `ItemsSource` Eigenschaft ist `null` , oder wenn die von der-Eigenschaft angegebene Auflistung `ItemsSource` `null` oder leer ist. Standardwert: `null`.
- `EmptyViewTemplate`– Gibt den [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) an, der angezeigt wird, wenn die- `ItemsSource` Eigenschaft ist `null` , oder wenn die von der-Eigenschaft angegebene Auflistung `ItemsSource` `null` oder leer ist. Standardwert: `null`.

> [!NOTE]
> Die `EmptyViewTemplate` -Eigenschaft hat Vorrang, wenn die `EmptyView` -Eigenschaft und die-Eigenschaft `EmptyViewTemplate` festgelegt sind.

Alle diese Eigenschaften können an die [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) -,-, [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) [`Grid`](xref:Xamarin.Forms.Grid) -, [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) -und-Klassen angefügt werden [`StackLayout`](xref:Xamarin.Forms.StackLayout) , die alle von der-Klasse abgeleitet sind [`Layout<T>`](xref:Xamarin.Forms.Layout`1) .

Die-Klasse macht eine Auflistung verfügbar `Layout<T>` [`Children`](xref:Xamarin.Forms.Layout`1.Children) , der die untergeordneten Elemente eines Layouts hinzugefügt werden. Wenn die `BinableLayout.ItemsSource` Eigenschaft auf eine Auflistung von Elementen festgelegt und an eine [`Layout<T>`](xref:Xamarin.Forms.Layout`1) von abgeleitete Klasse angefügt wird, wird jedes Element in der Auflistung der Auflistung `Layout<T>.Children` für die Anzeige durch das Layout hinzugefügt. Die von `Layout<T>` abgeleitete Klasse aktualisiert dann die untergeordneten Sichten, wenn sich die zugrunde liegende Auflistung ändert. Weitere Informationen zum Xamarin.Forms layoutcycle finden Sie unter [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md).

Bindbare Layouts sollten nur verwendet werden, wenn die Auflistung von Elementen, die angezeigt werden sollen, klein ist und ein Bildlauf und eine Auswahl nicht erforderlich sind. Wenn Sie einen Bildlauf durchführen können, indem Sie ein bindbare Layout in einem umgestalten [`ScrollView`](xref:Xamarin.Forms.ScrollView) , wird dies nicht empfohlen, da es bei bindbaren Layouts keine UI-Virtualisierung Wenn ein Bildlauf erforderlich ist, sollte eine Bild lauffähige Sicht verwendet werden, die die UI-Virtualisierung wie [`ListView`](xref:Xamarin.Forms.ListView) oder enthält [`CollectionView`](xref:Xamarin.Forms.CollectionView) . Wenn Sie diese Empfehlung nicht beachten, kann dies zu Leistungsproblemen führen.

> [!IMPORTANT]
> Obwohl es technisch möglich ist, ein bindbares Layout an eine beliebige Layoutklasse anzufügen, die von der- [`Layout<T>`](xref:Xamarin.Forms.Layout`1) Klasse abgeleitet ist, ist dies nicht immer praktisch, insbesondere für die [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) [`Grid`](xref:Xamarin.Forms.Grid) Klassen, und [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) . Angenommen, Sie möchten eine Auflistung von Daten in einem [`Grid`](xref:Xamarin.Forms.Grid) mithilfe eines bindbaren Layouts anzeigen, wobei jedes Element in der Auflistung ein Objekt mit mehreren Eigenschaften ist. Jede Zeile in der `Grid` sollte ein Objekt aus der Auflistung anzeigen, wobei jede Spalte in der `Grid` eine der Eigenschaften des Objekts anzeigt. Da der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für das bindbare Layout nur ein einzelnes Objekt enthalten kann, ist es erforderlich, dass dieses Objekt eine Layoutklasse mit mehreren Ansichten ist, die jeweils eine der Eigenschaften des Objekts in einer bestimmten `Grid` Spalte anzeigen. Obwohl dieses Szenario mit bindbaren Layouts realisiert werden kann, führt es zu einem übergeordneten Element, das ein untergeordnetes `Grid` `Grid` Element für jedes Element in der gebundenen Auflistung enthält. Dies ist eine hochgradig ineffiziente und problematische Verwendung des `Grid` Layouts.

## <a name="populate-a-bindable-layout-with-data"></a>Auffüllen eines bindbaren Layouts mit Daten

Ein bindbares Layout wird mit Daten aufgefüllt, indem die `ItemsSource` zugehörige-Eigenschaft auf eine beliebige Auflistung festgelegt wird, die implementiert `IEnumerable` , und an eine von [`Layout<T>`](xref:Xamarin.Forms.Layout`1) abgeleitete Klasse angehängt wird:

```xaml
<Grid BindableLayout.ItemsSource="{Binding Items}" />
```

Der entsprechende C#-Code lautet:

```csharp
IEnumerable<string> items = ...;
var grid = new Grid();
BindableLayout.SetItemsSource(grid, items);
```

Wenn die `BindableLayout.ItemsSource` angefügte-Eigenschaft für ein Layout festgelegt ist, die `BindableLayout.ItemTemplate` angefügte-Eigenschaft jedoch nicht festgelegt ist, wird jedes Element in der Auflistung `IEnumerable` von einem angezeigt, [`Label`](xref:Xamarin.Forms.Label) das von der-Klasse erstellt wird `BindableLayout` .

## <a name="define-item-appearance"></a>Element Darstellung definieren

Die Darstellung der einzelnen Elemente im bindbaren Layout kann definiert werden, indem die `BindableLayout.ItemTemplate` angefügte-Eigenschaft auf festgelegt wird [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding User.TopFollowers}"
             Orientation="Horizontal"
             ...>
    <BindableLayout.ItemTemplate>
        <DataTemplate>
            <controls:CircleImage Source="{Binding}"
                                  Aspect="AspectFill"
                                  WidthRequest="44"
                                  HeightRequest="44"
                                  ... />
        </DataTemplate>
    </BindableLayout.ItemTemplate>
</StackLayout>
```

Der entsprechende C#-Code lautet:

```csharp
DataTemplate circleImageTemplate = ...;
var stackLayout = new StackLayout();
BindableLayout.SetItemsSource(stackLayout, viewModel.User.TopFollowers);
BindableLayout.SetItemTemplate(stackLayout, circleImageTemplate);
```

In diesem Beispiel wird jedes Element in der Auflistung `TopFollowers` von einer Ansicht angezeigt, `CircleImage` die in definiert ist [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

![Bindable Layout mit einem DataTemplate](bindable-layouts-images/top-followers.png "Bindable Layout mit einer Daten Vorlage")

Weitere Informationen zu Datenvorlagen finden Sie unter [Xamarin.Forms-Datenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente im bindbaren Layout kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die `BindableLayout.ItemTemplateSelector` angefügte-Eigenschaft auf festgelegt wird [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) :

```xaml
<FlexLayout BindableLayout.ItemsSource="{Binding User.FavoriteTech}"
            BindableLayout.ItemTemplateSelector="{StaticResource TechItemTemplateSelector}"
            ... />
```

Der entsprechende C#-Code lautet:

```csharp
DataTemplateSelector dataTemplateSelector = new TechItemTemplateSelector { ... };
var flexLayout = new FlexLayout();
BindableLayout.SetItemsSource(flexLayout, viewModel.User.FavoriteTech);
BindableLayout.SetItemTemplateSelector(flexLayout, dataTemplateSelector);
```

Das [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) in der Beispielanwendung verwendete ist im folgenden Beispiel dargestellt:

```csharp
public class TechItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinFormsTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return (string)item == "Xamarin.Forms" ? XamarinFormsTemplate : DefaultTemplate;
    }
}
```

Die `TechItemTemplateSelector` -Klasse definiert die `DefaultTemplate` `XamarinFormsTemplate` [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Eigenschaften und, die auf unterschiedliche Datenvorlagen festgelegt sind. Die- `OnSelectTemplate` Methode gibt die zurück `XamarinFormsTemplate` , die ein Element in einem dunklen rot mit einem anderen Herz anzeigt, wenn das Element gleich " Xamarin.Forms " ist. Wenn das Element nicht gleich " Xamarin.Forms " ist, `OnSelectTemplate` gibt die Methode den zurück `DefaultTemplate` , der ein Element mit der Standardfarbe eines anzeigt [`Label`](xref:Xamarin.Forms.Label) :

![Bindable Layout mit DataTemplateSelector](bindable-layouts-images/favorite-tech.png "Bindable Layout mit Datenvorlagen Auswahl")

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Creating a Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="display-a-string-when-data-is-unavailable"></a>Zeigt eine Zeichenfolge an, wenn Daten nicht verfügbar sind.

Die- `EmptyView` Eigenschaft kann auf eine Zeichenfolge festgelegt werden, die von einem angezeigt wird, [`Label`](xref:Xamarin.Forms.Label) Wenn die- `ItemsSource` Eigenschaft ist `null` , oder wenn die von der-Eigenschaft angegebene Auflistung den Wert `ItemsSource` `null` oder leer ist. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}"
             BindableLayout.EmptyView="No achievements">
    ...
</StackLayout>
```

Das Ergebnis ist, dass bei der Daten gebundenen Auflistung `null` die Zeichenfolge, die als `EmptyView` Eigenschafts Wert festgelegt ist, angezeigt wird:

[![Screenshot einer bindbaren layoutzeichenfolge leere Ansicht unter IOS und Android](bindable-layouts-images/emptyview-string.png "Bindbare layoutzeichenfolge leere Ansicht")](bindable-layouts-images/emptyview-string-large.png#lightbox "Bindbare layoutzeichenfolge leere Ansicht")

## <a name="display-views-when-data-is-unavailable"></a>Anzeigen von Sichten, wenn Daten nicht verfügbar sind

Die- `EmptyView` Eigenschaft kann auf eine Ansicht festgelegt werden, die angezeigt wird, wenn die- `ItemsSource` Eigenschaft ist `null` , oder wenn die von der-Eigenschaft angegebene Auflistung `ItemsSource` `null` oder leer ist. Dabei kann es sich um eine einzelne Ansicht oder eine Ansicht handeln, die mehrere untergeordnete Sichten enthält. Das folgende XAML-Beispiel zeigt, wie die- `EmptyView` Eigenschaft auf eine Ansicht festgelegt ist, die mehrere untergeordnete Sichten enthält:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyView>
        <StackLayout>
            <Label Text="None."
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
            <Label Text="Try harder and return later?"
                   FontAttributes="Italic"
                   FontSize="{StaticResource smallTextSize}" />
        </StackLayout>
    </BindableLayout.EmptyView>
    ...
</StackLayout>
```

Das Ergebnis ist, dass bei der Daten gebundenen Auflistung `null` die [`StackLayout`](xref:Xamarin.Forms.StackLayout) und ihre untergeordneten Sichten angezeigt werden.

[![Screenshot einer leeren Ansicht mit bindbarem Layout mit mehreren Ansichten unter IOS und Android](bindable-layouts-images/emptyview-views.png "Leere Ansicht des bindbaren Layouts")](bindable-layouts-images/emptyview-views-large.png#lightbox "Leere Ansicht des bindbaren Layouts")

Ebenso kann die `EmptyViewTemplate` auf einen festgelegt werden [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , der angezeigt wird, wenn die- `ItemsSource` Eigenschaft ist `null` , oder wenn die von der- `ItemsSource` Eigenschaft angegebene Auflistung `null` oder leer ist. Die `DataTemplate` kann eine einzelne Ansicht oder eine Sicht enthalten, die mehrere untergeordnete Sichten enthält. Außerdem wird der von der von `BindingContext` `EmptyViewTemplate` geerbt `BindingContext` `BindableLayout` . Das folgende XAML-Beispiel zeigt, wie die- `EmptyViewTemplate` Eigenschaft auf festgelegt ist `DataTemplate` , die eine einzelne Ansicht enthält:

```xaml
<StackLayout BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
    <BindableLayout.EmptyViewTemplate>
        <DataTemplate>
            <Label Text="{Binding Source={x:Reference usernameLabel}, Path=Text, StringFormat='{0} has no achievements.'}" />
        </DataTemplate>
    </BindableLayout.EmptyViewTemplate>
    ...
</StackLayout>
```

Das Ergebnis ist, dass `null` die [`Label`](xref:Xamarin.Forms.Label) in der angezeigt wird, wenn die Daten gebundene Auflistung ist [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) :

[![Screenshot einer leeren Ansichts Vorlage für das bindbare Layout unter IOS und Android](bindable-layouts-images/emptyviewtemplate.png "Vorlage für leere Vorlage mit bindbarem Layout")](bindable-layouts-images/emptyviewtemplate-large.png#lightbox "Vorlage für leere Vorlage mit bindbarem Layout")

> [!NOTE]
> Die- `EmptyViewTemplate` Eigenschaft kann nicht über einen festgelegt werden [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) .

## <a name="choose-an-emptyview-at-runtime"></a>Emptyview zur Laufzeit auswählen

Sichten, die als angezeigt werden `EmptyView` , wenn Daten nicht verfügbar sind, können in einer als-Objekte definiert werden [`ContentView`](xref:Xamarin.Forms.ContentView) [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Die- `EmptyView` Eigenschaft kann dann zur Laufzeit auf einen bestimmten festgelegt werden `ContentView` , der auf einer Geschäftslogik basiert. Der folgende XAML-Code zeigt ein Beispiel für dieses Szenario:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...    
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No achievements."
                       FontSize="14" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="None."
                       FontAttributes="Italic"
                       FontSize="14" />
                <Label Text="Try harder and return later?"
                       FontAttributes="Italic"
                       FontSize="14" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout>
        ...
        <Switch Toggled="OnEmptyViewSwitchToggled" />

        <StackLayout x:Name="stackLayout"
                     BindableLayout.ItemsSource="{Binding UserWithoutAchievements.Achievements}">
            ...
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Der XAML-Code definiert zwei [`ContentView`](xref:Xamarin.Forms.ContentView) Objekte auf Seitenebene [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) , wobei das [`Switch`](xref:Xamarin.Forms.Switch) Objekt steuert, welches `ContentView` Objekt als `EmptyView` Eigenschafts Wert festgelegt wird. Wenn das-Objekt ein-/ausgeschaltet `Switch` wird, `OnEmptyViewSwitchToggled` führt der Ereignishandler die- `ToggleEmptyView` Methode aus:

```csharp
void ToggleEmptyView(bool isToggled)
{
    object view = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
    BindableLayout.SetEmptyView(stackLayout, view);
}
```

Die- `ToggleEmptyView` Methode legt die- `EmptyView` Eigenschaft des- `stackLayout` Objekts basierend auf dem Wert der-Eigenschaft auf eines der beiden- [`ContentView`](xref:Xamarin.Forms.ContentView) Objekte fest, die im gespeichert [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) sind [`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) . Wenn die Daten gebundene Auflistung ist `null` , wird das Objekt, das `ContentView` als-Eigenschaft festgelegt ist, `EmptyView` angezeigt:

[![Screenshot der Auswahl leerer Ansichten zur Laufzeit unter IOS und Android](bindable-layouts-images/emptyview-runtime.png "Bindable Layout leere Ansicht Lauf Zeitauswahl")](bindable-layouts-images/emptyview-runtime-large.png#lightbox "Bindable Layout leere Ansicht Lauf Zeitauswahl")

## <a name="related-links"></a>Verwandte Links

- [Demo zu bindbaren Layouts (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Erstellen eines benutzerdefinierten Layouts](~/xamarin-forms/user-interface/layouts/custom.md)
- [Xamarin.FormsDatenvorlagen](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Erstellen eines Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
