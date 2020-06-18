---
title: Bindungswertkonverter in Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Werte innerhalb einer Xamarin.Forms-Datenbindung durch Implementierung eines Wertkonverters (auch als Bindungskonverter oder Bindungswertkonverter bekannt) umgewandelt oder konvertiert werden.
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e1a4faabc8f0703b497062a8c5d587221692dab7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139759"
---
# <a name="xamarinforms-binding-value-converters"></a>Bindungswertkonverter in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

Datenbindungen wandeln Daten in der Regel von einer Quelleigenschaft in eine Zieleigenschaft und in einigen Fällen von der Zieleigenschaft in die Quelleigenschaft um. Diese Umwandlung ist einfach, wenn die Quell- und Zieleigenschaften vom gleichen Typ sind oder wenn ein Typ über eine implizite Konvertierung in den anderen Typ konvertiert werden kann. Wenn dies nicht der Fall ist, muss eine Typkonvertierung durchgeführt werden.

Im Artikel [**Formatierung von Zeichenfolgen**](string-formatting.md) wurde erläutert, wie Sie die `StringFormat`-Eigenschaft der Datenbindung zum Konvertieren von Typen in eine Zeichenfolge verwenden können. Für andere Konvertierungstypen müssen Sie speziellen Code in einer Klasse schreiben, die die Schnittstelle [`IValueConverter`](xref:Xamarin.Forms.IValueConverter) implementiert. (Die Universelle Windows-Plattform enthält eine ähnliche Klasse namens [`IValueConverter`](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) im `Windows.UI.Xaml.Data`-Namespace. Jedoch befindet sich diese `IValueConverter`-Klasse im `Xamarin.Forms`-Namespace.) Klassen, die `IValueConverter` implementieren, werden *Wertkonverter* genannt. Sie werden allerdings auch oft als *Bindungskonverter* oder *Bindungswertkonverter* bezeichnet.

## <a name="the-ivalueconverter-interface"></a>Die IValueConverter-Schnittstelle

Angenommen, Sie möchten eine Datenbindung definieren, bei der die Quelleigenschaft vom Typ `int`, die Zieleigenschaft jedoch vom Typ `bool` ist. Sie möchten, dass diese Datenbindung einen `false`-Wert erzeugt, wenn die Integerquelle gleich 0 ist und andernfalls `true`.  

Dies erreichen Sie mit einer Klasse, die die `IValueConverter`-Schnittstelle implementiert:

```csharp
public class IntToBoolConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value != 0;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? 1 : 0;
    }
}
```

Sie legen eine Instanz dieser Klasse auf die Eigenschaft [`Converter`](xref:Xamarin.Forms.Binding.Converter) der `Binding`-Klasse oder auf die Eigenschaft [`Converter`](xref:Xamarin.Forms.Xaml.BindingExtension.Converter) der `Binding`-Markuperweiterung fest. Diese Klasse wird Teil der Datenbindung.

Die `Convert`-Methode wird aufgerufen, wenn Daten von der Quelle zum Ziel in `OneWay`- oder `TwoWay`-Bindungen verschoben werden. Der Parameter `value` ist das Objekt oder der Wert der Quelle der Datenbindung. Die Methode muss einen Wert des Datenbindungszieltyps zurückgeben. Die hier dargestellte Methode wandelt den `value`-Parameter in eine `int`-Eigenschaft um und vergleicht diese dann mit „0“ für einen `bool`-Rückgabewert.

Die `ConvertBack`-Methode wird aufgerufen, wenn Daten vom Ziel zur Quelle in `TwoWay`- oder `OneWayToSource`-Bindungen verschoben werden. `ConvertBack` führt die umgekehrte Konvertierung aus: Diese Methode geht davon aus, dass der `value`-Parameter ein boolescher Wert (`bool`) aus dem Ziel ist und konvertiert diesen in einen `int`-Rückgabewert für die Quelle.

Wenn in der Datenbindung auch eine `StringFormat`-Einstellung enthalten ist, wird der Wertkonverter aufgerufen, bevor das Ergebnis als Zeichenfolge formatiert wird.

Die Seite **Enable Buttons** (Aktivieren von Schaltflächen) im Beispiel [**Data Binding Demos (Demos für die Datenbindung)** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos) veranschaulicht, wie dieser Wertkonverter in einer Datenbindung verwendet wird. Der Konstruktor `IntToBoolConverter` wird im Ressourcenverzeichnis der Seite instanziiert. Anschließend wird mit einer `StaticResource`-Markuperweiterung auf diesen verwiesen, um die Eigenschaft `Converter` in zwei Datenbindungen festzulegen. Es ist üblich, dass Datenkonverter zwischen mehreren Datenbindungen auf der Seite gemeinsam genutzt werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.EnableButtonsPage"
             Title="Enable Buttons">
    <ContentPage.Resources>
        <ResourceDictionary>
            <local:IntToBoolConverter x:Key="intToBool" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <Entry x:Name="entry1"
               Text=""
               Placeholder="enter search term"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Search"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry1},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />

        <Entry x:Name="entry2"
               Text=""
               Placeholder="enter destination"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Submit"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                IsEnabled="{Binding Source={x:Reference entry2},
                                    Path=Text.Length,
                                    Converter={StaticResource intToBool}}" />
    </StackLayout>
