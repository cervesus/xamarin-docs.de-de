---
title: Xamarin.FormsNetz
description: Das Xamarin.Forms Raster ist ein Layout, das seine untergeordneten Elemente in Zeilen und Spalten von Zellen organisiert.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9d2e697a07e033fd7c3c8d3efffa1d67f6c097c3
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946337"
---
# <a name="xamarinforms-grid"></a>Xamarin.FormsNetz

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-griddemos)

[![Xamarin.FormsNetz](grid-images/layouts.png "[! Schel. Raster "NO-LOC (xamarin. Forms)]"")](grid-images/layouts-large.png#lightbox "[! Schel. Raster "NO-LOC (xamarin. Forms)]"")

Bei [`Grid`](xref:Xamarin.Forms.Grid) handelt es sich um ein Layout, das seine untergeordneten Elemente in Zeilen und Spalten organisiert, die eine proportionale oder absolute Größe aufweisen können. Standardmäßig enthält eine `Grid` Zeile und eine Spalte. Außerdem `Grid` kann ein als übergeordnetes Layout verwendet werden, das andere untergeordnete Layouts enthält.

Das [`Grid`](xref:Xamarin.Forms.Grid) Layout sollte nicht mit Tabellen verwechselt werden und ist nicht für die Darstellung von Tabellendaten vorgesehen. Im Gegensatz zu HTML-Tabellen ist ein zum Anordnen `Grid` von Inhalt vorgesehen. Zum Anzeigen von Tabellendaten sollten Sie die Verwendung von [ListView](~/xamarin-forms/user-interface/listview/index.md), [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)oder [TableView](~/xamarin-forms/user-interface/tableview.md)in Erwägung gezogen.

Die- [`Grid`](xref:Xamarin.Forms.Grid) Klasse definiert die folgenden Eigenschaften:

- [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty)vom Typ `int` , bei dem es sich um eine angefügte Eigenschaft handelt, die die Spalten Ausrichtung einer Ansicht in einem übergeordneten Element angibt `Grid` . Der Standardwert dieser Eigenschaft ist 0. Ein Validierungs Rückruf stellt sicher, dass der Wert, wenn die Eigenschaft festgelegt wird, größer oder gleich 0 ist.
- [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions)ist vom Typ [`ColumnDefinitionCollection`](xref:Xamarin.Forms.ColumnDefinitionCollection) eine Liste von- [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) Objekten, die die Breite der Raster Spalten definieren.
- [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing)`double`gibt den Abstand zwischen Raster Spalten an. Der Standardwert dieser Eigenschaft ist 6 geräteunabhängige Einheiten.
- [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)vom Typ `int` , bei dem es sich um eine angefügte Eigenschaft handelt, die die Gesamtzahl der Spalten angibt, die eine Sicht innerhalb eines übergeordneten Elements umfasst `Grid` . Der Standardwert dieser Eigenschaft ist „1“. Ein Validierungs Rückruf stellt sicher, dass der Wert, wenn die Eigenschaft festgelegt wird, größer oder gleich 1 ist.
- [`Row`](xref:Xamarin.Forms.Grid.RowProperty)vom Typ `int` , bei dem es sich um eine angefügte Eigenschaft handelt, die die Zeilen Ausrichtung einer Sicht innerhalb eines übergeordneten Elements angibt `Grid` . Der Standardwert dieser Eigenschaft ist 0. Ein Validierungs Rückruf stellt sicher, dass der Wert, wenn die Eigenschaft festgelegt wird, größer oder gleich 0 ist.
- [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions)ist vom Typ [`RowDefinitionCollection`](xref:Xamarin.Forms.RowDefinitionCollection) eine Liste von- [`RowDefintion`](xref:Xamarin.Forms.RowDefinition) Objekten, die die Höhe der Raster Zeilen definieren.
- [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing)`double`gibt den Abstand zwischen Raster Zeilen an. Der Standardwert dieser Eigenschaft ist 6 geräteunabhängige Einheiten.
- [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)vom Typ `int` , bei dem es sich um eine angefügte Eigenschaft handelt, die die Gesamtzahl der Zeilen angibt, die eine Sicht innerhalb eines übergeordneten Elements umfasst `Grid` . Der Standardwert dieser Eigenschaft ist „1“. Ein Validierungs Rückruf stellt sicher, dass der Wert, wenn die Eigenschaft festgelegt wird, größer oder gleich 1 ist.

Diese Eigenschaften werden von- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen und formatierten sein können.

Die- [`Grid`](xref:Xamarin.Forms.Grid) Klasse wird von der- `Layout<T>` Klasse abgeleitet, die eine `Children` Eigenschaft vom Typ definiert `IList<T>` . Die `Children` -Eigenschaft ist der der `ContentProperty` `Layout<T>` -Klasse und muss daher nicht explizit aus XAML festgelegt werden.

> [!TIP]
> Um die bestmögliche layoutleistung zu erzielen, befolgen Sie die Richtlinien unter [Optimieren der layoutleistung](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance).

## <a name="rows-and-columns"></a>Zeilen und Spalten

Standardmäßig enthält eine [`Grid`](xref:Xamarin.Forms.Grid) Zeile und eine Spalte:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridTutorial.MainPage">
    <Grid Margin="20,35,20,20">
        <Label Text="By default, a Grid contains one row and one column." />
    </Grid>
</ContentPage>
```

In diesem Beispiel enthält das [`Grid`](xref:Xamarin.Forms.Grid) ein einzelnes untergeordnetes Element [`Label`](xref:Xamarin.Forms.Label) , das sich automatisch an einem einzelnen Speicherort befindet:

[![Screenshot eines Standard Raster Layouts](grid-images/default.png "Standard Raster Layout")](grid-images/default-large.png#lightbox "Standard Raster Layout")

Das Layoutverhalten eines [`Grid`](xref:Xamarin.Forms.Grid) kann mit der-Eigenschaft und der-Eigenschaft definiert werden, bei denen es sich um Auflistungen [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) von [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) - [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) Objekten bzw. Diese Auflistungen definieren die Zeilen-und Spalten Eigenschaften eines `Grid` und sollten ein- [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) Objekt für jede Zeile in der `Grid` sowie ein- [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) Objekt für jede Spalte in der enthalten `Grid` .

Die [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) -Klasse definiert eine [`Height`](xref:Xamarin.Forms.RowDefinition.Height) Eigenschaft vom Typ [`GridLength`](xref:Xamarin.Forms.GridLength) , und die- [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) Klasse definiert eine [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) Eigenschaft vom Typ [`GridLength`](xref:Xamarin.Forms.GridLength) . Die [`GridLength`](xref:Xamarin.Forms.GridLength) Struktur gibt eine Zeilenhöhe oder eine Spaltenbreite in Bezug auf die- [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) Enumeration an, die über drei Member verfügt:

- `Absolute`– die Zeilenhöhe oder Spaltenbreite ist ein Wert in geräteunabhängigen Einheiten (eine Zahl in XAML).
- `Auto`– die Zeilenhöhe oder Spaltenbreite wird basierend auf dem Zellen Inhalt ( `Auto` in XAML) automatisch vergrößert.
- `Star`– die Zeilenhöhe oder Spaltenbreite wird proportional zugewiesen (eine Zahl, gefolgt von `*` in XAML).

Eine [`Grid`](xref:Xamarin.Forms.Grid) Zeile mit `Height` der-Eigenschaft `Auto` schränkt die Höhe von Sichten in dieser Zeile auf die gleiche Weise ein, wie ein vertikaler Wert [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Entsprechend funktioniert eine Spalte mit `Width` der-Eigenschaft ähnlich `Auto` wie eine horizontale `StackLayout` .

> [!CAUTION]
> Stellen Sie sicher, dass so wenige Zeilen und Spalten wie möglich auf Größe festgelegt sind [`Auto`](xref:Xamarin.Forms.GridLength.Auto) . Durch jede Zeile oder Spalte, deren Größe automatisch angepasst wird, wird verursacht, dass die Layout-Engine zusätzliche Layoutberechnungen durchführt. Verwenden Sie stattdessen wenn möglich Zeilen und Spalten mit festen Größen. Alternativ können Sie Zeilen und Spalten festlegen, um eine proportionale Menge an Speicherplatz mit dem- [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) Enumerationswert zu belegen.

Der folgende XAML-Code zeigt, wie Sie eine [`Grid`](xref:Xamarin.Forms.Grid) mit drei Zeilen und zwei Spalten erstellen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.BasicGridPage"
             Title="Basic Grid demo">
   <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

In diesem Beispiel [`Grid`](xref:Xamarin.Forms.Grid) hat eine Gesamthöhe, die die Höhe der Seite ist. Der `Grid` weiß, dass die Höhe der dritten Zeile 100 geräteunabhängige Einheiten ist. Diese Höhe wird von der eigenen Höhe subtrahiert, und die verbleibende Höhe wird proportional zwischen der ersten und zweiten Zeile basierend auf der Zahl vor dem Stern zugewiesen. In diesem Beispiel entspricht die Höhe der ersten Zeile zweimal der zweiten Zeile.

Die beiden- [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) Objekte legen [`Width`](xref:Xamarin.Forms.ColumnDefinition.Width) auf fest `*` , das entspricht `1*` , was bedeutet, dass die Breite des Bildschirms gleich unterhalb der beiden Spalten aufgeteilt ist.

> [!IMPORTANT]
> Der Standardwert der- [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) Eigenschaft ist `*` . Entsprechend lautet der Standardwert der- [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) Eigenschaft `*` . Daher ist es nicht erforderlich, diese Eigenschaften in Fällen festzulegen, in denen diese Standardwerte zulässig sind.

Untergeordnete Sichten können [`Grid`](xref:Xamarin.Forms.Grid) mit den [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) angefügten Eigenschaften und in bestimmten Zellen positioniert werden [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) . Verwenden Sie außerdem die [`Grid.RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) angefügten Eigenschaften und, um die untergeordneten Sichten auf mehrere Zeilen und Spalten zu überspannen [`Grid.ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) .

Der folgende XAML-Code zeigt dieselbe [`Grid`](xref:Xamarin.Forms.Grid) Definition und positioniert auch untergeordnete Sichten in bestimmten `Grid` Zellen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.BasicGridPage"
             Title="Basic Grid demo">
   <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <BoxView Color="Green" />
        <Label Text="Row 0, Column 0"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Column="1"
                 Color="Blue" />
        <Label Grid.Column="1"
               Text="Row 0, Column 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Color="Teal" />
        <Label Grid.Row="1"
               Text="Row 1, Column 0"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="1"
                 Color="Purple" />
        <Label Grid.Row="1"
               Grid.Column="1"
               Text="Row1, Column 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="2"
                 Grid.ColumnSpan="2"
                 Color="Red" />
        <Label Grid.Row="2"
               Grid.ColumnSpan="2"
               Text="Row 2, Columns 0 and 1"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Grid>
</ContentPage>
```

> [!NOTE]
> Die `Grid.Row` -Eigenschaft und die-Eigenschaft `Grid.Column` werden beide von 0 indiziert und `Grid.Row="2"` verweisen auf die dritte Zeile, während `Grid.Column="1"` auf die zweite Spalte verweist. Außerdem haben beide Eigenschaften den Standardwert 0 und müssen daher nicht für untergeordnete Sichten festgelegt werden, die die erste Zeile oder erste Spalte eines belegen [`Grid`](xref:Xamarin.Forms.Grid) .

In diesem Beispiel werden alle drei [`Grid`](xref:Xamarin.Forms.Grid) Zeilen von [`BoxView`](xref:Xamarin.Forms.BoxView) -und- [`Label`](xref:Xamarin.Forms.Label) Sichten belegt. Die dritte Zeile ist 100 geräteunabhängige Einheiten hoch, wobei die ersten beiden Zeilen den verbleibenden Platz belegen (die erste Zeile ist doppelt so hoch wie die zweite Zeile). Die beiden Spalten sind gleich Breite und Dividieren den `Grid` in der Hälfte. Die `BoxView` in der dritten Zeile umfasst beide Spalten.

[![Screenshot eines grundlegenden Raster Layouts](grid-images/basic.png "Grundlegendes Raster Layout")](grid-images/basic-large.png#lightbox "Grundlegendes Raster Layout")

Darüber hinaus können untergeordnete Sichten in einem [`Grid`](xref:Xamarin.Forms.Grid) Zellen gemeinsam verwenden. Die Reihenfolge, in der die untergeordneten Elemente im XAML angezeigt werden, ist die Reihenfolge, in der die untergeordneten Elemente abgelegt werden `Grid` Im vorherigen Beispiel sind die- [`Label`](xref:Xamarin.Forms.Label) Objekte nur sichtbar, da Sie auf den-Objekten gerendert werden [`BoxView`](xref:Xamarin.Forms.BoxView) . Die `Label` Objekte sind nicht sichtbar, wenn die `BoxView` Objekte darauf gerendert wurden.

Der entsprechende C#-Code lautet:

```csharp
public class BasicGridPageCS : ContentPage
{
    public BasicGridPageCS()
    {
        Grid grid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition { Height = new GridLength(2, GridUnitType.Star) },
                new RowDefinition(),
                new RowDefinition { Height = new GridLength(100) }
            },
            ColumnDefinitions =
            {
                new ColumnDefinition(),
                new ColumnDefinition()
            }
        };

        // Row 0
        // The BoxView and Label are in row 0 and column 0, and so only needs to be added to the
        // Grid.Children collection to get default row and column settings.
        grid.Children.Add(new BoxView
        {
            Color = Color.Green
        });
        grid.Children.Add(new Label
        {
            Text = "Row 0, Column 0",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        });

        // This BoxView and Label are in row 0 and column 1, which are specified as arguments
        // to the Add method.
        grid.Children.Add(new BoxView
        {
            Color = Color.Blue
        }, 1, 0);
        grid.Children.Add(new Label
        {
            Text = "Row 0, Column 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 0);

        // Row 1
        // This BoxView and Label are in row 1 and column 0, which are specified as arguments
        // to the Add method overload.
        grid.Children.Add(new BoxView
        {
            Color = Color.Teal
        }, 0, 1, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Row 1, Column 0",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 0, 1, 1, 2); // These arguments indicate that that the child element goes in the column starting at 0 but ending before 1.
                        // They also indicate that the child element goes in the row starting at 1 but ending before 2.

        grid.Children.Add(new BoxView
        {
            Color = Color.Purple
        }, 1, 2, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Row1, Column 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 2, 1, 2);

        // Row 2
        // Alternatively, the BoxView and Label can be positioned in cells with the Grid.SetRow
        // and Grid.SetColumn methods.
        BoxView boxView = new BoxView { Color = Color.Red };
        Grid.SetRow(boxView, 2);
        Grid.SetColumnSpan(boxView, 2);
        Label label = new Label
        {
            Text = "Row 2, Column 0 and 1",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        };
        Grid.SetRow(label, 2);
        Grid.SetColumnSpan(label, 2);

        grid.Children.Add(boxView);
        grid.Children.Add(label);

        Title = "Basic Grid demo";
        Content = grid;
    }
}
```

Wenn Sie im Code die Höhe eines [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) -Objekts und die Breite eines-Objekts angeben möchten, [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) verwenden Sie die Werte der- [`GridLength`](xref:Xamarin.Forms.GridLength) Struktur, häufig in Kombination mit der- [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) Enumeration.

Der obige Beispielcode zeigt auch mehrere verschiedene Ansätze zum Hinzufügen von untergeordneten Elementen zum [`Grid`](xref:Xamarin.Forms.Grid) und zum Angeben der Zellen, in denen Sie sich befinden. Wenn Sie die-Überladung verwenden, die die `Add` Argumente *left*, *right*, *Top*und *Bottom* angibt, während das *linke* und das *oberste* Argument immer auf Zellen innerhalb von verweisen `Grid` , werden das *Rechte* und das *untere* Argument auf Zellen, die sich außerhalb des befinden, angezeigt `Grid` . Dies liegt daran, dass das *Rechte* Argument immer größer sein muss als das *linke* Argument, und das *untere* Argument muss immer größer sein als das *oberste* Argument. Im folgenden Beispiel, in dem ein 2 x 2-Element angenommen `Grid` wird, wird der entsprechende Code mithilfe beider- `Add` über Ladungen angezeigt:

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);           // first column, first row
grid.Children.Add(topRight, 1, 0);          // second column, first tow
grid.Children.Add(bottomLeft, 0, 1);        // first column, second row
grid.Children.Add(bottomRight, 1, 1);       // second column, second row

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);     // first column, first row
grid.Children.Add(topRight, 1, 2, 0, 1);    // second column, first tow
grid.Children.Add(bottomLeft, 0, 1, 1, 2);  // first column, second row
grid.Children.Add(bottomRight, 1, 2, 1, 2); // second column, second row
```

> [!NOTE]
> Darüber hinaus können [`Grid`](xref:Xamarin.Forms.Grid) mit den [`AddHorizontal`](xref:Xamarin.Forms.Grid.IGridList`1.AddHorizontal*) -und-Methoden, die untergeordnete Sichten zu [`AddVertical`](xref:Xamarin.Forms.Grid.IGridList`1.AddVertical*) einer einzelnen Zeile oder einzelnen Spalten hinzufügen, eine untergeordnete Ansicht hinzugefügt werden `Grid` . Der wird `Grid` dann in Zeilen oder Spalten erweitert, wenn diese Aufrufe durchgeführt werden, und die untergeordneten Elemente werden automatisch in den richtigen Zellen positioniert.

### <a name="simplify-row-and-column-definitions"></a>Vereinfachen von Zeilen-und Spaltendefinitionen

In XAML können die Zeilen-und Spalten Eigenschaften von [`Grid`](xref:Xamarin.Forms.Grid) mit einer vereinfachten Syntax angegeben werden, die das definieren [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) von-und- [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) Objekten für jede Zeile und Spalte vermeidet. Stattdessen können die [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) -Eigenschaft und die-Eigenschaft auf Zeichen folgen [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) mit durch Trennzeichen getrennten Werten festgelegt werden [`GridUnitType`](xref:Xamarin.Forms.GridUnitType) , aus denen Typkonverter in Xamarin.Forms Create `RowDefinition` -und-Objekte integriert werden `ColumnDefinition` :

```xaml
<Grid RowDefinitions="1*, Auto, 25, 14, 20"
      ColumnDefinitions="*, 2*, Auto, 300">
    ...
</Grid>
```

In diesem Beispiel [`Grid`](xref:Xamarin.Forms.Grid) verfügt über fünf Zeilen und vier Spalten. Die Dritten, nächsten und fünften Zeilen werden auf absolute Höhen festgelegt, wobei die automatische Größenanpassung der zweiten Zeile auf ihren Inhalt festgelegt wird. Die verbleibende Höhe wird dann der ersten Zeile zugeordnet.

Die Spalte weiter wird auf eine absolute Breite festgelegt, wobei die automatische Größenanpassung der dritten Spalte auf ihren Inhalt festgelegt ist. Die verbleibende Breite wird proportional zwischen der ersten und der zweiten Spalte basierend auf der Zahl vor dem Stern zugewiesen. In diesem Beispiel ist die Breite der zweiten Spalte zweimal das der ersten Spalte (da `*` mit identisch ist `1*` ).

## <a name="space-between-rows-and-columns"></a>Leerraum zwischen Zeilen und Spalten

Standardmäßig [`Grid`](xref:Xamarin.Forms.Grid) werden Zeilen durch 6 geräteunabhängige Speichereinheiten getrennt. Ebenso `Grid` sind Spalten durch 6 geräteunabhängige Speichereinheiten voneinander getrennt. Diese Standardwerte können durch Festlegen der-Eigenschaft und der-Eigenschaft geändert werden [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.GridSpacingPage"
             Title="Grid spacing demo">
    <Grid RowSpacing="0"
          ColumnSpacing="0">
        ..
    </Grid>
</ContentPage>
```

In diesem Beispiel wird ein erstellt [`Grid`](xref:Xamarin.Forms.Grid) , der keinen Abstand zwischen seinen Zeilen und Spalten hat:

[![Screenshot des Rasters ohne Abstand zwischen den Zellen](grid-images/spacing.png "Raster ohne Abstand zwischen Zellen")](grid-images/spacing-large.png#lightbox "Raster ohne Abstand zwischen Zellen")

> [!TIP]
> Die [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) -Eigenschaft und die-Eigenschaft [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) können auf negative Werte festgelegt werden, um Zell Inhalte zu überlappen.

Der entsprechende C#-Code lautet:

```csharp
public GridSpacingPageCS()
{
    Grid grid = new Grid
    {
        RowSpacing = 0,
        ColumnSpacing = 0,
        // ...
    };
    // ...

    Content = grid;
}
```

## <a name="alignment"></a>Ausrichtung

Untergeordnete Sichten in einer [`Grid`](xref:Xamarin.Forms.Grid) können mithilfe der-Eigenschaft und der-Eigenschaft in ihren Zellen positioniert werden [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) . Diese Eigenschaften können auf die folgenden Felder aus der Struktur festgelegt werden [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) :

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)

> [!IMPORTANT]
> Die `AndExpands` Felder in der [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) Struktur können nur auf Objekte angewendet werden [`StackLayout`](xref:Xamarin.Forms.StackLayout) .

Der folgende XAML-Code erstellt eine [`Grid`](xref:Xamarin.Forms.Grid) mit neun Zellen gleicher Größe und platziert eine [`Label`](xref:Xamarin.Forms.Label) in jeder Zelle mit einer anderen Ausrichtung:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="GridDemos.Views.GridAlignmentPage"
             Title="Grid alignment demo">
    <Grid RowSpacing="0"
          ColumnSpacing="0">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>

        <BoxView Color="AliceBlue" />
        <Label Text="Upper left"
               HorizontalOptions="Start"
               VerticalOptions="Start" />
        <BoxView Grid.Column="1"
                 Color="LightSkyBlue" />
        <Label Grid.Column="1"
               Text="Upper center"
               HorizontalOptions="Center"
               VerticalOptions="Start"/>
        <BoxView Grid.Column="2"
                 Color="CadetBlue" />
        <Label Grid.Column="2"
               Text="Upper right"
               HorizontalOptions="End"
               VerticalOptions="Start" />
        <BoxView Grid.Row="1"
                 Color="CornflowerBlue" />
        <Label Grid.Row="1"
               Text="Center left"
               HorizontalOptions="Start"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="1"
                 Color="DodgerBlue" />
        <Label Grid.Row="1"
               Grid.Column="1"
               Text="Center center"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <BoxView Grid.Row="1"
                 Grid.Column="2"
                 Color="DarkSlateBlue" />
        <Label Grid.Row="1"
               Grid.Column="2"
               Text="Center right"
               HorizontalOptions="End"
               VerticalOptions="Center" />
        <BoxView Grid.Row="2"
                 Color="SteelBlue" />
        <Label Grid.Row="2"
               Text="Lower left"
               HorizontalOptions="Start"
               VerticalOptions="End" />
        <BoxView Grid.Row="2"
                 Grid.Column="1"
                 Color="LightBlue" />
        <Label Grid.Row="2"
               Grid.Column="1"
               Text="Lower center"
               HorizontalOptions="Center"
               VerticalOptions="End" />
        <BoxView Grid.Row="2"
                 Grid.Column="2"
                 Color="BlueViolet" />
        <Label Grid.Row="2"
               Grid.Column="2"
               Text="Lower right"
               HorizontalOptions="End"
               VerticalOptions="End" />
    </Grid>
</ContentPage>
```

In diesem Beispiel werden die [`Label`](xref:Xamarin.Forms.Label) Objekte in jeder Zeile alle vertikal, aber unterschiedliche horizontale Ausrichtungen verwendet. Sie können sich auch vorstellen, wenn die `Label` Objekte in jeder Spalte gleichmäßig horizontal ausgerichtet sind, aber unterschiedliche vertikale Ausrichtungen verwenden:

[![Screenshot der Zellen Ausrichtung innerhalb eines Rasters](grid-images/alignment.png "Zellen Ausrichtung in einem Raster")](grid-images/alignment-large.png#lightbox "Zellen Ausrichtung in einem Raster")

Der entsprechende C#-Code lautet:

```csharp
public class GridAlignmentPageCS : ContentPage
{
    public GridAlignmentPageCS()
    {
        Grid grid = new Grid
        {
            RowSpacing = 0,
            ColumnSpacing = 0,
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition()
            },
            ColumnDefinitions =
            {
                new ColumnDefinition(),
                new ColumnDefinition(),
                new ColumnDefinition()
            }
        };

        // Row 0
        grid.Children.Add(new BoxView
        {
            Color = Color.AliceBlue
        });
        grid.Children.Add(new Label
        {
            Text = "Upper left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Start
        });

        grid.Children.Add(new BoxView
        {
            Color = Color.LightSkyBlue
        }, 1, 0);
        grid.Children.Add(new Label
        {
            Text = "Upper center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Start
        }, 1, 0);

        grid.Children.Add(new BoxView
        {
            Color = Color.CadetBlue
        }, 2, 0);
        grid.Children.Add(new Label
        {
            Text = "Upper right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.Start
        }, 2, 0);

        // Row 1
        grid.Children.Add(new BoxView
        {
            Color = Color.CornflowerBlue
        }, 0, 1);
        grid.Children.Add(new Label
        {
            Text = "Center left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Center
        }, 0, 1);

        grid.Children.Add(new BoxView
        {
            Color = Color.DodgerBlue
        }, 1, 1);
        grid.Children.Add(new Label
        {
            Text = "Center center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.Center
        }, 1, 1);

        grid.Children.Add(new BoxView
        {
            Color = Color.DarkSlateBlue
        }, 2, 1);
        grid.Children.Add(new Label
        {
            Text = "Center right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.Center
        }, 2, 1);

        // Row 2
        grid.Children.Add(new BoxView
        {
            Color = Color.SteelBlue
        }, 0, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower left",
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.End
        }, 0, 2);

        grid.Children.Add(new BoxView
        {
            Color = Color.LightBlue
        }, 1, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower center",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.End
        }, 1, 2);

        grid.Children.Add(new BoxView
        {
            Color = Color.BlueViolet
        }, 2, 2);
        grid.Children.Add(new Label
        {
            Text = "Lower right",
            HorizontalOptions = LayoutOptions.End,
            VerticalOptions = LayoutOptions.End
        }, 2, 2);

        Title = "Grid alignment demo";
        Content = grid;
    }
}
```

## <a name="nested-grid-objects"></a>Gitter Raster Objekte

Ein- [`Grid`](xref:Xamarin.Forms.Grid) Objekt kann als übergeordnetes Layout verwendet werden, das `Grid` untergeordnete Objekte oder andere untergeordnete Layouts enthält. Beim Schachteln `Grid` `Grid.Row` von Objekten verweisen die `Grid.Column` `Grid.RowSpan` angefügten Eigenschaften,, und `Grid.ColumnSpan` immer auf die Position von Sichten innerhalb ihres übergeordneten Elements `Grid` .

Der folgende XAML-Code zeigt ein Beispiel für die Schachtelung von [`Grid`](xref:Xamarin.Forms.Grid) Objekten:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:converters="clr-namespace:GridDemos.Converters"
             x:Class="GridDemos.Views.ColorSlidersGridPage"
             Title="Nested Grids demo">

    <ContentPage.Resources>
        <converters:DoubleToIntConverter x:Key="doubleToInt" />

        <Style TargetType="Label">
            <Setter Property="HorizontalTextAlignment"
                    Value="Center" />
        </Style>
    </ContentPage.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <BoxView x:Name="boxView"
                 Color="Black" />
        <Grid Grid.Row="1"
              Margin="20">
            <Grid.RowDefinitions>
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
                <RowDefinition />
            </Grid.RowDefinitions>
            <Slider x:Name="redSlider"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="1"
                   Text="{Binding Source={x:Reference redSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0}'}" />
            <Slider x:Name="greenSlider"
                    Grid.Row="2"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="3"
                   Text="{Binding Source={x:Reference greenSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0}'}" />
            <Slider x:Name="blueSlider"
                    Grid.Row="4"
                    ValueChanged="OnSliderValueChanged" />
            <Label Grid.Row="5"
                   Text="{Binding Source={x:Reference blueSlider},
                                  Path=Value,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Blue = {0}'}" />
        </Grid>
    </Grid>
</ContentPage>
```

