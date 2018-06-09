---
title: Teil 4. Data Binding-Grundlagen
description: Datenbindungen können Eigenschaften von zwei Objekten, die verknüpft werden, damit eine Änderung in einem bewirkt, dass eine Änderung in den anderen bestehen.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 342288C3-BB4C-4924-B178-72E112D777BA
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 117ddd033faedda871c33ba10c246739309e2e86
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245949"
---
# <a name="part-4-data-binding-basics"></a>Teil 4. Data Binding-Grundlagen

_Datenbindungen können Eigenschaften von zwei Objekten, die verknüpft werden, damit eine Änderung in einem bewirkt, dass eine Änderung in den anderen bestehen. Dies ist ein sehr wichtiges Tool, und während der datenbindungen, die vollständig im Code definiert werden können, stellt XAML Verknüpfungen und Vereinfachung. Daher ist eine der wichtigsten Markuperweiterungen in Xamarin.Forms binden._

## <a name="data-bindings"></a>Datenbindungen

Datenbindungen connect-Eigenschaften von zwei Objekte mit dem Namen der *Quelle* und *Ziel*. Im Code sind zwei Schritte erforderlich: die `BindingContext` Eigenschaft des Zielobjekts muss festgelegt werden, mit dem Quellobjekt und dem `SetBinding` Methode (wird häufig in Verbindung mit der `Binding` Klasse) muss aufgerufen werden, für das Zielobjekt eine Eigenschaft, die gebunden Objekt, das eine Eigenschaft des Quellobjekts.

Die Zieleigenschaft muss eine bindbare Eigenschaft an, was bedeutet, dass das Zielobjekt abgeleitet sein muss `BindableObject`. Die Onlinedokumentation Xamarin.Forms gibt an, welche Eigenschaften bindbare Eigenschaften sind. Eine Eigenschaft des `Label` wie z. B. `Text` bezieht sich auf die bindbare Eigenschaft `TextProperty`.

Im Markup, müssen Sie auch die gleichen beiden Schritte, die erforderlich sind, im Code ausführen mit dem Unterschied, dass die `Binding` Markuperweiterung erfolgt von der `SetBinding` aufrufen und die `Binding` Klasse.

Wenn Sie die datenbindungen in XAML definiert, es gibt jedoch mehrere Möglichkeiten zur Festlegung der `BindingContext` des Zielobjekts. Es ist manchmal festgelegt, aus der Code-Behind-Datei, in einigen Fällen mit einer `StaticResource` oder `x:Static` Markuperweiterung, und gelegentlich auch als Inhalt `BindingContext` Property-Element Tags.

Bindungen sind Verbindung die visuellen Elemente eines Programms mit einem zugrunde liegenden Datenmodell in der Regel in eine Realisierung der Anwendungsarchitektur MVVM (Model-View-ViewModel) am häufigsten verwendet, wie in beschrieben [Teil 5. Aus Datenbindungen, MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), aber andere Szenarien sind möglich.

## <a name="view-to-view-bindings"></a>Bindungen Ansicht

Sie können die datenbindungen zum Verknüpfen der beiden Ansichten auf derselben Seite Eigenschaften definieren. In diesem Fall legen Sie die `BindingContext` der das Ziel mit den `x:Reference` Markuperweiterung.

Hier ist eine XAML-Datei enthält eine `Slider` und zwei `Label` Ansichten, von denen gedreht wird die `Slider` Wert und ein anderes angezeigt der `Slider` Wert:

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

Die `Slider` enthält ein `x:Name` -Attribut, das durch die zwei verwiesen wird `Label` Ansichten mithilfe der `x:Reference` Markuperweiterung.

Die `x:Reference` bindungserweiterung definiert eine Eigenschaft namens `Name` in diesem Fall auf den Namen des das Verweiselement festlegen `slider`. Allerdings die `ReferenceExtension` Klasse, definiert die `x:Reference` Markuperweiterung definiert auch eine `ContentProperty` -Attribut für `Name`, was bedeutet, dass es nicht unbedingt erforderlich ist. Nur für verschiedene das erste `x:Reference` enthält "Name =", die zweite jedoch nicht:

```csharp
BindingContext="{x:Reference Name=slider}"
…
BindingContext="{x:Reference slider}"
```