</ContentPage>
```

Wenn ein Wertkonverter auf mehreren Seiten Ihrer Anwendung verwendet wird, können Sie ihn im Ressourcenverzeichnis in der Datei **App.xaml** instanziieren.

Auf der Seite **Enable Buttons** (Aktivieren von Schaltflächen) ist eine allgemeine Anforderung dargestellt, wenn eine Schaltfläche (`Button`) einen Vorgang ausführt, die auf dem vom Benutzer in eine `Entry`-Ansicht eingegebenen Text basiert. Wenn nichts in die `Entry`-Ansicht eingegeben wurde, sollte die Schaltfläche (`Button`) deaktiviert werden. Jede `Button`-Klasse enthält eine Datenbindung für ihre `IsEnabled`-Eigenschaft. Die Quelle der Datenbindung ist die `Length`-Eigenschaft der `Text`-Eigenschaft der entsprechenden `Entry`-Ansicht. Wenn diese `Length`-Eigenschaft nicht 0 (null) ist, gibt der Wertkonverter `true` zurück, und die Schaltfläche (`Button`) wird aktiviert:

[![Aktivieren von Schaltflächen](converters-images/enablebuttons-small.png "Aktivieren von Schaltflächen")](converters-images/enablebuttons-large.png#lightbox "Aktivieren von Schaltflächen")

Beachten Sie, dass die `Text`-Eigenschaft in jeder `Entry` mit einer leeren Zeichenfolge initialisiert wird. Die `Text`-Eigenschaft ist standardmäßig `null`, aber die Datenbindung funktioniert in diesem Fall nicht.

Einige Wertkonverter werden speziell für bestimmte Anwendungen geschrieben, andere werden wiederum verallgemeinert. Wenn Sie wissen, dass ein Wertkonverter nur in `OneWay`-Bindungen verwendet wird, kann die `ConvertBack`-Methode einfach `null` zurückgeben.

Die oben dargestellte `Convert`-Methode setzt voraus, dass das `value`-Argument vom Typ `int` ist und der Rückgabewert demnach vom Typ `bool` sein muss. Gleichzeitig geht die `ConvertBack`-Methode davon aus, dass das `value`-Argument vom Typ `bool` und der Rückgabewert `int` ist. Sollte dies nicht der Fall sein, wird eine Laufzeitausnahme ausgelöst.

Sie können Wertkonverter so schreiben, dass sie verallgemeinert werden und mehrere unterschiedliche Datentypen akzeptieren. Die Methoden `Convert` und `ConvertBack` können die Operatoren `as` oder `is` mit dem `value`-Parameter verwenden oder `GetType` für diesen Parameter aufrufen, um dessen Typ zu bestimmen und dann einen entsprechenden Vorgang auszuführen. Der erwartete Typ des Rückgabewerts jeder Methode wird vom `targetType`-Parameter bestimmt. In einigen Fällen werden Wertkonverter mit Datenbindungen verschiedener Zieltypen verwendet. Der Wertkonverter kann das Argument `targetType` verwenden, um eine Konvertierung für den richtigen Typ durchzuführen.

Sollte sich die durchgeführte Konvertierung für unterschiedliche Kulturen unterscheiden, verwenden Sie zu diesem Zweck den `culture`-Parameter. Das Argument `parameter` für `Convert` und `ConvertBack` wird später in diesem Artikel behandelt.

## <a name="binding-converter-properties"></a>Eigenschaften von Bindungskonvertern

Wertkonverterklassen können über Eigenschaften und generische Parameter verfügen. Dieser bestimmte Wertkonverter konvertiert einen booleschen Wert (`bool`) aus der Quelle in ein Objekt des Typs `T` für das Ziel:

```csharp
public class BoolToObjectConverter<T> : IValueConverter
{
    public T TrueObject { set; get; }