In diesem Beispiel enthält das Stamm [`Grid`](xref:Xamarin.Forms.Grid) Layout eine [`BoxView`](xref:Xamarin.Forms.BoxView) in der ersten Zeile und ein untergeordnetes Element `Grid` in der zweiten Zeile. Das untergeordnete Element `Grid` enthält [`Slider`](xref:Xamarin.Forms.Slider) -Objekte, die die vom `BoxView` -Objekt angezeigte Farbe und Objekte, die [`Label`](xref:Xamarin.Forms.Label) den Wert der einzelnen anzeigen, bearbeiten `Slider` :

[![Screenshot der Raster Raster](grid-images/nesting.png "Gitter Raster Objekte")](grid-images/nesting-large.png#lightbox "Gitter Raster Objekte")

> [!IMPORTANT]
> Wenn Sie [`Grid`](xref:Xamarin.Forms.Grid) Objekte und andere Layouts verschachteln, desto stärker wirkt sich das geschachtelte Layout auf die Leistung aus. Weitere Informationen finden Sie unter [auswählen des richtigen Layouts](~/xamarin-forms/deploy-test/performance.md#choose-the-correct-layout).

Der entsprechende C#-Code lautet:

```csharp
public class ColorSlidersGridPageCS : ContentPage
{
    BoxView boxView;
    Slider redSlider;
    Slider greenSlider;
    Slider blueSlider;

    public ColorSlidersGridPageCS()
    {
        // Create an implicit style for the Labels
        Style labelStyle = new Style(typeof(Label))
        {
            Setters =
            {
                new Setter { Property = Label.HorizontalTextAlignmentProperty, Value = TextAlignment.Center }
            }
        };
        Resources.Add(labelStyle);

        // Root page layout
        Grid rootGrid = new Grid
        {
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition()
            }
        };

        boxView = new BoxView { Color = Color.Black };
        rootGrid.Children.Add(boxView);

        // Child page layout
        Grid childGrid = new Grid
        {
            Margin = new Thickness(20),
            RowDefinitions =
            {
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition(),
                new RowDefinition()
            }
        };

        DoubleToIntConverter doubleToInt = new DoubleToIntConverter();

        redSlider = new Slider();
        redSlider.ValueChanged += OnSliderValueChanged;
        childGrid.Children.Add(redSlider);

        Label redLabel = new Label();
        redLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Red = {0}", source: redSlider));
        Grid.SetRow(redLabel, 1);
        childGrid.Children.Add(redLabel);

        greenSlider = new Slider();
        greenSlider.ValueChanged += OnSliderValueChanged;
        Grid.SetRow(greenSlider, 2);
        childGrid.Children.Add(greenSlider);

        Label greenLabel = new Label();
        greenLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Green = {0}", source: greenSlider));
        Grid.SetRow(greenLabel, 3);
        childGrid.Children.Add(greenLabel);

        blueSlider = new Slider();
        blueSlider.ValueChanged += OnSliderValueChanged;
        Grid.SetRow(blueSlider, 4);
        childGrid.Children.Add(blueSlider);

        Label blueLabel = new Label();
        blueLabel.SetBinding(Label.TextProperty, new Binding("Value", converter: doubleToInt, converterParameter: "255", stringFormat: "Blue = {0}", source: blueSlider));
        Grid.SetRow(blueLabel, 5);
        childGrid.Children.Add(blueLabel);

        // Place the child Grid in the root Grid
        rootGrid.Children.Add(childGrid, 0, 1);

        Title = "Nested Grids demo";
        Content = rootGrid;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs e)
    {
        boxView.Color = new Color(redSlider.Value, greenSlider.Value, blueSlider.Value);
    }
}
```

## <a name="related-links"></a>Verwandte Links

- [Raster Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-griddemos)
- [Layoutoptionen inXamarin.Forms](layout-options.md)
- [Layout auswählen Xamarin.Forms](choose-layout.md)
- [Optimieren der Xamarin.Forms App-Leistung](~/xamarin-forms/deploy-test/performance.md)
