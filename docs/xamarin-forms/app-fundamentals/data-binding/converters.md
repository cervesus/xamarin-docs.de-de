---
title: Bindung Wertkonverter
description: Umzuwandeln Sie bzw. zu konvertieren Sie Werte innerhalb der Datenbindung
ms.topic: article
ms.prod: xamarin
ms.assetid: 02B1BBE6-D804-490D-BDD4-8ACED8B70C92
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: aaa4c93eda9edb0eb5d568b3470c02352bdb7467
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="binding-value-converters"></a>Bindung Wertkonverter

Datenbindungen Datenübertragung in der Regel aus einer Quelleigenschaft auf eine Zieleigenschaft und in einigen Fällen von der Zieleigenschaft an die Quelleigenschaft. Diese Übertragung ist einfach, wenn die Quell- und Eigenschaften vom selben Typ sind oder wenn ein Typ in den anderen Typen über eine implizite Konvertierung konvertiert werden kann. Wenn das nicht der Fall ist, muss eine Konvertierung vom Typ stattfinden.

In der [ **Zeichenfolgenformatierung** ](string-formatting.md) Artikel wurde erläutert, wie Sie verwenden können die `StringFormat` -Eigenschaft einer Bindung auf einen beliebigen Typ in eine Zeichenfolge zu konvertieren. Für andere Typen von Konvertierungen, müssen Sie einige speziellen Code in einer Klasse zu schreiben, implementiert die [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) Schnittstelle. (Die universelle Windows-Plattform enthält eine ähnliche Klasse namens [ `IValueConverter` ](/uwp/api/Windows.UI.Xaml.Data.IValueConverter/) in der `Windows.UI.Xaml.Data` -Namespace, aber dies `IValueConverter` befindet sich in der `Xamarin.Forms` Namespace.) Klassen implementiert, `IValueConverter` heißen *Wert Typkonverter*, jedoch werden sie häufig auch bezeichnet als *binden Typkonverter* oder *binden Wertkonverter*.

## <a name="the-ivalueconverter-interface"></a>Die IValueConverter-Schnittstelle

Angenommen, Sie möchten eine Datenbindung definieren, in dem die Quelleigenschaft vom Typ ist `int` die Zieleigenschaft ist jedoch ein `bool`. Soll dieser Datenbindung zum Erzeugen einer `false` Wert, wenn die Quelle für die ganze Zahl gleich 0 ist und `true` andernfalls.  

Hierzu können Sie mit einer Klasse, implementiert die `IValueConverter` Schnittstelle:

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