Die `Binding` Markuperweiterung selbst kann mehrere Eigenschaften aufweisen, ebenso wie die `BindingBase` und `Binding` Klasse. Die `ContentProperty` für `Binding` ist `Path`, aber die "Pfad =" Teil der Markuperweiterung kann ausgelassen werden, wenn der Pfad in das erste Element ist der `Binding` Markuperweiterung. Im ersten Beispiel ist "Pfad =" im zweite Beispiel lässt es aber:

```csharp
Rotation="{Binding Path=Value}"
…
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

Die Eigenschaften können sich auf einer Zeile oder in mehrere Zeilen aufgeteilt:

```csharp
Text="{Binding Value,
               StringFormat='The angle is {0:F0} degrees'}"
```

Führen Sie an, was praktisch ist.

Beachten Sie, dass die `StringFormat` Eigenschaft in der zweiten `Binding` Markuperweiterung. In Xamarin.Forms mit, Bindungen führen keine implizite typkonvertierungen und ein Zeichenfolgenobjekt als Zeichenfolge angezeigt werden sollen. Geben Sie einen Typkonverter oder verwenden Sie `StringFormat`. Im Hintergrund, die statische `String.Format` Methode wird verwendet, implementieren `StringFormat`. Die besteht möglicherweise ein Problem, da .NET Formatierungsangaben geschweifte Klammern umfassen auch zur Begrenzung von Markuperweiterungen verwendet werden. Dadurch wird ein Risiko verwirrend ist die Verwendung von XAML-Parser erstellt. Um dies zu vermeiden, legen Sie die gesamte Formatierungszeichenfolge in einfache Anführungszeichen eingeschlossen:

```csharp
Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
```

So sieht das ausgeführte Programm aus:

[![](data-binding-basics-images/sliderbinding.png "View-View-Bindungen")](data-binding-basics-images/sliderbinding-large.png#lightbox "Ansicht-Bindungen ")

## <a name="the-binding-mode"></a>Den Bindungsmodus

Eine einzige Ansicht kann datenbindungen auf mehreren Eigenschaften haben. Jedoch jede Sicht kann nur einen haben `BindingContext`, sodass mehrere datenbindungen für diese Sicht alle müssen Eigenschaften des gleichen Objekts verweisen.

Die Lösung auf diese und andere Probleme die beinhaltet die `Mode` Eigenschaft, die auf einen Member festgelegt wird die `BindingMode` Enumeration:

- `Default`
- `OneWay` – Werte aus der Quelle an das Ziel übertragen werden
- `OneWayToSource` – Werte vom Ziel zur Quelle übertragen werden
- `TwoWay` – Werte beide Richtungen übertragen werden, zwischen Quelle und Ziel

Das folgende Programm zeigt häufig dazu verwendet die `OneWayToSource` und `TwoWay` Bindungsarten. Vier `Slider` Sichten dienen zur Steuerung der `Scale`, `Rotate`, `RotateX`, und `RotateY` Eigenschaften einer `Label`. Zunächst erscheint es wie diese vier Eigenschaften des der `Label` Datenbindungsfunktionen Ziele sollte sein werden, da jeder von festgelegt wird eine `Slider`. Allerdings die `BindingContext` von `Label` kann nur ein Objekt, und es gibt vier verschiedene Schieberegler.

Aus diesem Grund Alle Bindungen in festgelegt sind scheinbar rückwärts Arten: die `BindingContext` jedes der vier Schieberegler auf festgelegt ist die `Label`, und die Bindungen festgelegt sind die `Value` Eigenschaften die Schieberegler. Mithilfe der `OneWayToSource` und `TwoWay` Modi, diese `Value` Eigenschaften lassen sich die Datenquelleneigenschaften, die die `Scale`, `Rotate`, `RotateX`, und `RotateY` Eigenschaften des der `Label`:

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

Die Bindungen für drei der `Slider` Ansichten sind `OneWayToSource`. Dies bedeutet, die die `Slider` Wert führt dazu, dass es sich bei eine Änderung in der Eigenschaft des seine `BindingContext`, also die `Label` mit dem Namen `label`. Diese drei `Slider` Ansichten dazu führen, dass Änderungen an der `Rotate`, `RotateX`, und `RotateY` Eigenschaften der `Label`.

Allerdings die Bindung für die `Scale` Eigenschaft ist `TwoWay`. Grund hierfür ist, die `Scale` Eigenschaft hat den Standardwert 1 und mit einer `TwoWay` binden Ursachen der `Slider` ursprüngliche Wert auf 1 statt 0 festgelegt werden. Wäre, dass die Bindung `OneWayToSource`, `Scale` würde anfänglich-Eigenschaftensatz auf 0 aus der `Slider` default-Wert. Die `Label` würde nicht sichtbar, und für dem Benutzer eine gewissen Verwirrung verursachen können.

 [![](data-binding-basics-images/slidertransforms.png "Abwärtskompatibilität Bindungen")](data-binding-basics-images/slidertransforms-large.png#lightbox "Abwärtskompatibilität Bindungen")

## <a name="bindings-and-collections"></a>Bindungen und Sammlungen

Nichts veranschaulicht die Leistungsfähigkeit XAML und datenbindungen besser als ein aus einer Vorlage gebildete `ListView`.

`ListView` definiert eine `ItemsSource` Eigenschaft vom Typ `IEnumerable`, werden die Elemente in dieser Sammlung angezeigt. Diese Elemente können Objekte eines beliebigen Typs sein. Standardmäßig `ListView` verwendet die `ToString` Methode jedes Elements dieses Element angezeigt wird. Manchmal ist nur wollten, aber in vielen Fällen `ToString` gibt nur die den vollqualifizierten Klassennamen des Objekts zurück.

Jedoch die Elemente in der `ListView` Auflistung kann angezeigt werden, wie Sie, durch die Verwendung der möchten einen *Vorlage*, dem umfasst einer Klasse, die abgeleitet `Cell`. Die Vorlage wird geklont, für jedes Element in der `ListView`, und datenbindungen, die in der Vorlage festgelegt wurden in den einzelnen Klone übertragen werden.

Sehr häufig, sollten Sie zum Erstellen einer benutzerdefinierten Zelle für diese Elemente mithilfe der `ViewCell` Klasse. Dieser Prozess ist etwas im Code unübersichtlich, aber in XAML wird es sehr einfach.

In der XamlSamples enthalten Projekt ist eine Klasse mit dem Namen `NamedColor`. Jede `NamedColor` Objekt hat `Name` und `FriendlyName` Eigenschaften des Typs `string`, und ein `Color` Eigenschaft vom Typ `Color`. Darüber hinaus `NamedColor` hat 141 statische schreibgeschützte Felder des Typs `Color` , Farben, die in der Xamarin.Forms definiert entspricht `Color` Klasse. Ein statischer Konstruktor erstellt ein `IEnumerable<NamedColor>` Sammlung mit `NamedColor` Objekte für diese statische Felder und die öffentliche statische zugewiesen `All` Eigenschaft.

Festlegen der statischen `NamedColor.All` Eigenschaft, um die `ItemsSource` des eine `ListView` ist ganz einfach mit der `x:Static` Markuperweiterung:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">

    <ListView ItemsSource="{x:Static local:NamedColor.All}" />

</ContentPage>
```

