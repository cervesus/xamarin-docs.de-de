---
title: Teil 4. Grundlagen der Datenbindung
description: Datenbindungen können Sie Eigenschaften von zwei Objekten, die verknüpft werden, damit eine Änderung in einer bewirkt, dass eine Änderung in der anderen.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 4a24c8dbb9ab4e23afa03de4ae2dbc55ddfb5fa4
ms.sourcegitcommit: e000cc0765857c1d7f49538df9e62e9d3aa60775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2019
ms.locfileid: "56334565"
---
# <a name="part-4-data-binding-basics"></a>Teil 4. Grundlagen der Datenbindung

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)

_Datenbindungen können Sie Eigenschaften von zwei Objekten, die verknüpft werden, damit eine Änderung in einer bewirkt, dass eine Änderung in der anderen. Dies ist ein sehr wertvolles Tool, und während der datenbindungen, die vollständig in Code definiert werden können, Verknüpfungen und der Einfachheit halber XAML enthält. Daher ist eine der wichtigsten Markuperweiterungen in Xamarin.Forms binden._

## <a name="data-bindings"></a>Datenbindungen

Datenbindungen, die connect-Eigenschaften von zwei Objekte, mit dem Namen der *Quelle* und *Ziel*. Im Code sind zwei Schritte erforderlich: Die `BindingContext` Eigenschaft des Zielobjekts muss festgelegt werden, mit dem Quellobjekt und die `SetBinding` Methode (wird häufig in Verbindung mit der `Binding` Klasse) muss für das Zielobjekt eine Eigenschaft dieses Objekts an eine Eigenschaft der Quelle binden aufgerufen werden -Objekt.

Die Zieleigenschaft muss eine bindbare Eigenschaft an, was bedeutet, dass das Zielobjekt von abgeleitet werden, muss `BindableObject`. Die Xamarin.Forms-Onlinedokumentation gibt an, welche Eigenschaften bindbare Eigenschaften sind. Eine Eigenschaft des `Label` wie z. B. `Text` bezieht sich auf die bindbare Eigenschaft `TextProperty`.

Im Markup müssen Sie auch die gleichen zwei Schritte, die erforderlich sind, im Code, Ausführen mit dem Unterschied, dass die `Binding` Markuperweiterung nimmt den Platz von der `SetBinding` aufrufen und die `Binding` Klasse.

Wenn Sie datenbindungen in XAML definieren, es gibt jedoch mehrere Möglichkeiten zum Festlegen der `BindingContext` des Zielobjekts. Manchmal festgelegt aus der CodeBehind-Datei, manchmal mit eine `StaticResource` oder `x:Static` Markuperweiterung, und gelegentlich auch als Inhalt der `BindingContext` Eigenschaftenelement Tags.

Bindungen werden am häufigsten mit einem zugrunde liegenden Datenmodell, in der Regel in eine Realisierung der Architektur des MVVM (Model-View-ViewModel)-Anwendung, die Verbindung die visuellen Elemente eines Programms verwendet, wie unter [Teil 5. Von Datenbindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), aber andere Szenarien sind möglich.

## <a name="view-to-view-bindings"></a>Ansicht-zu-Ansicht-Bindungen

Sie können die datenbindungen, die zum Verknüpfen der Eigenschaften von zwei Ansichten auf derselben Seite definieren. In diesem Fall legen Sie die `BindingContext` der das Ziel mit dem `x:Reference` Markuperweiterung.

Hier ist eine XAML-Datei mit einer `Slider` und zwei `Label` Ansichten, von denen gedreht wird, durch die `Slider` Wert und einen anderen angezeigt der `Slider` Wert:

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

Die `Slider` enthält ein `x:Name` -Attribut, das die beiden verweist `Label` Ansichten mit der `x:Reference` Markuperweiterung.

Die `x:Reference` bindungserweiterung definiert eine Eigenschaft namens `Name` , auf den Namen des Elements auf die verwiesen wird, legen Sie in diesem Fall `slider`. Allerdings die `ReferenceExtension` Klasse, die definiert die `x:Reference` Markuperweiterung definiert auch eine `ContentProperty` Attribut für `Name`, was bedeutet, dass sie nicht explizit erforderlich ist. Nur für verschiedene die erste `x:Reference` enthält "Name =" die zweite jedoch nicht:

```xaml
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

Die `Binding` Markuperweiterung selbst kann verfügen über verschiedene Eigenschaften, wie die `BindingBase` und `Binding` Klasse. Die `ContentProperty` für `Binding` ist `Path`, aber die "Path =" Teil der Markuperweiterung ausgelassen werden, wenn der Pfad in das erste Element ist die `Binding` Markuperweiterung. Im erste Beispiel ist "Path =" im zweite Beispiel lässt es aber:

```xaml
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Die Eigenschaften können alle in einer Zeile befinden oder in mehrere Zeilen aufgeteilt:

```xaml
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

Führen Sie an, was praktisch ist.

Beachten Sie, dass die `StringFormat` Eigenschaft in der zweiten `Binding` Markuperweiterung. In Xamarin.Forms Bindungen führen keine implizite typkonvertierungen, und ein nichtzeichenfolgen-Objekt als Zeichenfolge angezeigt werden sollen. Sie müssen einen Typkonverter oder `StringFormat`. Hinter den Kulissen wird die statische `String.Format` Methode dient zum Implementieren `StringFormat`. Das ist möglicherweise ein Problem, da .NET Formatierungsangaben geschweifte Klammern, beinhalten die auch zum Trennen von Markuperweiterungen verwendet werden. Dadurch wird die Gefahr der verwirrend erscheinen des XAML-Parsers erstellt. Um dies zu vermeiden, fügen Sie die gesamte Formatierung Zeichenfolge in einfache Anführungszeichen einschließen:

```xaml
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Hier ist das aktive Programm:

[![](data-binding-basics-images/sliderbinding.png "Ansicht-zu-Ansicht Bindungen")](data-binding-basics-images/sliderbinding-large.png#lightbox "Ansicht-zu-Ansicht-Bindungen ")

## <a name="the-binding-mode"></a>Den Bindungsmodus

Eine einzelne Ansicht kann auf verschiedene Eigenschaften über datenbindungen verfügen. Jede Sicht kann jedoch haben nur eine `BindingContext`, sodass mehrere datenbindungen, die für diese Sicht alle müssen das gleiche Objekt verweisen.

Die Lösung für dieses und andere Probleme umfasst die `Mode` -Eigenschaft, die auf einen Member festgelegt wird die `BindingMode` Enumeration:

- `Default`
- `OneWay` – Werte werden aus der Quelle zum Ziel übertragen
- `OneWayToSource` – Werte vom Ziel zur Quelle übertragen werden
- `TwoWay` – Werte werden in beide Richtungen zwischen Quelle und Ziel übertragen
- `OneTime` – Daten werden von der Quelle zum Ziel, aber nur, wenn die `BindingContext` Änderungen

Das folgende Programm zeigt häufig dazu verwendet die `OneWayToSource` und `TwoWay` Bindungsmodi. Vier `Slider` Sichten dienen der Kontrolle der `Scale`, `Rotate`, `RotateX`, und `RotateY` Eigenschaften eine `Label`. Zunächst erscheint es als ob diese vier Eigenschaften der `Label` Datenbindung Ziele sollte sein werden, da jeder von festgelegt wird eine `Slider`. Allerdings die `BindingContext` von `Label` kann nur ein Objekt sein, und es gibt vier verschiedene Schieberegler.

Aus diesem Grund Alle Bindungen in festgelegt sind scheinbar rückwärts Möglichkeiten: Die `BindingContext` jedes der vier Schieberegler auf festgelegt ist die `Label`, und die Bindungen festgelegt sind, auf die `Value` Eigenschaften der Schieberegler. Mithilfe der `OneWayToSource` und `TwoWay` Modi diese `Value` Eigenschaften können die Datenquelleneigenschaften, die festgelegt die `Scale`, `Rotate`, `RotateX`, und `RotateY` Eigenschaften der `Label`:

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

Die Bindungen für drei der `Slider` Ansichten sind `OneWayToSource`. Dies bedeutet, der `Slider` Wert führt dazu, dass es sich bei eine Änderung in der Eigenschaft des seine `BindingContext`, d.h. die `Label` mit dem Namen `label`. Diese drei `Slider` Ansichten dazu führen, dass Änderungen an der `Rotate`, `RotateX`, und `RotateY` Eigenschaften der `Label`.

Allerdings die Bindung für die `Scale` Eigenschaft `TwoWay`. Grund hierfür ist die `Scale` Eigenschaft hat den Standardwert 1 und mit einer `TwoWay` Bindung bewirkt, dass die `Slider` Initialwert auf 1 statt 0 festgelegt werden. Wäre, dass die Bindung `OneWayToSource`, `Scale` Eigenschaft anfangs auf 0 aus fest der `Slider` default-Wert. Die `Label` würde nicht sichtbar, und für dem Benutzer eine gewissen Verwirrung verursachen können.

 [![](data-binding-basics-images/slidertransforms.png "Abwärtskompatibilität Bindungen")](data-binding-basics-images/slidertransforms-large.png#lightbox "rückwärts Bindungen")

 > [!NOTE]
 > Die [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) -Klasse verfügt auch über [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) und [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) Eigenschaften, die Skalierung der `VisualElement` auf der x-Achse und y-Achse bzw.

## <a name="bindings-and-collections"></a>Bindungen und Sammlungen

Nichts illustriert die Leistung von XAML und datenbindungen besser als eine auf Vorlagen basierenden `ListView`.

`ListView` definiert eine `ItemsSource` Eigenschaft vom Typ `IEnumerable`, und zeigt die Elemente in der Auflistung. Diese Elemente können Objekte eines beliebigen Typs sein. In der Standardeinstellung `ListView` verwendet die `ToString` -Methode jedes Elements dieses Element angezeigt wird. Manchmal ist dies nur erwünscht, aber in vielen Fällen `ToString` gibt nur den vollständig qualifizierten Klassennamen des Objekts.

Allerdings die Elemente in der `ListView` Auflistung kann angezeigt werden, wie Sie, durch die Verwendung von möchten einer *Vorlage*, hierbei ist eine abgeleitete Klasse `Cell`. Die Vorlage geklont für jedes Element in der `ListView`, und der datenbindungen, die in der Vorlage festgelegt wurden, die in die einzelnen Klone übertragen werden.

Sehr häufig, sollten Sie zum Erstellen einer benutzerdefinierten Zelle für diese Elemente mithilfe der `ViewCell` Klasse. Dieser Prozess wird im Code etwas unübersichtlich, aber in XAML wird es sehr einfach.

Die XamlSamples Projekt enthält eine Klasse namens `NamedColor`. Jede `NamedColor` Objekt verfügt über `Name` und `FriendlyName` Eigenschaften des Typs `string`, und ein `Color` Eigenschaft vom Typ `Color`. Darüber hinaus `NamedColor` hat 141 statische schreibgeschützte Felder vom Typ `Color` , die in der Xamarin.Forms definierten Farben entsprechen `Color` Klasse. Ein statischer Konstruktor erstellt ein `IEnumerable<NamedColor>` Sammlung mit `NamedColor` Objekte, die diese statischen Feldern entsprechen, und weist sie die öffentliche statische `All` Eigenschaft.

Festlegen der statischen `NamedColor.All` Eigenschaft, um die `ItemsSource` von einer `ListView` ist einfach mit der `x:Static` Markuperweiterung:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

Die resultierenden Anzeige gibt an, dass die Elemente wirklich vom Typ sind `XamlSamples.NamedColor`:

[![](data-binding-basics-images/listview1.png "Binden an eine Auflistung")](data-binding-basics-images/listview1-large.png#lightbox "Binden an eine Auflistung")

Es ist nicht viele Informationen, aber die `ListView` ist bildlauffähigen und ausgewählt werden.

Um eine Vorlage für die Elemente zu definieren, sollten Sie sich die `ItemTemplate` -Eigenschaft, wie ein Property-Element, und legen Sie dafür eine `DataTemplate`, das Klicken Sie dann auf eine `ViewCell`. Um die `View` Eigenschaft der `ViewCell` können Sie ein Layout von ein oder mehrere Ansichten zum Anzeigen der einzelnen Elemente zu definieren. Hier ist ein einfaches Beispiel:

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

Die `Label` -Elementgruppe ist die `View` Eigenschaft der `ViewCell`. (Die `ViewCell.View` Tags sind nicht erforderlich, da die `View` -Eigenschaft ist die Content-Eigenschaft des `ViewCell`.) Dieses Markup zeigt die `FriendlyName` Eigenschaft der einzelnen `NamedColor` Objekt:

[![](data-binding-basics-images/listview2.png "Binden an eine Auflistung mit einem DataTemplate")](data-binding-basics-images/listview2-large.png#lightbox "Binden an eine Auflistung mit einem DataTemplate")

Viel besser. Jetzt ist alles, was erforderlich ist, um die Elementvorlage mit Informationen und die tatsächliche Farbe Fichte. Um diese Vorlage zu unterstützen, haben einige Werte und Objekte im Ressourcenverzeichnis der Seite definiert wurde:

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

Beachten Sie, dass die Verwendung von `OnPlatform` definieren Sie die Größe des eine `BoxView` und die Höhe der der `ListView` Zeilen. Obwohl die Werte für alle Plattformen gleich sind, kann auf das Markup problemlos für andere Werte zur Optimierung der Anzeige angepasst werden können.

## <a name="binding-value-converters"></a>Binden von Wertkonvertern

Die vorherige **ListView Demo** XAML-Datei zeigt die einzelnen `R`, `G`, und `B` Eigenschaften der Xamarin.Forms `Color` Struktur. Diese Eigenschaften sind vom Typ `double` und liegen zwischen 0 und 1. Wenn Sie die hexadezimale Werte anzeigen möchten, einfach können keine `StringFormat` mit einer Formatierung der Spezifikation "X2". Dies funktioniert nur für ganze Zahlen und außerdem die `double` Werte müssen durch 255 multipliziert werden sollen.

Dieses kleine Problem gelöst wurde, mit einem *Wertkonverter*auch Namens eine *Binding-Konverter*. Dies ist eine Klasse, die implementiert die `IValueConverter` Schnittstelle, d. h., sie verfügt über zwei Methoden, die mit dem Namen `Convert` und `ConvertBack`. Die `Convert` Methode wird aufgerufen, wenn ein Wert aus der Quelle zum Ziel übertragen wird die `ConvertBack` Methode wird vom Ziel für Übertragungen aufgerufen, an der Quelle in `OneWayToSource` oder `TwoWay` Bindungen:

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

Die `ConvertBack` Methode wird eine Rolle nicht in diesem Programm wiedergegeben werden, da die Bindungen nur eine Möglichkeit, aus der Quelle zum Ziel sind.

Eine Bindung verweist auf ein Binding-Konverter, mit der `Converter` Eigenschaft. Ein Binding-Konverter kann auch mit Angabe von Parametern akzeptieren die `ConverterParameter` Eigenschaft. Für einige Vielseitigkeit geht ist dies an, wie der Multiplikator, der angegeben wird. Die Binding-Konverter überprüft, ob den Konverter-Parameter für einen gültigen `double` Wert.

Der Konverter wird im Ressourcenverzeichnis instanziiert, damit sie zwischen mehreren Bindungen gemeinsam genutzt werden kann:

```xaml
<local:DoubleToIntConverter x:Key="intConverter" />
```

Drei datenbindungen verweisen auf diese einzelne Instanz. Beachten Sie, dass die `Binding` Markuperweiterung enthält ein eingebettetes `StaticResource` Markuperweiterung:

```xaml
<Label Text="{Binding Color.R,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

Hier ist das Ergebnis:

[![](data-binding-basics-images/listview3.png "Binden an eine Auflistung mit einem DataTemplate und Konverter")](data-binding-basics-images/listview3-large.png#lightbox "Binden an eine Auflistung mit einem DataTemplate und Konverter")

Die `ListView` ist ziemlich ausgereift ist, bei der Verarbeitung von Änderungen, die dynamisch in der zugrunde liegenden Daten, aber nur auftreten, wenn Sie bestimmte Schritte ausführen. Wenn die Auflistung von Elementen zugewiesen der `ItemsSource` Eigenschaft der `ListView` Änderungen während der Laufzeit –, ist, wenn Elemente hinzugefügt werden können, oder aus der Auflistung entfernt – verwenden Sie eine `ObservableCollection` -Klasse für diese Elemente. `ObservableCollection` implementiert die `INotifyCollectionChanged` -Schnittstelle und `ListView` installiert einen Handler für die `CollectionChanged` Ereignis.

Wenn Eigenschaften für die Elemente selbst während der Laufzeit ändern, sollten die Elemente in der Sammlung implementieren, die `INotifyPropertyChanged` -Schnittstelle und Signal Änderungen an Eigenschaftswerten, die mit der `PropertyChanged` Ereignis. Dies wird im nächsten Teil dieser Reihe dargestellt [Teil 5. Aus einer Datenbindung zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Zusammenfassung

Datenbindungen bieten einen leistungsfähigen Mechanismus zum Verknüpfen von Eigenschaften, die zwischen zwei Objekten innerhalb einer Seite oder zwischen visuellen Objekten und dem zugrunde liegenden Daten. Doch bei der Arbeit mit Datenquellen gestartet, beginnt ein Architekturmuster für gängige Anwendung als eine nützliche Paradigma eingebunden. Dies wird im behandelt [Teil 5. Von Datenbindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).



## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Teil 1. Erste Schritte mit XAML (Beispiel)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 2. Essential XAML Syntax (Beispiel)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 3. XAML-Markuperweiterungen (Beispiel)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 5. Aus einer Datenbindung zu MVVM (Beispiel)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