Festlegen, dass eine Instanz dieser Klasse, die [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Converter/) Eigenschaft der `Binding` Klasse oder die [ `Converter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Converter/) Eigenschaft der `Binding` Markuperweiterung. Diese Klasse wird dem Datenbindung.

Die `Convert` Methode wird aufgerufen, wenn Daten aus der Quelle, die das Ziel verschoben `OneWay` oder `TwoWay` Bindungen. Die `value` Parameter ist das Objekt oder einen Wert aus der Datenbindungsquelle. Die Methode muss einen Wert vom Typ des Ziels Datenbindungsfunktionen zurückgeben. Die Methode, die hier gezeigten Umwandlungen der `value` Parameter an eine `int` und vergleicht ihn dann mit 0 für eine `bool` Rückgabewert.

Die `ConvertBack` Methode wird aufgerufen, wenn Daten aus der Zieldatenbank verschoben werden, mit der Datenquelle im `TwoWay` oder `OneWayToSource` Bindungen. `ConvertBack` führt die umgekehrte Konvertierung: Es wird vorausgesetzt der `value` Parameter ist ein `bool` aus dem Ziel und konvertiert sie in ein `int` Rückgabewert für die Quelle.

Wenn die Datenbindung auch enthält eine `StringFormat` festlegen, der Wertkonverter wird aufgerufen, bevor das Ergebnis als Zeichenfolge formatiert sind.

Die **Schaltflächen aktivieren** auf der Seite der [ **Daten Bindung Demos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Beispiel wird veranschaulicht, wie diese Wertkonverter in einer Bindung verwendet. Die `IntToBoolConverter` in die Seite Ressourcenwörterbuch instanziiert wird. Klicken Sie dann mit verwiesen wird eine `StaticResource` Markuperweiterung festzulegende der `Converter` Eigenschaft zwei datenbindungen. Es ist üblich, Typkonverter von Daten zwischen mehreren datenbindungen auf der Seite freigeben:

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

Wenn ein Wertkonverter auf mehreren Seiten der Anwendung verwendet wird, können Sie es im Ressourcenverzeichnis in Instanziieren der **App.xaml** Datei.

Die **Schaltflächen aktivieren** Seite zeigt eine allgemeine erforderlich, wenn eine `Button` führt einen Vorgang basierend auf Text, der die Benutzertypen in eine `Entry` anzeigen. Wenn nichts in typisiert wurde die `Entry`die `Button` sollte deaktiviert werden. Jede `Button` enthält eine Datenbindung für die `IsEnabled` Eigenschaft. Der Datenbindungsquelle ist die `Length` Eigenschaft von der `Text` -Eigenschaft des entsprechenden `Entry`. Wenn für das `Length` Eigenschaft ist nicht "0", "der Wertkonverter zurück `true` und die `Button` aktiviert ist:

[![Aktivieren Sie die Schaltflächen](converters-images/enablebuttons-small.png "Schaltflächen aktivieren")](converters-images/enablebuttons-large.png "Schaltflächen aktivieren")

Beachten Sie, dass die `Text` Eigenschaft in jedem `Entry` auf eine leere Zeichenfolge initialisiert wird. Die `Text` Eigenschaft `null` standardmäßig, und die Daten Bindung funktioniert nicht in diesem Fall.

Einige Wertkonverter sind speziell für bestimmte Anwendungen geschrieben, während andere generalisiert sind. Wenn Sie wissen, dass ein Wertkonverter in nur verwendet werden, `OneWay` Bindungen, und klicken Sie dann die `ConvertBack` Methode kann einfach zurückkehren `null`.

Die `Convert` implizit oben gezeigten Methode setzt voraus, dass die `value` Argument ist vom Typ `int` und der Rückgabewert muss vom Typ `bool`. Auf ähnliche Weise die `ConvertBack` Methode setzt voraus, dass die `value` Argument ist vom Typ `bool` und der Rückgabewert ist `int`. Wenn das nicht der Fall ist, wird eine Laufzeitausnahme ausgelöst.

Sie können Wertkonverter verallgemeinert werden und akzeptiert verschiedene Typen von Daten schreiben. Die `Convert` und `ConvertBack` Methoden können die `as` oder `is` Operatoren mit den `value` Parameter, oder durch Aufrufen `GetType` geeignet für diesen Parameter, dessen Typ, und führen Sie etwas zu bestimmen. Der erwartete Typ des Rückgabewerts für jede Methode ist gegeben, durch die `targetType` Parameter. In manchen Fällen werden Wertkonverter mit datenbindungen, die von den unterschiedlichen Zieltypen verwendet. der Wertkonverter können die `targetType` Argument beim Ausführen einer Konvertierung für den richtigen Typ.

Wenn die Konvertierung ausgeführt wird für die verschiedenen Kulturen unterschiedlich ist, verwenden die `culture` -Parameter für diesen Zweck. Die `parameter` Argument `Convert` und `ConvertBack` wird weiter unten in diesem Artikel erläutert.

## <a name="binding-converter-properties"></a>Konverter Bindungseigenschaften

Wertklassen der Konverter können es sich um Eigenschaften und generische Parameter aufweisen. Diese bestimmten Wertkonverter konvertiert eine `bool` aus der Quelle in ein Objekt vom Typ `T` für das Ziel:

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

Die **Switch Indikatoren** Seite wird veranschaulicht, wie sie zum Anzeigen des Werts verwendet werden kann ein `Switch` anzeigen. Jedoch Wertkonverter als Ressourcen in einem Ressourcenwörterbuch zu instanziieren, zeigt diese Seite eine Alternative: jeder Wertkonverter zwischen instanziiert wird `Binding.Converter` Property-Element Tags. Die `x:TypeArguments` gibt an, dem generischen Argument und `TrueObject` und `FalseObject` beide auf Objekte dieses Typs festgelegt sind:

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

In den letzten der drei `Switch` und `Label` -Paaren, mit das generische Argument festgelegt ist `Style`, und der gesamte `Style` Objekte werden bereitgestellt, für die Werte der `TrueObject` und `FalseObject`. Diese außer Kraft setzen, die implizite Formatvorlage für `Label` im Ressourcenverzeichnis festgelegt, sodass die Eigenschaften in diesem Format explizit zugewiesen sind die `Label`. Umschalten der `Switch` bewirkt, dass das entsprechende `Label` um die Änderung zu übernehmen:

[![Wechseln Sie Indikatoren](converters-images/switchindicators-small.png "wechseln Indikatoren")](converters-images/switchindicators-large.png "Indikatoren wechseln")

Es ist auch möglich, verwenden Sie [ `Triggers` ](~/xamarin-forms/app-fundamentals/triggers.md) ähnliche Änderungen in der Benutzeroberfläche, basierend auf andere Sichten implementiert.

## <a name="binding-converter-parameters"></a>Konverter Bindungsparameter

Die `Binding` Klasse definiert ein [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.ConverterParameter/) -Eigenschaft, und die `Binding` Markuperweiterung definiert auch einen [ `ConverterParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.ConverterParameter/) Eigenschaft. Wenn diese Eigenschaft festgelegt ist, wird der Wert wird zum Übergeben der `Convert` und `ConvertBack` Methoden als der `parameter` Argument. Auch wenn die Instanz von der Wertkonverter von mehreren datenbindungen, gemeinsam genutzt wird die `ConverterParameter` für etwas unterschiedlichen Konvertierungen verschieden sein können.