Die resultierenden Anzeige legt fest, dass die Elemente des Typs tatsächlich sind `XamlSamples.NamedColor`:

[![](data-binding-basics-images/listview1.png "Binden an eine Auflistung")](data-binding-basics-images/listview1-large.png#lightbox "Binden an eine Auflistung")

Es ist nicht viele Informationen, aber die `ListView` bildlauffähiges und ausgewählt ist.

Um eine Vorlage für die Elemente zu definieren, sollten Sie gliedern Sie die `ItemTemplate` -Eigenschaft, wie ein Eigenschaftenelement und legen Sie dafür eine `DataTemplate`, welche Verweise ein `ViewCell`. Um die `View` Eigenschaft von der `ViewCell` können Sie ein Layout für eine oder mehrere Ansichten zur Anzeige der einzelnen Elemente zu definieren. Hier ist ein einfaches Beispiel:

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

Die `Label` -Elementgruppe ist die `View` Eigenschaft von der `ViewCell`. (Die `ViewCell.View` Tags sind nicht erforderlich, da die `View` Eigenschaft ist für die Inhaltseigenschaft des `ViewCell`.) Dieses Markup zeigt die `FriendlyName` -Eigenschaft jedes `NamedColor` Objekt:

[![](data-binding-basics-images/listview2.png "Binden an eine Auflistung mit einem DataTemplate")](data-binding-basics-images/listview2-large.png#lightbox "Binden an eine Auflistung mit DataTemplate")

Viel besser. Jetzt ist die einzige erforderliche zum Einrichten der Elementvorlage mit Informationen und die tatsächliche Farbe Fichte. Zur Unterstützung dieser Vorlage haben einige Werte und Objekte in der Seite Ressourcenwörterbuch definiert wurde:

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

Beachten Sie die Verwendung von `OnPlatform` definieren Sie die Größe des eine `BoxView` und die Höhe der `ListView` Zeilen. Obwohl die Werte für alle drei Plattformen identisch sind, konnte auf das Markup problemlos für andere Werte zum Optimieren der Anzeige angepasst werden können.

## <a name="binding-value-converters"></a>Bindung Wertkonverter

Der vorherige **ListView Demo** XAML-Datei zeigt die einzelnen `R`, `G`, und `B` Eigenschaften von der Xamarin.Forms `Color` Struktur. Diese Eigenschaften sind vom Typ `double` und zwischen 0 und 1 liegen. Wenn Sie die hexadezimale Werte anzeigen möchten, einfach können keine `StringFormat` mit einer Formatierung Spezifikation "X2". Das funktioniert nur für ganze Zahlen und neben dem, der `double` Werte müssen durch 255 multipliziert werden soll.

Dieses kleine Problem behoben wurde eine *Wertkonverter*auch Namens eine *Bindung Konverter*. Dies ist eine Klasse, implementiert die `IValueConverter` -Schnittstelle, d. h., sie verfügt über zwei Methoden, die mit dem Namen `Convert` und `ConvertBack`. Die `Convert` Methode wird aufgerufen, wenn ein Wert aus der Quelle zum Ziel übertragen wird die `ConvertBack` -Methode ist für Übertragungen vom Ziel aufgerufen, um die Quelle in `OneWayToSource` oder `TwoWay` Bindungen:

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

Die `ConvertBack` Methode wird eine Rolle nicht an diesem Programm wiedergegeben werden, da die Bindungen nur unidirektionale aus der Quelle zum Ziel sind.

Eine Bindung verweist auf eine Bindung-Konverter, für die `Converter` Eigenschaft. Ein Konverter für die Bindung kann auch akzeptieren einen Parameter angegeben wird, mit der `ConverterParameter` Eigenschaft. Für einige Vielseitigkeit ist dies an, wie der Multiplikator angegeben wird. Der Konverter Bindung überprüft der Konverterparameter für eine gültige `double` Wert.

Der Konverter wird im Ressourcenverzeichnis instanziiert, damit es in mehrere Bindungen verwendet werden kann:

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

[![](data-binding-basics-images/listview3.png "Binden an eine Auflistung mit DataTemplate und Konverter")](data-binding-basics-images/listview3-large.png#lightbox "Binden an eine Auflistung mit DataTemplate und Typkonverter")

Die `ListView` ist ziemlich ausgereift bei der Verarbeitung von Änderungen, die dynamisch in der zugrunde liegenden Daten, aber nur auftreten, wenn Sie bestimmte Schritte ausführen. Wenn die Auflistung von Elementen zugewiesen der `ItemsSource` Eigenschaft von der `ListView` Änderungen während der Laufzeit –, ist, wenn Elemente hinzugefügt werden können, oder aus der Auflistung entfernt – verwenden eine `ObservableCollection` Klasse für diese Elemente. `ObservableCollection` implementiert die `INotifyCollectionChanged` -Schnittstelle, und `ListView` installiert einen Handler für das `CollectionChanged` Ereignis.

Wenn die Eigenschaften der Elemente selbst ändern, während der Laufzeit, und klicken Sie dann die Elemente in der Auflistung implementieren sollten die `INotifyPropertyChanged` Schnittstelle und Signal Änderungen von Eigenschaftswerten, die mit der `PropertyChanged` Ereignis. Dies wird im nächsten Teil dieser Reihe veranschaulicht [Teil 5. Aus dem Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Zusammenfassung

Datenbindungen stellen einen leistungsstarken Mechanismus zum Verknüpfen von Eigenschaften zwischen zwei Objekten innerhalb einer Seite oder zwischen visuellen Elementen und die zugrunde liegenden Daten. Aber bei der Anwendung arbeiten mit Datenquellen beginnt, wird eine verbreitete-Architekturschema als ein nützlich-Paradigma ergibt sich beginnt. Dieser Vorgang wird beschrieben [Teil 5. Aus Datenbindungen, MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).



## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Teil 1. Erste Schritte mit XAML (Beispiel)](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 2. Grundlegende XAML-Syntax (Beispiel)](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 3. Verwendung von XAML-Markuperweiterungen (Beispiel)](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 5. Aus einer Datenbindung zu MVVM (Beispiel)](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