    public T FalseObject { set; get; }

    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (bool)value ? TrueObject : FalseObject;
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return ((T)value).Equals(TrueObject);
    }
}
```

Auf der Seite **Switch Indicators** (Umschalten von Indikatoren) ist dargestellt, wie so der Wert einer `Switch`-Ansicht angezeigt werden kann. Obwohl es üblich ist, Wertkonverter als Ressourcen in einem Ressourcenverzeichnis zu instanziieren, wird auf dieser Seite eine Alternative vorgestellt: Jeder Wertkonverter wird zwischen `Binding.Converter`-Eigenschaftselementtags instanziiert. Die Anweisung `x:TypeArguments` gibt das generische Argument an, und `TrueObject` und `FalseObject` werden jeweils auf Objekte dieses Typs festgelegt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SwitchIndicatorsPage"
             Title="Switch Indicators">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>

            <Style TargetType="Switch">
                <Setter Property="VerticalOptions" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Subscribe?" />
            <Switch x:Name="switch1" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch1}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Of course!"
                                                         FalseObject="No way!" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Allow popups?" />
            <Switch x:Name="switch2" />
            <Label>
                <Label.Text>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="x:String"
                                                         TrueObject="Yes"
                                                         FalseObject="No" />
                        </Binding.Converter>
                    </Binding>
                </Label.Text>
                <Label.TextColor>
                    <Binding Source="{x:Reference switch2}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Color"
                                                         TrueObject="Green"
                                                         FalseObject="Red" />
                        </Binding.Converter>
                    </Binding>
                </Label.TextColor>
            </Label>
        </StackLayout>

        <StackLayout Orientation="Horizontal"
                     VerticalOptions="CenterAndExpand">
            <Label Text="Learn more?" />
            <Switch x:Name="switch3" />
            <Label FontSize="18"
                   VerticalOptions="Center">
                <Label.Style>
                    <Binding Source="{x:Reference switch3}"
                             Path="IsToggled">
                        <Binding.Converter>
                            <local:BoolToObjectConverter x:TypeArguments="Style">
                                <local:BoolToObjectConverter.TrueObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Indubitably!" />
                                        <Setter Property="FontAttributes" Value="Italic, Bold" />
                                        <Setter Property="TextColor" Value="Green" />
                                    </Style>                                    
                                </local:BoolToObjectConverter.TrueObject>

                                <local:BoolToObjectConverter.FalseObject>
                                    <Style TargetType="Label">
                                        <Setter Property="Text" Value="Maybe later" />
                                        <Setter Property="FontAttributes" Value="None" />
                                        <Setter Property="TextColor" Value="Red" />
                                    </Style>
                                </local:BoolToObjectConverter.FalseObject>
                            </local:BoolToObjectConverter>
                        </Binding.Converter>
                    </Binding>
                </Label.Style>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

In den letzten drei `Switch`- und `Label`-Paaren wird das generische Argument auf `Style` festgelegt, und die gesamten `Style`-Objekte werden für die Werte `TrueObject` und `FalseObject` bereitgestellt. Diese überschreiben das impliziten Format für die im Ressourcenverzeichnis festgelegte `Label`-Klasse. Deshalb werden die Eigenschaften in diesem Format explizit zur `Label`-Klasse zugewiesen. Das Umschalten von `Switch` bewirkt, dass die entsprechende `Label`-Klasse die Änderung widerspiegelt:

[![Umschalten von Indikatoren](converters-images/switchindicators-small.png "Umschalten von Indikatoren")](converters-images/switchindicators-large.png#lightbox "Umschalten von Indikatoren")

Zum Implementieren von ähnlichen Änderungen auf der Benutzeroberfläche basierend auf anderen Ansichten können Sie ebenso [`Triggers`](~/xamarin-forms/app-fundamentals/triggers.md) verwenden.

## <a name="binding-converter-parameters"></a>Parameter von Bindungskonvertern

Die Klasse `Binding` definiert eine [`ConverterParameter`](xref:Xamarin.Forms.Binding.ConverterParameter)-Eigenschaft, und die Markuperweiterung `Binding` definiert ebenfalls eine [`ConverterParameter`](xref:Xamarin.Forms.Xaml.BindingExtension.ConverterParameter)-Eigenschaft. Wenn diese Eigenschaft festgelegt ist, dann wird der Wert an die Methoden `Convert` und `ConvertBack` als `parameter`-Argument übergeben. Auch wenn die Instanz des Wertkonverters auf mehrere Datenbindungen aufgeteilt wird, kann sich `ConverterParameter` für andere Konvertierungen unterscheiden.

Die Verwendung von `ConverterParameter` wird mit einem Farbauswahlprogramm veranschaulicht. In diesem Fall verfügt `RgbColorViewModel` über drei Eigenschaften vom Typ `double` namens `Red`, `Green` und `Blue`, die verwendet werden, um einen `Color`-Wert zu erstellen:

```csharp
public class RgbColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Red
    {
        set
        {
            if (color.R != value)
            {
                Color = new Color(value, color.G, color.B);
            }
        }
        get
        {
            return color.R;
        }
    }

    public double Green
    {
        set
        {
            if (color.G != value)
            {
                Color = new Color(color.R, value, color.B);
            }
        }
        get
        {
            return color.G;
        }
    }

    public double Blue
    {
        set
        {
            if (color.B != value)
            {
                Color = new Color(color.R, color.G, value);
            }
        }
        get
        {
            return color.B;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Red"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Green"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Blue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

Der Bereich für die Eigenschaften `Red`, `Green` und `Blue` liegt zwischen 0 und 1. Möglicherweise bevorzugen Sie, dass die Komponenten als zweistellige Hexadezimalwerte angezeigt werden.

Damit sie diese als Hexadezimalwerte in XAML anzeigen können, müssen sie mit 255 multipliziert und in einen Integer konvertiert werden. Anschließend müssen Sie mit der Spezifikation „X2“ in die `StringFormat`-Eigenschaft formatiert werden. Die ersten beiden Aufgaben (Multiplikation mit 255 und Konvertieren in einen Integer) können vom Wertkonverter übernommen werden. Sie können den Wertkonverter so allgemein wie möglich gestalten, indem Sie den Multiplikationsfaktor mit der Eigenschaft `ConverterParameter` angeben. Das heißt, dass dieser die `Convert`- und `ConvertBack`-Methode als `parameter`-Argument angibt:

```csharp
public class DoubleToIntConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)Math.Round((double)value * GetParameter(parameter));
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return (int)value / GetParameter(parameter);
    }

    double GetParameter(object parameter)
    {
        if (parameter is double)
            return (double)parameter;

        else if (parameter is int)
            return (int)parameter;

        else if (parameter is string)
            return double.Parse((string)parameter);

        return 1;
    }
}
```

Die `Convert`-Methode wird vom Typ `double` in `int` konvertiert, während eine Multiplikation mit dem `parameter`-Wert durchgeführt wird. `ConvertBack` teilt das ganzzahlige `value`-Argument durch `parameter` und gibt ein `double`-Ergebnis zurück. (Im unten gezeigten Programm wird der Wertkonverter nur in Verbindung mit der Zeichenfolgenformatierung verwendet, also wird `ConvertBack` nicht verwendet.)

Der Typ des `parameter`-Arguments kann leicht variieren, je nachdem, ob die Datenbindung in Code oder XAML definiert ist. Wenn die `ConverterParameter`-Eigenschaft von `Binding` in Code festgelegt ist, dann ist es wahrscheinlich, dass sie auf einen numerischen Wert festgelegt wird:

```csharp
binding.ConverterParameter = 255;
```

Die `ConverterParameter`-Eigenschaft ist vom Typ `Object`, also interpretiert der C#-Compiler das Literal 255 als Integer und legt die Eigenschaft auf diesen Wert fest.

In XAML ist es jedoch wahrscheinlich, dass `ConverterParameter` wie folgt festgelegt wird:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

Das Literal 255 sieht zwar aus wie eine Zahl, da `ConverterParameter` jedoch vom Typ `Object` ist, behandelt der XAML-Parser 255 als Zeichenfolge.

Aus diesem Grund enthält der oben dargestellte Wertkonverter eine separate `GetParameter`-Methode, Fälle behandelt, in denen der `parameter` vom Typ `double`, `int` oder `string` ist.  

Die Seite **RGB Color Selector** (RGB-Farbselektor) instanziiert `DoubleToIntConverter` im Ressourcenverzeichnis, gefolgt von der Definition von zwei impliziten Formaten:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.RgbColorSelectorPage"
             Title="RGB Color Selector">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <local:DoubleToIntConverter x:Key="doubleToInt" />
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:RgbColorViewModel Color="Gray" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Red}" />
            <Label Text="{Binding Red,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Red = {0:X2}'}" />

            <Slider Value="{Binding Green}" />
            <Label Text="{Binding Green,
                                  Converter={StaticResource doubleToInt},
                                  ConverterParameter=255,
                                  StringFormat='Green = {0:X2}'}" />

            <Slider Value="{Binding Blue}" />
            <Label>
                <Label.Text>
                    <Binding Path="Blue"
                             StringFormat="Blue = {0:X2}"
                             Converter="{StaticResource doubleToInt}">
                        <Binding.ConverterParameter>
                            <x:Double>255</x:Double>
                        </Binding.ConverterParameter>
                    </Binding>
                </Label.Text>
            </Label>
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Die Werte der Eigenschaften `Red` und `Green` werden mit einer `Binding`-Markuperweiterung angezeigt. Die `Blue`-Eigenschaft instanziiert jedoch die `Binding`-Klasse, um zu veranschaulichen, wie ein expliziter `double`-Wert auf die `ConverterParameter`-Eigenschaft festgelegt werden kann.

So sieht das Ergebnis aus:

[![RGB-Farbselektor](converters-images/rgbcolorselector-small.png "RGB-Farbselektor")](converters-images/rgbcolorselector-large.png#lightbox "RGB-Farbselektor")

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Zusammenfassung von Kapitel 16.: Datenbindung](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