Die Verwendung von `ConverterParameter` wird ein Farbauswahl Programm veranschaulicht. In diesem Fall die `RgbColorViewModel` verfügt über drei Eigenschaften des Typs `double` mit dem Namen `Red`, `Green`, und `Blue` , die verwendet wird, zum Erstellen einer `Color` Wert:

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

Die `Red`, `Green`, und `Blue` Eigenschaften Bereich zwischen 0 und 1. Allerdings empfiehlt es sich, dass die Komponenten als zweistelligen Hexadezimalwerten angezeigt werden.

Um diese als hexadezimale Werte in XAML anzuzeigen, sie müssen 255 multipliziert, in eine ganze Zahl konvertiert und dann mit einer Spezifikation des "X2" formatiert sind, der `StringFormat` Eigenschaft. Die ersten beiden Aufgaben (255 multipliziert, und die Konvertierung in eine ganze Zahl), die von der Wertkonverter verarbeitet werden können. Um der Wertkonverter wie generalisiert wie möglich zu machen, den Multiplikationsfaktor angegeben werden kann, mit der `ConverterParameter` -Eigenschaft, d. h. er wird die `Convert` und `ConvertBack` Methoden als der `parameter` Argument:

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

Die `Convert` konvertiert aus einer `double` auf `int` beim Multiplikation mit der `parameter` Wert; die `ConvertBack` teilt die ganze Zahl `value` Argument nach `parameter` und gibt eine `double` Ergebnis. (Im unten gezeigten Programm der Wertkonverter dient nur im Zusammenhang mit die zeichenfolgenformatierung, also `ConvertBack` wird nicht verwendet.)

Der Typ des der `parameter` Argument ist wahrscheinlich variieren, je nachdem, ob die Datenbindung in Code oder in XAML definiert ist. Wenn die `ConverterParameter` Eigenschaft `Binding` festgelegt ist im Code, es ist wahrscheinlich auf einen numerischen Wert festgelegt werden:

```csharp
binding.ConverterParameter = 255;
```

Die `ConverterParameter` -Eigenschaft ist vom Typ `Object`, sodass der C#-Compiler das literal 255 als ganze Zahl interpretiert, und legt die Eigenschaft auf diesen Wert fest.

In XAML wird jedoch die `ConverterParameter` ist es wahrscheinlich, dass wie folgt festgelegt werden:

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource doubleToInt},
                      ConverterParameter=255,
                      StringFormat='Red = {0:X2}'}" />
```

Die 255 sieht wie eine Zahl ist, aber, da `ConverterParameter` ist vom Typ `Object`, der XAML-Parser behandelt die 255 als Zeichenfolge.

Aus diesem Grund enthält der oben gezeigte Wertkonverter eine Separate `GetParameter` Methode, die zu den Einsatzgebieten von behandelt `parameter` Typ `double`, `int`, oder `string`.  

Die **RGB-Farbauswahl** Seite instanziiert `DoubleToIntConverter` in seiner Ressourcenwörterbuch, befolgen die Definition von zwei impliziten Stilen:

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

Die Werte der `Red` und `Green` Eigenschaften werden angezeigt, mit einem `Binding` Markuperweiterung. Die `Blue` Eigenschaft instanziiert jedoch die `Binding` Klasse zu veranschaulichen, wie eine explizite `double` Wert kann festgelegt werden, um `ConverterParameter` Eigenschaft.

Hier ist das Ergebnis:

[![RGB-Farbauswahl](converters-images/rgbcolorselector-small.png "RGB-Farbauswahl")](converters-images/rgbcolorselector-large.png "RGB-Farbauswahl")


## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Daten Bindung Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
