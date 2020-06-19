---
title: Teil 4. Grundlagen der Datenbindung
description: Mit Daten Bindungen können Eigenschaften von zwei-Objekten verknüpft werden, sodass eine Änderung in einer Änderung eine Änderung in der anderen bewirkt.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 08be571d3ba69891a56c08efd556a999e51431c8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139853"
---
# <a name="part-4-data-binding-basics"></a>Teil 4. Grundlagen der Datenbindung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_Mit Daten Bindungen können Eigenschaften von zwei-Objekten verknüpft werden, sodass eine Änderung in einer Änderung eine Änderung in der anderen bewirkt. Dies ist ein sehr wertvolles Tool, und während Daten Bindungen vollständig im Code definiert werden können, bietet XAML Verknüpfungen und bequeme Möglichkeiten. Folglich ist eine der wichtigsten Markup Erweiterungen in " Xamarin.Forms Binding"._

## <a name="data-bindings"></a>Daten Bindungen

Daten Bindungen verbinden Eigenschaften von zwei-Objekten, die als *Quelle* und *Ziel*bezeichnet werden. Im Code sind zwei Schritte erforderlich: die `BindingContext` -Eigenschaft des Zielobjekts muss auf das-Quell Objekt festgelegt werden, und die- `SetBinding` Methode (häufig in Verbindung mit der- `Binding` Klasse verwendet) muss für das Zielobjekt aufgerufen werden, um eine Eigenschaft dieses Objekts an eine Eigenschaft des Quell Objekts zu binden.

Die Ziel Eigenschaft muss eine bindbare Eigenschaft sein, was bedeutet, dass das Zielobjekt von abgeleitet werden muss `BindableObject` . Die Online Xamarin.Forms Dokumentation gibt an, welche Eigenschaften bindbare Eigenschaften sind. Eine Eigenschaft von, `Label` wie z `Text` . b., ist der bindbaren Eigenschaft zugeordnet `TextProperty` .

In Markup müssen Sie auch die beiden Schritte ausführen, die im Code erforderlich sind, mit dem Unterschied, dass die `Binding` Markup Erweiterung an der Stelle des `SetBinding` Aufrufes und der-Klasse beteiligt ist `Binding` .

Wenn Sie jedoch Daten Bindungen in XAML definieren, gibt es mehrere Möglichkeiten, die `BindingContext` des Zielobjekts festzulegen. Manchmal wird Sie aus der Code Behind-Datei festgelegt, manchmal mithilfe `StaticResource` einer `x:Static` -oder-Markup Erweiterung und manchmal als Inhalt von `BindingContext` Eigenschaften-Element-Tags.

Bindungen werden am häufigsten verwendet, um die visuellen Elemente eines Programms mit einem zugrunde liegenden Datenmodell zu verbinden, in der Regel in der Realisierung der MVVM (Model-View-ViewModel)-Anwendungsarchitektur, wie in [Teil 5 erläutert. Von Daten Bindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), aber andere Szenarien sind möglich.

## <a name="view-to-view-bindings"></a>Ansicht-zu-Ansicht-Bindungen

Sie können Daten Bindungen definieren, um Eigenschaften zweier Sichten auf derselben Seite zu verknüpfen. In diesem Fall legen Sie den `BindingContext` des Zielobjekts mithilfe der `x:Reference` Markup Erweiterung fest.

Hier sehen Sie eine XAML-Datei, die eine `Slider` und zwei `Label` Sichten enthält, von denen eine durch den-Wert gedreht wird, und eine andere, die `Slider` den `Slider` Wert anzeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderBindingsPage"
             Title="Slider Bindings Page">

    <StackLayout>
        <Label Text="ROTATION"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
               FontAttributes="Bold"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Der `Slider` enthält ein `x:Name` Attribut, auf das von den beiden `Label` Ansichten mithilfe der `x:Reference` Markup Erweiterung verwiesen wird.

Die `x:Reference` Bindungs Erweiterung definiert eine Eigenschaft mit dem Namen, die `Name` auf den Namen des Elements festgelegt wird, auf das verwiesen wird, in diesem Fall `slider` . Allerdings definiert die-Klasse, die `ReferenceExtension` die `x:Reference` Markup Erweiterung definiert, auch ein- `ContentProperty` Attribut für `Name` , was bedeutet, dass Sie nicht explizit erforderlich ist. Nur für die Vielfalt umfasst das erste `x:Reference` "Name =", das zweite jedoch nicht:

```xaml
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

Die `Binding` Markup Erweiterung selbst kann mehrere Eigenschaften aufweisen, ebenso wie die `BindingBase` -Klasse und die- `Binding` Klasse. Der `ContentProperty` für `Binding` ist `Path` , aber der "Path ="-Teil der Markup Erweiterung kann weggelassen werden, wenn der Pfad das erste Element in der `Binding` Markup Erweiterung ist. Das erste Beispiel weist "Path =" auf, aber das zweite Beispiel lässt es nicht zu:

```xaml
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Die Eigenschaften können sich alle in einer Zeile befinden oder in mehrere Zeilen aufgeteilt werden:

```xaml
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

Alles, was praktisch ist.

Beachten Sie die- `StringFormat` Eigenschaft in der zweiten `Binding` Markup Erweiterung. In Xamarin.Forms führen Bindungen keine impliziten Typkonvertierungen aus, und wenn Sie ein nicht-Zeichen folgen Objekt als Zeichenfolge anzeigen müssen, müssen Sie einen Typkonverter angeben oder verwenden `StringFormat` . Im Hintergrund wird die statische- `String.Format` Methode verwendet, um zu implementieren `StringFormat` . Das ist möglicherweise ein Problem, da die .net-Formatierungs Spezifikationen geschweifte Klammern einschließen, die auch zum Begrenzen von Markup Erweiterungen verwendet werden. Dies birgt das Risiko, dass der XAML-Parser verwirrend ist. Um dies zu vermeiden, fügen Sie die gesamte Formatierungs Zeichenfolge in einfache Anführungszeichen ein:

```xaml
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Das Programm wird ausgeführt:

[![Ansicht-zu-Ansicht-Bindungen](data-binding-basics-images/sliderbinding.png)](data-binding-basics-images/sliderbinding-large.png#lightbox)

## <a name="the-binding-mode"></a>Der Bindungs Modus

Eine einzelne Ansicht kann über Daten Bindungen in mehreren Eigenschaften verfügen. Jede Sicht kann jedoch nur eine aufweisen `BindingContext` , sodass mehrere Daten Bindungen in dieser Sicht alle auf Eigenschaften desselben Objekts verweisen müssen.

Die Lösung für dieses und andere Probleme bezieht sich `Mode` auf die-Eigenschaft, die auf einen Member der-Enumeration festgelegt ist `BindingMode` :

- `Default`
- `OneWay`–-Werte werden von der Quelle in das Ziel übertragen.
- `OneWayToSource`–-Werte werden vom Ziel an die Quelle übertragen.
- `TwoWay`–-Werte werden auf beide Arten zwischen Quelle und Ziel übertragen.
- `OneTime`– Daten werden von der Quelle zum Ziel weitergeleitet, aber nur, wenn die `BindingContext` Änderungen

Das folgende Programm veranschaulicht eine häufige Verwendung der `OneWayToSource` `TwoWay` Bindungs Modi und. Vier `Slider` Ansichten dienen zum Steuern der `Scale` Eigenschaften, `Rotate` , `RotateX` und `RotateY` von `Label` . Zunächst scheint es so, als wären diese vier Eigenschaften von `Label` Daten Bindungs Ziele, da jede von einem festgelegt wird `Slider` . Der `BindingContext` von kann jedoch `Label` nur ein Objekt sein, und es gibt vier verschiedene Schieberegler.

Aus diesem Grund werden alle Bindungen in scheinbar rückwärts festgelegt: die jedes der `BindingContext` vier Schieberegler ist auf festgelegt `Label` , und die Bindungen werden auf die `Value` Eigenschaften der Schieberegler festgelegt. Mit den `OneWayToSource` Modi und `TwoWay` `Value` können diese Eigenschaften die Quell Eigenschaften festlegen, die die `Scale` `Rotate` Eigenschaften,, `RotateX` und von sind `RotateY` `Label` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderTransformsPage"
             Padding="5"
             Title="Slider Transforms Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <!-- Scaled and rotated Label -->
        <Label x:Name="label"
               Text="TEXT"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <!-- Slider and identifying Label for Scale -->
        <Slider x:Name="scaleSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="1" Grid.Column="0"
                Maximum="10"
                Value="{Binding Scale, Mode=TwoWay}" />

        <Label BindingContext="{x:Reference scaleSlider}"
               Text="{Binding Value, StringFormat='Scale = {0:F1}'}"
               Grid.Row="1" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for Rotation -->
        <Slider x:Name="rotationSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="2" Grid.Column="0"
                Maximum="360"
                Value="{Binding Rotation, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationSlider}"
               Text="{Binding Value, StringFormat='Rotation = {0:F0}'}"
               Grid.Row="2" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationX -->
        <Slider x:Name="rotationXSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="3" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationX, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationXSlider}"
               Text="{Binding Value, StringFormat='RotationX = {0:F0}'}"
               Grid.Row="3" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationY -->
        <Slider x:Name="rotationYSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="4" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationY, Mode=OneWayToSource}" />

        <Label BindingContext="{x:Reference rotationYSlider}"
               Text="{Binding Value, StringFormat='RotationY = {0:F0}'}"
               Grid.Row="4" Grid.Column="1"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

Die Bindungen in drei der `Slider` Sichten sind. `OneWayToSource` Dies bedeutet, dass der- `Slider` Wert eine Änderung der-Eigenschaft des-Objekts bewirkt, das `BindingContext` der `Label` benannte ist `label` . Diese drei `Slider` Sichten bewirken Änderungen an den `Rotate` `RotateX` Eigenschaften, und `RotateY` von `Label` .

Die Bindung für die- `Scale` Eigenschaft ist jedoch `TwoWay` . Dies liegt daran, dass die `Scale` -Eigenschaft den Standardwert 1 hat und die Verwendung einer- `TwoWay` Bindung bewirkt, dass der `Slider` Anfangswert auf 1 und nicht auf 0 festgelegt wird. Wenn diese Bindung war `OneWayToSource` , wird die- `Scale` Eigenschaft anfänglich vom Standardwert auf 0 festgelegt `Slider` . Die `Label` ist nicht sichtbar, und dies kann zu Verwirrung des Benutzers führen.

 [![Rückwärts Bindungen](data-binding-basics-images/slidertransforms.png)](data-binding-basics-images/slidertransforms-large.png#lightbox)

 > [!NOTE]
 > Die [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Klasse verfügt auch über die [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) -Eigenschaft und die-Eigenschaft [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) , die `VisualElement` auf der x-Achse bzw. der y-Achse skaliert.

## <a name="bindings-and-collections"></a>Bindungen und Auflistungen

Nichts zeigt die Leistungsfähigkeit von XAML und Daten Bindungen besser als eine Vorlage `ListView` .

`ListView`definiert eine `ItemsSource` Eigenschaft vom Typ `IEnumerable` und zeigt die Elemente in dieser Auflistung an. Diese Elemente können Objekte eines beliebigen Typs sein. Standardmäßig `ListView` verwendet die- `ToString` Methode der einzelnen Elemente, um dieses Element anzuzeigen. Manchmal ist dies genau das, was Sie möchten, aber in vielen Fällen wird `ToString` nur der voll qualifizierte Klassenname des Objekts zurückgegeben.

Allerdings können die Elemente in der Auflistung `ListView` beliebig angezeigt werden, indem Sie eine *Vorlage*verwenden, die eine von abgeleitete Klasse umfasst `Cell` . Die Vorlage wird für jedes Element in der geklont `ListView` , und Daten Bindungen, die für die Vorlage festgelegt wurden, werden an die einzelnen Klone übertragen.

Häufig empfiehlt es sich, eine benutzerdefinierte Zelle für diese Elemente mithilfe der-Klasse zu erstellen `ViewCell` . Dieser Prozess ist im Code etwas unpraktisch, aber in XAML ist es sehr unkompliziert.

Im xamlsamples-Projekt ist eine Klasse mit dem Namen enthalten `NamedColor` . Jedes `NamedColor` -Objekt verfügt über die `Name` -und- `FriendlyName` Eigenschaften des `string` -Typs und eine- `Color` Eigenschaft des Typs `Color` . Außerdem verfügt über `NamedColor` 141 statische, schreibgeschützte Felder vom Typ, `Color` die den in der-Klasse definierten Farben entsprechen Xamarin.Forms `Color` . Ein statischer Konstruktor erstellt eine Auflistung `IEnumerable<NamedColor>` , die `NamedColor` -Objekte enthält, die diesen statischen Feldern entsprechen, und weist diese der öffentlichen statischen- `All` Eigenschaft zu.

Das Festlegen der statischen- `NamedColor.All` Eigenschaft auf die `ItemsSource` eines `ListView` ist einfach mit der `x:Static` Markup Erweiterung:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

Die resultierende Anzeige legt fest, dass die Elemente wirklich vom Typ sind `XamlSamples.NamedColor` :

[![Binden an eine Sammlung](data-binding-basics-images/listview1.png)](data-binding-basics-images/listview1-large.png#lightbox)

Dabei handelt es sich nicht um viele Informationen, aber die `ListView` ist Scrollfähig und wählbar.

Wenn Sie eine Vorlage für die Elemente definieren möchten, sollten Sie die `ItemTemplate` Eigenschaft als Eigenschafts Element aufteilen und auf einen festlegen `DataTemplate` , der auf eine verweist `ViewCell` . Mit der- `View` Eigenschaft von `ViewCell` können Sie ein Layout für eine oder mehrere Ansichten definieren, um die einzelnen Elemente anzuzeigen. Im folgenden finden Sie ein einfaches Beispiel:

```xaml
<ListView ItemsSource="{x:Static local:NamedColor.All}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <ViewCell.View>
                    <Label Text="{Binding FriendlyName}" />
                </ViewCell.View>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

> [!NOTE]
> Die Bindungs Quelle für Zellen und untergeordnete Elemente von Zellen ist die Auflistung `ListView.ItemsSource` .

Das- `Label` Element wird auf die- `View` Eigenschaft von festgelegt `ViewCell` . (Die- `ViewCell.View` Tags werden nicht benötigt, da die- `View` Eigenschaft die Content-Eigenschaft von ist `ViewCell` .) Dieses Markup zeigt die- `FriendlyName` Eigenschaft der einzelnen `NamedColor` Objekte an:

[![Binden an eine Sammlung mit einem DataTemplate](data-binding-basics-images/listview2.png)](data-binding-basics-images/listview2-large.png#lightbox)

Viel besser. Nun müssen Sie nur noch die Element Vorlage mit weiteren Informationen und der tatsächlichen Farbe anfichten. Um diese Vorlage zu unterstützen, wurden einige Werte und Objekte im Ressourcen Wörterbuch der Seite definiert:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <OnPlatform x:Key="boxSize"
                        x:TypeArguments="x:Double">
                <On Platform="iOS, Android, UWP" Value="50" />
            </OnPlatform>

            <OnPlatform x:Key="rowHeight"
                        x:TypeArguments="x:Int32">
                <On Platform="iOS, Android, UWP" Value="60" />
            </OnPlatform>

            <local:DoubleToIntConverter x:Key="intConverter" />

        </ResourceDictionary>
    </ContentPage.Resources>

    <ListView ItemsSource="{x:Static local:NamedColor.All}"
              RowHeight="{StaticResource rowHeight}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <StackLayout Padding="5, 5, 0, 5"
                                 Orientation="Horizontal"
                                 Spacing="15">

                        <BoxView WidthRequest="{StaticResource boxSize}"
                                 HeightRequest="{StaticResource boxSize}"
                                 Color="{Binding Color}" />

                        <StackLayout Padding="5, 0, 0, 0"
                                     VerticalOptions="Center">

                            <Label Text="{Binding FriendlyName}"
                                   FontAttributes="Bold"
                                   FontSize="Medium" />

                            <StackLayout Orientation="Horizontal"
                                         Spacing="0">
                                <Label Text="{Binding Color.R,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat='R={0:X2}'}" />

                                <Label Text="{Binding Color.G,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', G={0:X2}'}" />

                                <Label Text="{Binding Color.B,
                                       Converter={StaticResource intConverter},
                                       ConverterParameter=255,
                                       StringFormat=', B={0:X2}'}" />
                            </StackLayout>
                        </StackLayout>
                    </StackLayout>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Beachten Sie, dass verwendet wird `OnPlatform` , um die Größe eines `BoxView` und die Höhe der `ListView` Zeilen zu definieren. Obwohl die Werte für alle Plattformen identisch sind, kann das Markup problemlos für andere Werte angepasst werden, um die Anzeige zu optimieren.

## <a name="binding-value-converters"></a>Binden von Wertkonvertern

In der vorherigen **ListView** -demoxaml-Datei `R` werden die einzelnen `G` Eigenschaften, und `B` der-Struktur angezeigt Xamarin.Forms `Color` . Diese Eigenschaften sind vom Typ `double` und reichen von 0 bis 1. Wenn Sie die hexadezimalen Werte anzeigen möchten, können Sie nicht einfach `StringFormat` mit der Formatierungs Spezifikation "x2" verwenden. Dies funktioniert nur für ganze Zahlen und außerdem müssen die `double` Werte mit 255 multipliziert werden.

Dieses kleine Problem wurde mit einem *Wert Konverter*gelöst, auch als *Bindungs Konverter*bezeichnet. Dies ist eine Klasse, die die `IValueConverter` -Schnittstelle implementiert, d. h., Sie verfügt über zwei Methoden namens `Convert` und `ConvertBack` . Die- `Convert` Methode wird aufgerufen, wenn ein Wert von der Quelle zum Ziel übertragen wird. die- `ConvertBack` Methode wird für Übertragungen von Target an Source in- `OneWayToSource` oder-Bindungen aufgerufen `TwoWay` :

```csharp
using System;
using System.Globalization;
using Xamarin.Forms;

namespace XamlSamples
{
    class DoubleToIntConverter : IValueConverter
    {
        public object Convert(object value, Type targetType,
                              object parameter, CultureInfo culture)
        {
            double multiplier;

            if (!Double.TryParse(parameter as string, out multiplier))
                multiplier = 1;

            return (int)Math.Round(multiplier * (double)value);
        }

        public object ConvertBack(object value, Type targetType,
                                  object parameter, CultureInfo culture)
        {
            double divider;

            if (!Double.TryParse(parameter as string, out divider))
                divider = 1;

            return ((double)(int)value) / divider;
        }
    }
}
```

Die- `ConvertBack` Methode spielt in diesem Programm keine Rolle, da die Bindungen nur eine Möglichkeit von der Quelle zum Ziel sind.

Eine Bindung verweist auf einen Bindungs Konverter mit der- `Converter` Eigenschaft. Ein Bindungs Konverter kann auch einen Parameter akzeptieren, der mit der-Eigenschaft angegeben wird `ConverterParameter` . Für einige Vielseitigkeit ist dies die Art und Weise, wie der Multiplikator angegeben wird. Der Bindungs Konverter überprüft den Konverter-Parameter auf einen gültigen `double` Wert.

Der Konverter wird im Ressourcen Wörterbuch instanziiert, sodass er von mehreren Bindungen gemeinsam genutzt werden kann:

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

Drei Daten Bindungen verweisen auf diese einzelne Instanz. Beachten Sie, dass die `Binding` Markup Erweiterung eine eingebettete `StaticResource` Markup Erweiterung enthält:

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

Dies ist das Ergebnis:

[![Binden an eine Sammlung mit DataTemplate und Konvertern](data-binding-basics-images/listview3.png)](data-binding-basics-images/listview3-large.png#lightbox)

Der `ListView` ist sehr komplex bei der Verarbeitung von Änderungen, die dynamisch in den zugrunde liegenden Daten auftreten können, jedoch nur, wenn Sie bestimmte Schritte ausführen. Wenn die Auflistung der Elemente, die der- `ItemsSource` Eigenschaft der `ListView` Änderungen während der Laufzeit zugewiesen werden – d. h., wenn Elemente der Auflistung hinzugefügt oder daraus entfernt werden können –, verwenden Sie eine- `ObservableCollection` Klasse für diese Elemente. `ObservableCollection`implementiert die `INotifyCollectionChanged` -Schnittstelle und `ListView` installiert einen Handler für das- `CollectionChanged` Ereignis.

Wenn sich die Eigenschaften der Elemente selbst während der Laufzeit ändern, sollten die Elemente in der Auflistung die `INotifyPropertyChanged` -Schnittstelle implementieren und Änderungen an Eigenschafts Werten mit dem- `PropertyChanged` Ereignis signalisieren. Dies wird im nächsten Teil dieser Reihe, [Teil 5, veranschaulicht. Von der Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Zusammenfassung

Daten Bindungen bieten einen leistungsfähigen Mechanismus zum Verknüpfen von Eigenschaften zwischen zwei Objekten innerhalb einer Seite oder zwischen visuellen Objekten und zugrunde liegenden Daten. Wenn die Anwendung jedoch mit Datenquellen arbeitet, wird ein gängiges Anwendungsarchitektur Muster als nützliches Paradigma hervorgehen. Dies wird in [Teil 5 behandelt. Von Daten Bindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="related-links"></a>Verwandte Links

- [Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Teil 1: Einführung in XAML (Beispiel)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 2. Wichtige XAML-Syntax (Beispiel)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 3: XAML-Markup Erweiterungen (Beispiel)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 5. Datenbindung an MVVM (Beispiel)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
